# Basic User Calibration Checklist

Calibration flow for Sovol SV08 in the exact working order.

Rule:

- Do not continue to the next step until the expected result is correct
- If a step fails, fix the issue first and repeat the same step

## 1. `G28`

Purpose:

- Home all axes and establish the printer reference position

How to do it:

- Heat the printer to normal working temperature first
- Open the Klipper console
- Run `G28`

Expected result:

- X, Y, and Z home without errors
- No unusual grinding, skipping, or probe error appears
- Toolhead movement looks normal and controlled

If OK:

- Continue to `QUAD_GANTRY_LEVEL`

If not OK:

- Check endstops, probe state, cable routing, and free axis movement
- Repeat `G28`

## 2. `QUAD_GANTRY_LEVEL`

Purpose:

- Align the gantry before bed mesh and first-layer tuning

How to do it:

- After successful homing, run `QUAD_GANTRY_LEVEL`
- Wait until the full routine finishes
- If your setup requires it, run `G28` once more after gantry leveling

Expected result:

- The routine finishes without stopping on a probe error
- Z adjustments get smaller if you repeat the command
- The gantry becomes stable and repeatable

If OK:

- Continue to `BED_MESH_CALIBRATE`

If not OK:

- Check probe reliability, bed clearance, gantry mechanics, and loose parts
- Repeat `QUAD_GANTRY_LEVEL`

## 3. `BED_MESH_CALIBRATE`

Purpose:

- Measure the bed surface after gantry alignment

How to do it:

- Run `BED_MESH_CALIBRATE`
- Let the probe finish the full mesh
- Save or load the mesh only if the result looks sane in the interface

Expected result:

- Mesh completes without probe failure
- No corner or area looks clearly wrong compared with the rest
- The measured surface looks believable and repeatable

If OK:

- Continue to the first-layer test and Z adjustment

If not OK:

- Clean the bed, check the probe, confirm the plate is seated correctly, and repeat the mesh

## 4. First-layer test and Z adjustment

Purpose:

- Set the real nozzle-to-bed gap for reliable adhesion

How to do it:

- Start a simple first-layer test print
- Watch the first layer closely
- Adjust Z offset in very small steps from the screen or interface while printing

Expected result:

- Lines are smooth, slightly squished, and joined together
- Surface looks even across the bed
- No dragging, tearing, or gaps between lines

If nozzle is too high:

- Lines look round
- Lines do not join well
- Adhesion is weak

If nozzle is too low:

- Surface looks rough or over-squished
- Nozzle drags through the plastic
- Extra material pushes out to the sides

If OK:

- Continue to material test prints

If not OK:

- Adjust Z offset and repeat the first-layer test until the result is stable

## 5. Material test prints

Purpose:

- Tune the profile for the exact filament and nozzle in use

How to do it:

- Run a temperature tower first
- Then run a flow or single-wall test
- Then run a stringing or retraction test
- If needed, run a small dimensional test cube

Expected result:

- Temperature tower shows one clearly better zone
- Flow test matches expected wall thickness closely
- Stringing is low without under-extrusion
- Dimensional error is small and repeatable

If OK:

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

- Continue to optional `PID_CALIBRATE ...` or `SAVE_CONFIG`

If not OK:

- Check accelerometer setup and mounting, then repeat only if needed

## 7. Optional `PID_CALIBRATE ...`

Purpose:

- Tune heater control after heater, thermistor, hotend, or bed changes

How to do it:

- Run `PID_CALIBRATE HEATER=extruder TARGET=220`
- Run `PID_CALIBRATE HEATER=heater_bed TARGET=60`
- Use targets that match your common materials if needed

Expected result:

- Each heater reaches the target without error
- Temperature stays controlled instead of hunting too much
- Klipper reports successful tuning values

If OK:

- Continue to `SAVE_CONFIG`

If not OK:

- Check heater wiring, thermistor readings, and cooling conditions before retrying

## 8. `SAVE_CONFIG`

Purpose:

- Store only the values that have already been confirmed

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

## 9. Final validation print

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