# Advanced User Calibration Checklist

Short order-of-operations checklist for Sovol SV08.

Rule:

- Run the next step only if the previous result is correct

## 1. `G28`

- Do: home all axes
- Expect: clean X/Y/Z homing, no probe or motion errors
- Next: `QUAD_GANTRY_LEVEL`

## 2. `QUAD_GANTRY_LEVEL`

- Do: level gantry after homing
- Expect: successful routine, repeatable result, no probe failures
- Next: `BED_MESH_CALIBRATE`

## 3. `BED_MESH_CALIBRATE`

- Do: generate fresh bed mesh
- Expect: full mesh completes, no obviously bad points
- Next: first-layer test and Z offset check

## 4. First-layer test and Z offset

- Do: print a first-layer pattern and trim Z in small steps
- Expect: smooth joined lines, even squish, no drag, no gaps
- Next: material tests

## 5. Material tests

- Do: temperature tower, flow test, stringing test, optional dimension test
- Expect: stable temp choice, correct walls, low stringing, repeatable dimensions
- Next: optional `SHAPER_CALIBRATE` or optional `PID_CALIBRATE ...`

## 6. Optional `SHAPER_CALIBRATE`

- Do: run resonance tuning if hardware changed or ringing is visible
- Expect: valid shaper recommendation, no sensor or motion errors
- Next: optional `PID_CALIBRATE ...` or `SAVE_CONFIG`

## 7. Optional `PID_CALIBRATE ...`

- Do: tune extruder and bed heaters if thermal hardware changed
- Commands:
  - `PID_CALIBRATE HEATER=extruder TARGET=220`
  - `PID_CALIBRATE HEATER=heater_bed TARGET=60`
- Expect: stable targets, successful tune output
- Next: `SAVE_CONFIG`

## 8. `SAVE_CONFIG`

- Do: save only confirmed values
- Expect: clean save, normal restart, values persist after reconnect
- Next: final validation print

## 9. Final validation print

- Do: print one known-good model with the target profile
- Expect: good first layer, even walls, clean corners, closed top layers, acceptable dimensions
- Finish: calibration complete