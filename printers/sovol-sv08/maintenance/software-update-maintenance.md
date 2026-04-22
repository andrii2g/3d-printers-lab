# Software Update Maintenance

Short maintenance guide for updating software on the Sovol SV08.

This document is written for `stock SV08 firmware`.

## Rule for stock SV08

- Do not blindly run `Update All`
- Do not update `Klipper` or `Moonraker` just to remove UI warnings
- Prefer official Sovol firmware packages for core software updates
- Treat `Klipper` and `Moonraker` as high-risk components on stock firmware
- Treat `Mainsail`, `Obico`, and similar add-ons as lower risk, but still back up first

## 1. Confirm current state

- Check printer is idle and not printing
- Check web UI opens normally
- Check no active heater, MCU, or probe error is present
- Check whether the printer still looks like stock SV08 firmware

Useful checks:

```bash
hostname
whoami
git -C ~/klipper remote -v
git -C ~/moonraker remote -v
git -C ~/mainsail remote -v
git -C ~/klipper status --short
git -C ~/moonraker status --short
```

Expected:

- Host looks like `SPI-XI`
- User is `sovol`
- `klipper` and `moonraker` may contain local changes
- `mainsail` may point to a `Sovol3d` source

## 2. Back up before any change

- Back up config files
- Back up service-specific repos before destructive cleanup
- Back up the printer state before firmware-level changes

Useful commands:

```bash
cp ~/printer_data/config/moonraker.conf ~/printer_data/config/moonraker.conf.backup-$(date +%F-%H%M)
cp ~/printer_data/config/printer.cfg ~/printer_data/config/printer.cfg.backup-$(date +%F-%H%M)
cp -r ~/printer_data/config ~/printer_data/config-backup-$(date +%F-%H%M)
```

Optional repo backups:

```bash
cp -r ~/moonraker ~/moonraker-backup-$(date +%F-%H%M)
cp -r ~/klipper ~/klipper-backup-$(date +%F-%H%M)
cp -r ~/moonraker-obico ~/moonraker-obico-backup-$(date +%F-%H%M)
```

Expected:

- You have a rollback point before modifying anything

## 3. Decide what kind of update you are doing

### Safe or lower-risk changes

- `Mainsail` frontend update
- `moonraker-timelapse` install or repair
- `Obico` repair or repo cleanup
- small config cleanup in `moonraker.conf`

### High-risk changes on stock SV08

- `Klipper` update
- `Moonraker` update
- `KIAUH Update All`
- broad `apt upgrade`
- anything that may require MCU reflash or vendor patch compatibility

## 4. Update order

Use this order if you really need to touch multiple components:

1. backup configs
2. verify network and SSH
3. fix broken `apt` sources only if an installer requires it
4. update or repair add-ons first
5. update frontend only if needed
6. leave `Klipper` and `Moonraker` alone unless you are using an official Sovol package or a planned mainline migration
7. run verification after every single change

## 5. Maintenance steps by component

### Mainsail

- Risk: low to medium
- Purpose: web UI only
- Safe action: update only if UI features are needed

Checks:

```bash
grep -R "2.10.0\\|2.10.1" /usr/share/mainsail 2>/dev/null | head -20
```

Expected:

- UI loads after update
- hard refresh may be needed in the browser
- version label may stay old if the vendor package metadata is inconsistent

### Moonraker

- Risk: high on stock SV08
- Safe action: do not update through KIAUH just to clear warnings
- Preferred path: official Sovol package only

Checks:

```bash
git -C ~/moonraker status
git -C ~/moonraker diff --stat
```

Expected:

- dirty repo usually means vendor modifications are present
- if repo is dirty, stop and do not force-update on stock firmware

### Klipper

- Risk: very high on stock SV08
- Safe action: do not update through KIAUH unless you are intentionally leaving stock
- Preferred path: official Sovol firmware package only

Checks:

```bash
git -C ~/klipper status
git -C ~/klipper diff --stat
```

Expected:

- dirty repo usually means vendor modifications or generated files are present
- if update requires MCU changes, do not continue casually

### Obico

- Risk: medium
- Safe action: back up repo, then restore or update the plugin repo if needed

Useful checks:

```bash
git -C ~/moonraker-obico status
sudo systemctl status moonraker-obico
sudo journalctl -u moonraker-obico -n 50 --no-pager
```

Expected:

- repo is clean after restore
- service runs without Python import errors

### moonraker-timelapse

- Risk: low to medium
- Safe action: install plugin rather than forcing a Moonraker update

Useful checks:

```bash
grep -n "\\[timelapse\\]" -A10 ~/printer_data/config/moonraker.conf
grep -i timelapse -n ~/printer_data/logs/moonraker.log | tail -30
```

Expected:

- no `ModuleNotFoundError` for `moonraker.components.timelapse`
- no unparsed `[timelapse]` warning after install and restart

## 6. If a repo is dirty

Use this only for add-ons or non-core repos unless you know exactly what you are doing.

Inspect first:

```bash
git status
git diff --stat
```

If you want to restore the repo completely:

```bash
git fetch origin
git reset --hard origin/master
git clean -fd
```

Expected:

- repo exactly matches remote
- local untracked files are removed

Warning:

- reasonable for `moonraker-obico`
- not a casual fix for stock `klipper` or `moonraker`

## 7. If an installer fails because of `apt`

Check source files:

```bash
grep -R "bullseye-backports" /etc/apt/sources.list /etc/apt/sources.list.d 2>/dev/null
```

Back up APT config first:

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup-$(date +%F-%H%M)
sudo cp -r /etc/apt/sources.list.d /etc/apt/sources.list.d.backup-$(date +%F-%H%M)
```

Expected:

- remove or disable dead repository entries only as needed
- `sudo apt update` should work again

## 8. Verification after every change

Run these after each update or repair:

```bash
sudo systemctl status moonraker
sudo systemctl status klipper
sudo systemctl status moonraker-obico
grep -i "error\\|traceback\\|warning" -n ~/printer_data/logs/moonraker.log | tail -50
```

Expected:

- services are `active (running)` or intentionally disabled
- no new import errors
- web UI opens by printer IP
- `G28` works after core software or firmware changes

## 9. Final validation

- Open the web UI
- Confirm webcam, temperatures, and controls work
- Run `G28`
- Run a small known-good test print
- Watch the first layer

Expected:

- printer behaves the same or better than before the change

## 10. Recommended rule of thumb

- Stock SV08 working correctly: leave `Klipper` and `Moonraker` alone
- Missing feature in UI: update `Mainsail` carefully
- Plugin missing: install or repair the plugin, not the whole stack
- Core software warning only: document it first, update only if there is a confirmed fix path
