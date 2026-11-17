# Room Templates & Functional Zoning (T2 & T3)

Minimum room sizes, required furniture, and spatial zoning rules per function.
Read in Phase 1.5 to lock room bounds in the Coordinate Anchor Table, and in Phase 6 to verify room sizes.

---

## 1. Minimum Room Sizes & Required Blocks

| Room / Zone | Min size (W×D) | Min height | Required functional blocks |
|---|---|---|---|
| Bedroom (1 bed) | 3×4 | 3 | Paired bed (`head` + `foot`), chest/barrel, light |
| Bedroom (2 beds) | 4×4 | 3 | 2 paired beds, chest/barrel, light |
| Kitchen | 4×4 | 3 | Furnace/smoker, crafting table, chest/barrel, light |
| Dining room | 5×4 | 3 | Table (fence/slab) + ≥ 4 seats (stairs), light |
| Living room | 5×5 | 3 | Seating (stairs), low table, light |
| Storage | 3×3 | 3 | ≥ 4 chests/barrels, light |
| Workshop | 5×5 | 3 | Profession workstation (`barrel`, `smithing_table`, `loom`, etc.), storage, light |
| Library | 5×5 | 3 | Bookshelves (≥ 4), lectern, light |
| Trading hall | 5×5 | 3 | ≥ 2 workstations, counter (slabs), light |
| Bathroom | 3×3 | 3 | Cauldron, trapdoor/slab trim, light |
| Hallway / Corridor | 1–2×N | 3 | Light every 5 blocks |
| Stairwell | 3×N | 3 + headroom | Continuous 1:1 stairs (`half: 'bottom'`), 3-block headroom carved |
| Entrance / Foyer | 4×4 | 3 | Door (`lower` + `upper`), 2-block clearance, light |
| Balcony / Deck / Pier | 3×3 | 2 (open) | Fence railing around open edges, light |

---

## 2. Spatial Zoning Rules (For Coordinate Anchor Table)

Instead of drawing ASCII boxes, assign each room/zone an explicit rectangular coordinate range `[X1..X2, Y_floor, Z1..Z2]` in Phase 3 following these zoning principles:

- **Entry & Front Zone (`Z` near entrance):** Keep a `2 × 2` clear landing inside the front door (`D`) free of chests, beds, or stairs.
- **Wet / Heat Zone (Kitchen, Forge, Chimney):** Place furnaces, smokers, and blast furnaces along an exterior wall so an exterior chimney (`X = 0` or `X = W - 1`) aligns directly behind them without crossing interior floor space.
- **Quiet / Sleeping Zone (Bedrooms):** Place beds in back corners or on upper floors away from main doors. Leave ≥ 1 walkable floor tile along the long side of every bed (`2 × 1`) for safe villager wake-up pathing.
- **Vertical Transition Zone (Stairwells):** Run stairs parallel along an interior wall (`X = 1` or `X = W - 2`), starting at least 2 blocks away from any doorway, and ending flush at `Y_upper - 1` against the upper floor slab.
- **Multi-Room T3 Compounds:** Connect distinct wings or buildings via `≥ 2`-block-wide exterior paths/bridges and `≥ 1`-block-wide interior doorways.

---

## 3. Phase 6 Verification

For each room or zone in the build, verify against your Coordinate Anchor Table:

| Room / Zone | Min size met? | Required functional blocks present? | Light source present? |
|---|---|---|---|
| Bedroom | ≥ 3×4 (1 bed) / 4×4 (2 beds) | Paired bed + storage | ≥ 1 |
| Kitchen / Workshop | ≥ 4×4 / 5×5 | Workstation + storage | ≥ 1 |
| ... | ... | ... | ... |
