# Software Update and Upgrade

Short guide for updating Sovol SV08 software.

Important:

- Stock SV08 firmware is customized by Sovol
- On stock firmware, do not blindly update core components from Mainsail, KIAUH, or `apt upgrade`
- Community reports show that direct updates can break Klipper, Moonraker, the screen, webcam, or MCU communication
- If the printer works normally, a Mainsail dependency warning alone is not enough reason to force an update

## When you see a warning like `Dependency: Moonraker`

- Meaning: your installed Moonraker version is older than the Mainsail UI expects
- If printer works: you can continue printing and plan the update later
- If you need the newer features: prefer an official Sovol firmware update, not a random in-place package update from Mainsail

## Safe update path for stock SV08

## 1. Check current state

- Do: open the web UI and confirm the printer connects normally
- Check: no MCU error, no heater error, no probe error
- Expected: printer is operational before you start any update

## 2. Back up your working config

- Do: download or copy `printer.cfg` and any custom macros or profiles
- Check: keep a copy on your PC before changing software
- Expected: you can restore your settings if the update goes wrong

## 3. Prefer official Sovol firmware package

- Do: look for the latest official SV08 firmware package from Sovol
- Check: confirm it is for `SV08`, not another model
- Expected: package comes from official Sovol source or official download link

## 4. Update using the official Sovol method

- Do: follow the Sovol package or firmware instructions exactly
- Usually: this means using the provided package and the printer's supported update flow
- Expected: printer reboots cleanly and comes back online after the update

## 5. Verify after update

- Do: open the web UI again by printer IP
- Check: printer connects, camera works if used, screen works, no MCU errors appear
- Run: `G28`
- Expected: normal homing with no new errors

## 6. Run one test print

- Do: run a small known-good print
- Check: first layer, motion, temperatures, and UI behavior
- Expected: printer behaves the same or better than before the update

## What not to do on stock SV08

- Do not run `apt upgrade` for the whole system unless you are prepared to recover the printer
- Do not use Mainsail `Update All` on stock firmware just because a warning appears
- Do not update Klipper, Moonraker, or system packages blindly through KIAUH on stock firmware
- Do not update right before an important print job

## If you are on a mainline or custom Klipper setup

- Different rules apply
- In that case, Mainsail and Moonraker updates may be normal, but MCU compatibility must still be checked
- If Klipper updates require MCU reflashing, do not update until you are ready to do that work

## Recommended decision for basic users

- If the printer prints correctly now: leave it as is
- Update only when Sovol provides an official SV08 firmware package that clearly includes the fix you want
- After the update, test homing and one small print before normal use