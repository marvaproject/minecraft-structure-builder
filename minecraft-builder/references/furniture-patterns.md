# Furniture & Multi-Block Object Patterns (T1c)

Ergonomics, orientation, and coordinate recipes for multi-block furniture and decorative props.
Read in Phase 3 (T1c only) to lock exact block coordinates, and in Phase 2 to confirm blocks exist in `registry_26_3.json`.

---

## 1. Ergonomics & Proportion Rules

| Furniture | Surface height | Backrest / Upper tier | Clearance |
|---|---|---|---|
| Seat (chair, stool, bench) | 1 block (stair/slab top at `y+1`) | 1–2 blocks (if any) on `-facing` side | ≥ 1 adjacent floor block |
| Table / Desk | 1 block (slab/fence/trapdoor top at `y+1`) | none | 1 block on each used side |
| Counter / Bar | 1 block (slab/stair at `y+1`) | none | 1 block front (customer side) |
| Bed frame / Bunk bed | 1 block (`y+1` base; `y+3` top bunk) | headboard optional | 1 block along side/foot |
| Shelf / Rack | 2–3 blocks tall | wall-mounted (`-facing` against wall) | 1 block front |

### Orientation Rules (Minecraft Stair-Seat Convention)
- **Stair Chairs & Sofas (`minecraft:*_stairs`):** In Minecraft, a stair's `facing` property points toward its **high backrest side** (the ascending step side), NOT the low seat side! Therefore, to seat a player facing direction `D`, set `facing` to the **opposite direction (`-D`)** so the backrest sits behind the player and away from the table/room center:
  - Chair on West (`x - 1`) facing East toward table → `facing: 'west'` (backrest on West)
  - Chair on East (`x + 1`) facing West toward table → `facing: 'east'` (backrest on East)
  - Chair/Sofa on North (`z - 1`) facing South toward room → `facing: 'north'` (backrest on North)
  - Chair/Sofa on South (`z + 1`) facing North toward room → `facing: 'south'` (backrest on South)
- **Tables:** Symmetric; use `bottom` slabs or `trapdoor` on top of a `fence` post (`y+1`), or `bottom` slabs (`type: 'bottom'`) for low coffee tables so they rest flush on the floor without floating.
- **Counters:** Face the customer/open side (`+facing`), with storage/workstation space behind.
- **Beds:** `head` against wall (`(X, Y, Z)`), `foot` toward open floor (`(X - dx, Y, Z - dz)`).
- **Shelves & Racks:** Back flush against a wall, open front facing the room.

### Material Coherence
- Match primary seat and table wood (e.g., oak stair seats + oak slab table).
- Use at most 1 contrasting trim material (e.g., spruce trapdoor armrests/backrest on oak seating).
- Keep total distinct materials ≤ 3 per furniture piece.

---

## 2. Coordinate Recipes (Relative to Base `x, y, z` on Floor `y`)

- **Single Chair (player sits facing `south`, backrest on North `z-1`):**
  - Seat: `(x, y+1, z)` → `minecraft:<wood>_stairs` (`facing: 'north'`, `half: 'bottom'`)
  - Optional tall backrest: `(x, y+2, z-1)` (or trapdoor at `(x, y+1, z-1)` with `facing: 'south'`, `open: 'true'`)
  - Armrests (optional): signs or trapdoors on `(x-1, y+1, z)` and `(x+1, y+1, z)`
- **Stool (1×1):**
  - Leg: `(x, y+1, z)` → `minecraft:<wood>_fence`
  - Top: `(x, y+2, z)` → `minecraft:<wood>_pressure_plate` (or `slab` at `y+1`)
- **Bench / Sofa (3-wide along X, player sits facing `south`, backrest against North wall):**
  - Seats: `(x..x+2, y+1, z)` → 3× `minecraft:<wood>_stairs` (`facing: 'north'`, `half: 'bottom'`)
  - Optional trapdoor backrest: `(x..x+2, y+1, z-1)` → 3× `minecraft:<wood>_trapdoor` (`facing: 'south'`, `open: 'true'`)
- **4-Seat Dining Table (3×3 footprint centered at `x, z`):**
  - Table leg & top: `(x, y+1, z)` `minecraft:<wood>_fence` + `(x, y+2, z)` `minecraft:<wood>_slab` (`type: 'bottom'`)
  - 4 Chairs (all backrests outward, seats facing table center):
    - North seat `(x, y+1, z-1)`: `facing: 'north'`
    - South seat `(x, y+1, z+1)`: `facing: 'south'`
    - West seat `(x-1, y+1, z)`: `facing: 'west'`
    - East seat `(x+1, y+1, z)`: `facing: 'east'`
- **6-Seat Long Table (3×5 footprint):**
  - Table spine: 3× fence posts at `y+1` + 3× slabs at `y+2` along `z..z+2`
  - Seats: 3× stairs on `x-1` with `facing: 'west'`, 3× stairs on `x+1` with `facing: 'east'`
- **Counter / Bar (N blocks wide, facing `south`):**
  - Base: `(x..x+N-1, y+1, z)` → `minecraft:<wood>_fence` or upside-down stairs (`half: 'top'`, `facing: 'north'`)
  - Top: `(x..x+N-1, y+2, z)` → `minecraft:<wood>_slab` (`type: 'bottom'`)
- **Bookshelf / Storage Unit (3×2×1 against north wall):**
  - Lower storage: `(x..x+2, y+1, z)` → `minecraft:barrel` (`facing: 'south'`) or `minecraft:bookshelf`
  - Upper shelf: `(x..x+2, y+2, z)` → `minecraft:bookshelf` flanked by `minecraft:<wood>_trapdoor` trim
- **Bunk Bed (2×4×1, facing `east`):**
  - Bottom bunk (`y+1`): `foot` at `(x, y+1, z)`, `head` at `(x+1, y+1, z)` (`facing: 'east'`)
  - Support posts: `(x-1, y+1..y+3, z)` and `(x+2, y+1..y+3, z)` → `minecraft:<wood>_fence` + `minecraft:ladder` (`facing: 'west'`) on end post
  - Top bunk (`y+3`): slabs at `(x..x+1, y+2, z)` + `foot` at `(x, y+3, z)`, `head` at `(x+1, y+3, z)`
- **Table / Hanging Lamp:**
  - Table lamp: `(x, y+1, z)` `minecraft:<wood>_fence` + `(x, y+2, z)` `minecraft:lantern` (`hanging: 'false'`)
  - Hanging lamp: `(x, y+2, z)` `minecraft:chain` (`axis: 'y'`) + `(x, y+1, z)` `minecraft:lantern` (`hanging: 'true'`)

---

## 3. Orientation Cheat Sheet

| Furniture | Block `facing` value | Reasoning |
|---|---|---|
| Stair chair at table / desk | **Away from table** (toward backrest) | Minecraft stair `facing` points to the high backrest side |
| Stair sofa against wall | **Toward the wall** behind it | High backrest rests flush against the wall; seat faces room |
| Counter | Customer side outward | Vendor behind counter |
| Bed | `head` against wall | `foot` toward open floor |
| Shelf / Weapon rack | Back against wall | Front accessible |
| Lamp | N/A (light radiates) | Place at eye/ceiling height |

---

## 4. Phase 6 Report Format (T1c)

For each furniture piece, report:

| Furniture | Dimensions (W×H×L) | Seats / capacity | Material palette | Orientation |
|---|---|---|---|---|
| Dining table | 3×2×3 | 4 seats | Oak + spruce | Backrests outward |
| Chair | 1×2×1 | 1 seat | Oak + spruce | Backrest outward |

No full architectural checklist needed — T1c skips Phase 6 architectural checks.
