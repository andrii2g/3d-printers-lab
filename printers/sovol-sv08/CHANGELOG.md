# Changelog

All notable documentation changes for `printers/sovol-sv08/` are recorded here.


## 2026-04-23

### Aligned docs with stock macros

- Reviewed [Macro.cfg](./configs/Macro.cfg) and updated the calibration docs to note that stock [`START_PRINT`](./configs/Macro.cfg) already handles `QUAD_GANTRY_LEVEL` when needed and runs `BED_MESH_CALIBRATE ADAPTIVE=1`.
- Removed manual `QUAD_GANTRY_LEVEL` and manual `BED_MESH_CALIBRATE` as required standalone calibration steps for normal stock-SV08 use.
- Kept first-layer and Z-offset checks as the main manual print-quality validation step.

### Updated calibration workflow

- Updated [Basic User Calibration Checklist](./calibration/step-by-step-calibration.md) to reflect the stock SV08 workflow more accurately.
- Updated [Advanced User Calibration Checklist](./calibration/advanced-user-calibration-checklist.md) with the same stock-macro-aware order of operations.
- Replaced the earlier generic temperature-calibration step with optional [`PID_CALIBRATE`](./calibration/step-by-step-calibration.md) guidance placed early in the flow after `G28`.
- Added guidance that `PID_CALIBRATE` targets should match filament temperature requirements, with `220/60` documented as a reasonable first-start baseline.
- Added notes that `PID_CALIBRATE` may be re-run after hardware changes, temperature instability, or when moving to filaments with clearly different temperature ranges.


### Added save guidance

- Updated both calibration checklists to distinguish between slicer-side changes and printer-side values that should be stored with `SAVE_CONFIG`.

## 2026-04-22


### Added maintenance workflow

- Added [Software Update Maintenance](./maintenance/software-update-maintenance.md) with backup steps, update order, per-component maintenance guidance, dirty-repo recovery notes, and post-update verification steps.

### Added calibration guides

- Added [Basic User Calibration Checklist](./calibration/step-by-step-calibration.md) with step-by-step calibration flow, commands, expected results, and next actions.
- Added [Advanced User Calibration Checklist](./calibration/advanced-user-calibration-checklist.md) with a compact order-of-operations checklist for experienced users.
- Updated [calibration/README.md](./calibration/README.md) to link both calibration guides.

### Added startup and access notes

- Added [Startup Checklist](./notes/startup-checklist.md) for short pre-print preparation steps.
- Added [First Start and Network Access](./notes/first-start-and-network-access.md) for Wi-Fi/Ethernet setup, IP lookup, and opening the web UI.
- Added [Terminal Access](./notes/terminal-access.md) with SSH login details and common first commands.



### Updated notes index

- Updated [notes/README.md](./notes/README.md) to link the new startup, network, update, and terminal-access documents.

