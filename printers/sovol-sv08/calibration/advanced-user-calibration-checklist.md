# Advanced User Calibration Checklist

Short order-of-operations checklist for Sovol SV08.

Rule:

- Run the next step only if the previous result is correct

## 1. `G28`

- Do: home all axes
- Preheat: bed to target print temp, nozzle to a warm cleanup/stable temp such as `140-150C`
- Note: stock SV08 macros may use their own leveling temps, observed as bed `80C` and nozzle `130C`
- Expect: clean X/Y/Z homing, no probe or motion errors
- Next: `QUAD_GANTRY_LEVEL`

## 2. Optional `PID_CALIBRATE ...`

- Do: tune extruder and bed heaters early if first setup, after thermal hardware changes, when switching to a filament with clearly different temperature requirements, or if temps are unstable
- Commands:
  - `PID_CALIBRATE HEATER=extruder TARGET=220`
  - `PID_CALIBRATE HEATER=heater_bed TARGET=60`
- Note: choose targets to match the filament working range; `220/60` is only an example
- Note: for the very first startup, `220/60` is a reasonable baseline; re-run later with final filament temperatures if needed
- Save: run `SAVE_CONFIG`
- Expect: stable targets, successful tune output
- Next: `QUAD_GANTRY_LEVEL`

## 3. Automatic gantry leveling and bed mesh

- Do: on stock SV08, let `START_PRINT` handle `QUAD_GANTRY_LEVEL` and `BED_MESH_CALIBRATE ADAPTIVE=1`
- Note: manual QGL and bed mesh are mainly for troubleshooting or validation
- Expect: start macro completes leveling and adaptive mesh without probe errors
- Next: first-layer test and Z offset check

## 4. First-layer test and Z offset

- Do: print a first-layer pattern and trim Z in small steps
- Save: run `SAVE_CONFIG` if the final Z offset is written on the printer side
- Expect: smooth joined lines, even squish, no drag, no gaps
- Next: material tests

## 5. Material tests

- Do: flow test, stringing test, optional dimension test
- Save: slicer profile changes after each confirmed result
- Expect: correct walls, low stringing, repeatable dimensions
- Next: optional `SHAPER_CALIBRATE` or optional `PID_CALIBRATE ...`

## 6. Optional `SHAPER_CALIBRATE`

- Do: run resonance tuning if hardware changed or ringing is visible
- Save: run `SAVE_CONFIG` if you keep the suggested shaper values
- Expect: valid shaper recommendation, no sensor or motion errors
- Next: optional `PID_CALIBRATE ...` or `SAVE_CONFIG`

## 7. `SAVE_CONFIG`

- Do: save only confirmed values
- Expect: clean save, normal restart, values persist after reconnect
- Next: final validation print

## 8. Final validation print

- Do: print one known-good model with the target profile
- Expect: good first layer, even walls, clean corners, closed top layers, acceptable dimensions
- Finish: calibration complete
