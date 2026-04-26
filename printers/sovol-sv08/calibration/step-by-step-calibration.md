# Basic User Calibration Checklist

Calibration flow for Sovol SV08 in the exact working order.

Rule:

- Do not continue to the next step until the expected result is correct
- If a step fails, fix the issue first and repeat the same step

## 1. `G28`

Purpose:

- Home all axes and establish the printer reference position

How to do it:

- Preheat the bed to the target print temperature first
- Preheat the nozzle enough to soften residue and reach a stable state
- For gantry leveling, full print nozzle temperature is not required; `140-150C` is usually enough if filament residue can be cleaned safely
- On stock SV08, `QUAD_GANTRY_LEVEL` may apply its own leveling temperatures; observed behavior is bed `80C` and nozzle `130C`
- Open the Klipper console
- Run `G28`

Expected result:

- X, Y, and Z home without errors
- No unusual grinding, skipping, or probe error appears
- Toolhead movement looks normal and controlled

## 2. Optional `PID_CALIBRATE ...`

Purpose:

- Tune heater control early in the setup process so nozzle and bed temperatures stay stable

How to do it:

- Run `PID_CALIBRATE HEATER=extruder TARGET=220`
- Run `PID_CALIBRATE HEATER=heater_bed TARGET=60`
- Use targets that match the filament you are calibrating for
- Example only: PLA may be around `220/60`, while PETG, ABS, ASA, or TPU may require different nozzle and bed targets
- For the very first startup, using `220/60` is a reasonable baseline if the final filament-specific temperatures are not chosen yet
- After filament temperature calibration is completed later, repeat `PID_CALIBRATE` with the real working temperatures if they are meaningfully different
- Run this on first setup, after hotend/bed/thermistor changes, when switching to filament with very different temperature requirements, or if temperature control starts drifting

Expected result:

- Each heater reaches the target without error
- Temperature holds steady instead of hunting too much
- Klipper reports successful tuning values

If OK:

- Run `SAVE_CONFIG`
- Continue to `QUAD_GANTRY_LEVEL`

If not OK:

- Check heater wiring, thermistor readings, cooling conditions, and repeat only after fixing the issue

Recurring maintenance:

- Not usually needed every month on a stable printer
- Re-run after hardware changes, heater instability, or clear temperature overshoot/undershoot
- Re-run when moving to a filament family with clearly different nozzle or bed temperatures, for example PLA to ABS/ASA or TPU to PETG if you want the heater tuning to match the new working range
- Optional periodic check every few months is reasonable if you want to verify stability

## 3. Automatic gantry leveling and bed mesh

Purpose:

- Use the stock SV08 print macros that already handle gantry leveling and adaptive bed mesh

How to do it:

- On stock SV08, `START_PRINT` already runs `QUAD_GANTRY_LEVEL` if needed
- The same print-start flow also runs `BED_MESH_CALIBRATE ADAPTIVE=1`
- The wrapped macros already manage the heating and homing behavior for these steps
- For normal use, you do not need to run `QUAD_GANTRY_LEVEL` and `BED_MESH_CALIBRATE` manually every time
- Run them manually only for troubleshooting, validation, or when checking the macros directly

Expected result:

- Print start completes the automatic leveling sequence without probe errors
- Gantry leveling applies correctly
- Adaptive bed mesh is generated as part of the start sequence

If OK:

- Continue to the first-layer test and Z adjustment

If not OK:

- Check probe reliability, bed clearance, gantry mechanics, and the stock macros in `Macro.cfg`
- If needed, run `QUAD_GANTRY_LEVEL` or `BED_MESH_CALIBRATE` manually for diagnosis

## 4. First-layer test and Z adjustment

Purpose:

- Set the initial nozzle-to-bed gap, then confirm it with a real first-layer print

How to do it:

- In the console, run `PROBE_CALIBRATE`
- Let Klipper move into calibration mode
- Use the paper method or, better, a `0.10 mm` feeler gauge
- Move the nozzle down slowly until the paper or feeler gauge has light friction
- Use small adjustment steps such as:
  - `TESTZ Z=-0.1`
  - `TESTZ Z=-0.05`
  - `TESTZ Z=-0.01`
  - `TESTZ Z=0.01`
- When the feel is correct, run:
  - `ACCEPT`
  - `SAVE_CONFIG`
- Let the printer restart after `SAVE_CONFIG`
- Then start a simple first-layer test print and watch the first layer closely

Expected result:

- Paper method: acceptable
- `0.10 mm` feeler gauge: better and more repeatable
- Correct feel: the paper or gauge still moves, but with light drag
- Lines are smooth, slightly squished, and joined together
- Surface looks even across the bed
- No dragging, tearing, or gaps between lines

If nozzle is too high:

- Paper or gauge moves too freely with almost no resistance
- Lines look round
- Lines do not join well
- Adhesion is weak

If nozzle is too low:

- Paper or gauge is pinned, scratched, or hard to move
- Surface looks rough or over-squished
- Nozzle drags through the plastic
- Extra material pushes out to the sides

If OK:

- If you changed printer-side stored offset values, run `SAVE_CONFIG`
- Continue to material test prints

If not OK:

- Adjust Z offset and repeat the process until the result is stable

Important:

- `PROBE_CALIBRATE` only sets the starting value
- The real confirmation is the first-layer print

## 5. Material test prints

Purpose:

- Tune the profile for the exact filament and nozzle in use

How to do it:

- Run a flow or single-wall test
- Then run a stringing or retraction test
- If needed, run a small dimensional test cube

Expected result:

- Flow test matches expected wall thickness closely
- Stringing is low without under-extrusion
- Dimensional error is small and repeatable

If OK:

- Save slicer-side changes in the profile
- Continue to optional `SHAPER_CALIBRATE` if motion tuning is needed
- Otherwise continue to optional `PID_CALIBRATE` or directly to `SAVE_CONFIG`

If not OK:

- Change only one setting at a time and repeat the relevant test

## 6. Optional `SHAPER_CALIBRATE`

Purpose:

- Tune resonance after hardware changes or visible ringing

How to do it:

- Run `SHAPER_CALIBRATE`
- Wait for Klipper to finish the test and suggest values
- Review the suggested shaper settings before saving anything

Expected result:

- The command completes without sensor or motion errors
- Klipper returns usable shaper recommendations
- Ringing should improve on the next validation print

If OK:

- Run `SAVE_CONFIG` if you want to keep the new shaper values
- Continue to optional `PID_CALIBRATE ...` or `SAVE_CONFIG`

If not OK:

- Check accelerometer setup and mounting, then repeat only if needed

## 7. `SAVE_CONFIG`

Purpose:

- Store any confirmed printer-side values that were changed during the previous steps

How to do it:

- Run `SAVE_CONFIG`
- Let Klipper write the values and restart if requested

Expected result:

- Config saves without error
- Printer restarts normally
- The saved values remain present after reconnecting

If OK:

- Continue to the final validation print

If not OK:

- Review config write permissions and any config syntax problems

## 8. Final validation print

Purpose:

- Confirm that the full calibration sequence works in a real print

How to do it:

- Print one known model with the calibrated profile
- Watch the first layer
- Inspect walls, corners, top layers, and dimensions after the print finishes

Expected result:

- First layer sticks well
- Walls are even
- Corners are clean
- Top layers close properly
- Print quality is repeatable

If OK:

- Calibration is complete

If not OK:

- Return only to the failed area: Z offset, temperature, flow, retraction, or motion tuning
