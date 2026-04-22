# First Layer Large-Print Heat Soak

Short note based on Sovol guidance for improving SV08 first-layer behavior on large prints.

## Main idea

- Large first-layer problems can come from bed shape changing as the bed heats up
- This matters more on large-area prints than on small ones
- Sovol recommends firmware `2.3.3+` for this workflow

## Recommended workflow for large prints

- Preheat the bed to `65C`
- Let it soak for `30 minutes`
- Then run `G28` and start the print
- If needed, insert `G4 P1800000` before `G28` in the slicer start G-code to wait 30 minutes

## For smaller prints

- Remove the long dwell from start G-code
- Sovol says the extra wait is usually not needed for prints smaller than about `300 x 300`

## Extra tuning ideas from Sovol

- Reduce first-layer speed if needed
- Use moderate Z-hop if needed; keep it under about `1.0 mm`
- Reduce first-layer flow if the nozzle is over-extruding or dragging
- For ABS or similar materials, improve adhesion with better fill strategy or glue

## Bed mesh check

- In Mainsail, review the `HEIGHTMAP`
- If the bed variation is still over about `1 mm` after heating to `65C` for `30 minutes`, re-check the bed condition
- If the problem remains large after heat soak, it may be a hardware issue

## Source

- [How to Enhance SV08 First Layer Printing](https://wiki.sovol3d.com/en/How-to-Enhance-SV08-First-Layer-Printing)
