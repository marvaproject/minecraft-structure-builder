# Architecture Principles (T2 & T3)

Universal architectural and technical construction rules for occupiable buildings. Read this in Phase 1.5 (to lock the design) and Phase 6 (to verify the output). T1a/T1b/T1c builds skip this file.

---

## 1. Proportion & Massing

### Anti-Cube Rule
No building may have all three dimensions equal (1:1:1). A cube reads as a
"box" and lacks architectural character. Vary at least one dimension by ≥ 2
blocks.

**Exceptions (intentional):** pyramids, obelisks, cube monuments, and any
build where symmetry is the design intent. Note the waiver in the final
response.

### Recommended Footprint Ratios
| Building type | W:D ratio | Height note |
|---|---|---|
| Cottage | 1:1.2 – 1:1.5 | Height ≤ width |
| Townhouse | 1:1 – 1:1.3 | Height may exceed width |
| Shop | 1:1 – 1:1.5 | Single story + sign |
| Tower | 1:1 (square) | Height 2–4× width |
| Hall | 1:1.5 – 1:2 | Height ≤ width |
| Cathedral | 1:3 – 1:5 | Height may exceed width |

### Vertical Proportion
- **Floor height:** 3 blocks (floor + 2 air + ceiling) minimum for comfort;
  4 blocks for grand rooms.
- **Ceiling height:** ≥ 3 blocks above floor. Pitched roofs add 2–4 blocks.
- **Foundation:** 1 block below ground level (or pilings/stilts into water) to avoid a floating look.

---

## 2. Facade Depth & Silhouette

### Structural Framing Depth
- Never leave exterior walls completely flat. Place structural corner and bay
  pillars (`minecraft:*_log` or `stripped_*_log`) framing the wall plane, and
  align upper horizontal cross-beams at ceiling height.
- Add window depth using `glass_pane` framed with open `trapdoor` shutters,
  flower boxes (`grass_block` + `trapdoor`), or recessed sills.

### Silhouette Breakers
Every T2/T3 building must include **at least 2 silhouette/trim details** beyond a plain box and roof:
- Exterior chimney stack with lit `campfire` and `trapdoor` cap
- Covered front porch, wrap-around deck, or upper balcony with fence railings
- Roof dormer window or attic gable window
- Wall-mounted `vine`, leaf clusters, or hanging flower baskets

---

## 3. Circulation & Technical Access Rules (The Sims Navigation Standard)

### Path Width & Corner Turns
- **Main path:** ≥ 2 blocks wide (player + villager passing).
- **Secondary path:** ≥ 1 block wide.
- **90° Corner Turns (L-Turn / U-Turn):** Every turning point in a corridor or stairwell MUST reserve a full `1 × 1` walkable corner floor block (`1 × 2` air height, or `1 × 3` in stairwells) free of pillars, pots, or low-hanging lanterns so characters never snag on corners.

### Doors, Fence Gates & Trapdoors
- Always place **both** halves of a door (`half: 'lower'` at `Y` and `half: 'upper'` at `Y + 1`) with identical `facing` and `hinge`.
- **Front & Back Approach Clearance:** Every door, `fence_gate`, and floor/wall `trapdoor` passage MUST have at least **1–2 blocks of unobstructed walkable floor** (`minecraft:air` at `Y` and `Y + 1` above a solid floor at `Y - 1`) both **in front (`+facing`)** and **behind (`-facing`)**.
- Never place a door in a room corner or adjacent to a staircase. Doors should swing **into** the room, not outward.
- Avoid two doors directly facing each other < 3 blocks apart.

### Staircases, Ladders & Interior Landing Bounds
- Construct staircases as a continuous 1:1 slope (`half: 'bottom'`), shifting `1` block horizontally for every `+1` rise until flush against the upper floor slab.
- **Interior Landing Bounds (Never Breach Exterior Walls):** Both the **bottom entry landing** (`Y_start`) and the **top exit landing** (`Y_upper`) MUST lie strictly on **interior floor tiles** (at least 1 block inside the exterior wall perimeter `X_wall ± 1`, `Z_wall ± 1`), completely free of furniture, walls, railings, or doors. Never allow a staircase top landing to land on or breach an exterior wall line.
- Inset staircases inside solid exterior walls (`X = 1` or `X = W - 2`) so side elevations stay clean.
- Always carve **3 blocks of vertical `minecraft:air`** (`y + 1` to `y + 3`) above every walkable stair step AND above both bottom/top landing tiles after building upper floors/roofs.

### Balconies, Porches & Fenced Decks (Clearance Outside Railings)
- Because `fence` / `wall` railings consume the entire `1 × 1` block they occupy, a balcony, porch, or deck MUST have **at least 1 full row/column of unobstructed walkable floor (`1 × 2` air height) inside the outer railing perimeter** (meaning minimum total depth is **2 blocks**: `1` walkable floor block + `1` outer railing block; minimum total width for a 3-sided projecting balcony is **3 blocks**: `1` left rail + `1` walkable center + `1` right rail).

### Beds, Chests, Seating & Workstation Access
- **Beds (`2 × 1`):** Always place both `part: 'head'` and `part: 'foot'`, keep `Y + 1` and `Y + 2` clear above the bed, and leave **at least 1 walkable floor block (`1 × 2` air)** along the long side of the bed so characters wake safely without suffocating in walls.
- **Stair Chairs & Sofas (`minecraft:*_stairs`):** In Minecraft, stair `facing` points toward the **high backrest side**, NOT the low seat side. Always set chair/sofa `facing` **toward the backrest** (away from dining tables, or flush toward the wall behind a sofa). For low coffee tables, use `bottom` slabs (`type: 'bottom'`) so they rest on the floor without floating.
- **Chests, Barrels & Workstations:** Leave **at least 1 walkable floor block** in front (`+facing`) for interaction, plus **1 empty/transparent block above every `chest`** (`Y + 1`) so the lid opens freely.

### Discrete Grid Adjacency & Auto-Connected Fences/Panes
Because Minecraft uses a discrete `1 × 1 × 1` block grid, objects attached to or placed flush against a wall NEVER share the wall block's `(X, Y, Z)` coordinate — they occupy the **adjacent air block** (`±1` on the perpendicular axis):
- **Wall-Mounted Attachments (`wall_torch`, `*_wall_sign`, `ladder`, `tripwire_hook`, `lever`, `button`):** Place at `(X_wall ± 1, Y, Z_wall ± 1)` in the air block adjacent to the solid wall, with `facing` pointing **away** from the supporting wall.
- **Vines (`minecraft:vine`):** Place in the adjacent air block `(X_wall ± 1, Y, Z_wall ± 1)`, with the directional boolean (`north`/`south`/`east`/`west`) pointing **toward** the wall being climbed.
- **Window Shutters (`*_trapdoor`):** Place in the exterior air block directly outside the window plane (`Z_wall ± 1` or `X_wall ± 1`), NOT overwriting `glass_pane`, with `open: 'true'` and `facing` pointing outward.
- **Flush Wall Furniture (`chest`, `barrel`, `furnace`, `crafting_table`, bed `head`):** Place at `(X_wall ± 1, Y_floor, Z_wall ± 1)` on the interior floor tile in front of the wall, with `facing` pointing **into the room** (away from the wall).
- **Ceiling-Hung Lanterns (`lantern`):** Place at `(X, Y_ceiling - 1, Z)` directly beneath a solid ceiling/beam block at `Y_ceiling` with `hanging: 'true'`.
- **Auto-Connected Fences, Walls & Panes (`*_fence`, `*_wall`, `glass_pane`, `iron_bars`):** Never hardcode `north`/`south`/`east`/`west` connection booleans manually. Run a post-placement neighbor pass (`connectFencesAndPanes()`) before NBT serialization so each fence/wall/pane connects automatically (`'true'`/`'false'`) to adjacent solid blocks and matching fences/panes, preventing awkward disconnected or backward-looking corners.

### Room Connectivity
- Every room must be reachable from the entrance via a continuous path ≥ 1 block wide.
- No dead-end rooms without a purpose (storage closets are fine; empty dead-ends are not).

---

## 4. Lighting

### Density Rule
- **≥ 1 light source per 4×4 interior area** (16 blocks²).
- A 6×6 room needs ≥ 2–3 light sources.
- A 4×4 room needs ≥ 1 light source.

### Light Source Selection
| Context | Preferred | Avoid |
|---|---|---|
| Ceiling | Lantern (`hanging: 'true'`), chandelier (chain + lantern) | Torch (looks flat) |
| Wall | Wall torch, lantern on fence/post | Floating torch |
| Floor | Campfire (outdoor), lantern on post | Torch on floor (low light) |
| Outdoor path / dock | Lantern on fence post every 5–7 blocks | Torch spam |

### Avoid
- Torches directly adjacent to beds (villagers path awkwardly).
- Light sources hidden inside solid walls.
- Dark corners in occupiable rooms (mobs spawn at light level 0).

---

## 5. Material Coherence & Vertical Gradation

### Palette Rule
- **≤ 3 primary materials + 1 accent.**
- Primary: structural blocks (foundation/walls, framing, floor, roof).
- Accent: decorative blocks (trim, shutters, lanterns, signs, flower pots, banners).

### Vertical Gradation
- **Base / Lower Wall (`Y_floor` level):** Use heavy, moisture-resistant stone (`cobblestone`, `stone_bricks`, `andesite`, `deepslate_bricks`) for the foundation and bottom wall course.
- **Upper Wall:** Transition to warmer or lighter wall materials (`oak_planks`, `spruce_planks`, `stripped_*_log`, `sandstone`, `terracotta`).
- **Roof Trim:** Contrast the roof field (`spruce_stairs`, `dark_oak_stairs`) with a stone or contrasting wood outer trim (`stone_brick_stairs`, `cobblestone_stairs`).

### Material Pairing Examples
| Primary | Secondary | Accent |
|---|---|---|
| Oak planks | Cobblestone / Spruce log | Spruce trapdoor / Lantern |
| Spruce planks | Stone bricks | Dark oak sign / Barrel |
| Sandstone | Smooth sandstone | Terracotta |
| Deepslate bricks | Polished deepslate | Copper |
| Dark oak | Stripped dark oak | Warped trapdoor |

---

## 6. Roof & Chimney Rules

### Roof Types by Biome
| Biome | Roof style | Pitch |
|---|---|---|
| Plains | Gable / pitched | 1:1 or 2:1 |
| Taiga | Steep gable | 2:1 or 3:1 |
| Desert | Flat with parapet | 0 (flat) |
| Savanna | Flat with overhang | 0 (flat) |
| Snowy | Steep gable | 2:1 or 3:1 |
| Swamp / River | Gable, mossy / timber trim | 1:1 or 2:1 |
| Pale Garden | Gable, pale oak | 2:1 |

### Roof & Chimney Rules
- **Overhang:** 1 block beyond the wall plane on all sides.
- **Ridge:** Top row of stairs/slabs forming the peak.
- **Gable ends:** Fill triangular gable ends with wall planks and/or an attic window, never open air.
- **Chimney Placement:** Place chimneys along an **exterior wall** (outside the sloped roof planes) or rising cleanly through the central roof ridge, extending ≥ 1 block above the roof peak. Never pass a chimney shaft through interior walkways, stairs, or sloped roof pitches.

---

## 7. Foundation & Grounding

- **Base layer:** 1 block of stone/cobblestone below the lowest floor block (or full wooden/stone pilings down to `Y = 0` for over-water/dock structures) to avoid a floating look.
- **Foundation depth:** 1 block below ground level minimum; 2 blocks for hillside builds.
- **Path connection:** A path block, gravel trail, or wooden dock leading cleanly to the main entrance.

---

## 8. Villager Pathfinding & Multi-Part Blocks (T2/T3)

- **Paired Beds (`2 × 1`):** Always place both `part: 'head'` at `(X, Y, Z)` and `part: 'foot'` at `(X - dx, Y, Z - dz)` matching `facing`, with ≥ 1 walkable floor block (`1 × 2` air) adjacent to the bed and 2 blocks air above it.
- **Workstation & Chest access:** ≥ 1 walkable block adjacent to the workstation (`barrel`, `smoker`, `lectern`, etc.), and ≥ 1 empty/transparent block above every `chest` so lids open freely.
- **Door & Gate clearance:** ≥ 1–2 walkable blocks inside and outside every door or `fence_gate`.
- **Stair & Ladder access:** Stairs and ladders must have a ≥ 1-block walkable interior landing at both bottom entry and top exit, reachable without jumping and never breaching exterior walls.
- **Balcony & Deck depth:** ≥ 1 walkable floor block clear of the outer `fence`/`wall` railing row (minimum total depth 2 blocks).
- **No 1-block-wide bottlenecks** on main villager routes, with full `1 × 1` clearance on 90° corners.

---

## 9. Phase 6 Verification Checklist

Use this table in Phase 6 to report pass/fail to the user:

| Check | Pass criteria | Source |
|---|---|---|
| Room size | Every room ≥ minimum for its function | `room-templates.md` |
| Light density | ≥ 1 light per 4×4 interior | §4 above |
| Roof style & overhang | Matches biome convention + 1-block overhang | `biome-styles.md` & §6 |
| Palette & gradation | ≤ 3 primary + 1 accent; heavy base, lighter upper | §5 above |
| Facade depth & silhouette | Framing pillars + ≥ 2 exterior trim/silhouette details | §2 above |
| Proportion | Not 1:1:1 unless intentional | §1 above |
| Technical & circulation (The Sims & Grid Standard) | Doors/gates front & back clear; stairs/ladders 3-block headroom + interior bottom/top 1-block landing; beds/chests/workstations/stair-chairs oriented properly (backrest outward); balconies ≥ 1 walkable block inside railing; fences/panes auto-connected; wall-mounted/flush items on adjacent air coordinates | §3 & §8 above |

If any check fails, revise Phase 3 coordinates and regenerate.
