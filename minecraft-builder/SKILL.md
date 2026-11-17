---
name: minecraft-builder
description: Brainstorm, design, and generate ANY static block-based Minecraft structure — from single furniture pieces (chair, table, shelf, bunk bed) and decorative/civic builds (well, street lamp, statue, bridge, fountain, farm) through occupiable shelters (tent, shed, hut, watchtower) and standard buildings (villager house, shop, cottage, lighthouse, temple) up to complex compounds (castle, cathedral, village hall, market square) — using a Coordinate Anchor Table, 5-Pass Constructive Layering, tiered architectural quality rules, and a downloadable standalone zero-dependency Node.js (.mjs) generator file that exports vanilla Minecraft .nbt files (Minecraft 26.3 / DataVersion 5023). Use whenever the user asks to design, plan, or generate any Minecraft build, structure, furniture, villager house/shop, monument, or .nbt structure generator in JavaScript.
---

# Minecraft Builder

Design functional **and architecturally sound** Minecraft structures of **any static block-based scale** — from tiny 1×2 furniture to sprawling castles — through brief collaborative brainstorming, tiered architectural review, exact **Coordinate Anchor Mapping**, and **5-Pass Constructive Layering**. The final output is a **downloadable self-contained, zero-dependency ESM (`.mjs`) file** (exported via the file generation tool rather than dumping raw code blocks in chat) that generates a gzip-compressed vanilla `.nbt` structure file for Minecraft 26.3 (`DataVersion: 5023`) cleanly without runtime validation crashes.

## When to Use

  - The user asks to create, design, or generate **any static block-based Minecraft structure**:
      - **Furniture & Props (T1c):** chairs, dining tables, bookshelves, benches, bunk beds, thrones, counters, lamps, weapon racks.
      - **Decorative & Civic Structures (T1b):** wells, street lamps, statues, signposts, bridges, fences, farm plots, fountains, monuments, gazebos.
      - **Simple Occupiable Shelters (T1a):** tents, campfire shelters, sheds, single-room huts, small watchtowers.
      - **Standard Buildings (T2):** villager houses, shops, small temples, 2-story cottages, libraries, bakeries, lighthouses, stables.
      - **Complex Compounds (T3):** castles, cathedrals, village halls, mansions, market squares, walled settlements.
  - The user wants a standalone Node.js script that outputs a vanilla `.nbt` file ready for local execution and 3D previewing on Shulkr Viewer.

## Out of Scope

This skill does **not** handle:

  - **Redstone logic** (circuits, piston doors, traps) — use a dedicated redstone skill.
  - **Terraforming** (mountains, valleys, rivers, custom terrain) — use a dedicated terrain skill.
  - **Block entity state** (chest contents, furnace progress, sign text) — the template writes palette/blocks only; edit these in-game after loading.
  - **Entity posing** (armor stands, item frames with specific items) — requires entity NBT far beyond the base template.
  - **Command blocks / animations** — not static `.nbt` structures.

If the user requests any of the above, state the limitation clearly and offer the closest static-block alternative.

## Folder Layout

    minecraft-builder/
    ├── SKILL.md
    ├── registry_26_3.json
    └── references/
        ├── architecture-principles.md   (T2 & T3)
        ├── biome-styles.md              (T2 & T3, or biome-matched T1)
        ├── room-templates.md            (T2 & T3)
        └── furniture-patterns.md        (T1c)

## Build Complexity Tiers

Classify every request into one of five tiers first. The tier determines which phases, technical rules, and reference files apply.

| Tier | Examples | Phases applied | Focus | |---|---|---|---| | **T1a — Simple Occupiable** | Tent, campfire shelter, small shed, single-room hut, small watchtower | 1, 2, 3 (conditional), 4, 5 | Entrance clear, grounded blocks, functional furniture present | | **T1b — Non-Occupiable Structure** | Well, street lamp, statue, signpost, bridge, fence, farm plot, fountain, monument | 1, 2, 3 (support only), 4, 5 | Dimensions, functional element (water/light/connection), support integrity | | **T1c — Furniture / Multi-block Object** | Chair, dining table, bookshelf unit, bench, bed frame, bunk bed, throne, counter | 1, 2, 3 (ergonomics), 4, 5 | Ergonomics, orientation, accessibility, material coherence | | **T2 — Standard Building** | Villager house, shop, small temple, 2-story cottage, library, bakery, lighthouse | 1, 1.5, 2, 3, 4, 5, 6 | Full architectural & technical checklist (biome advisory) | | **T3 — Complex Compound** | Multi-building compound, castle, village hall, cathedral, mansion, market square | 1, 1.5, 2, 3 (+circulation routes), 4, 5, 6 | Full checklist, strict biome coherence, explicit inter-room circulation |

If the tier is ambiguous, ask the user: *"Is this furniture (chair/table), a decorative structure (well/lamp/statue), a simple shelter (tent/shed), a standard building (house/shop), or a complex compound (castle/hall)?"*

## Workflow

### Phase 1: Brief Brainstorming & Clarification (When Details Are Ambiguous)

Classify the build tier first. If key design parameters are missing or ambiguous, ask **2 to 4 concise, concrete questions** tailored to the tier:

**For T1a (Occupiable Simple):**

1.  How many occupants? (determines beds, seating, storage)
2.  Biome / palette (forest camp, plains, taiga, etc.)
3.  Enclosed or open-air? Special features (campfire, chest, canopy)?

**For T1b (Non-Occupiable Structure):**

1.  What is the functional element? (water source, light source, connection point, decorative focus)
2.  Target dimensions (height × width × depth)?
3.  Biome / material palette?
4.  Does it need a cap/roof/canopy or support base?

**For T1c (Furniture):**

1.  What is the furniture's function? (seating, dining, storage, sleeping, display)
2.  How many "seats" or capacity units? (1 chair, 4-seat table, 6-slot shelf)
3.  Material palette? (oak, spruce, dark oak, warped, etc.)
4.  Where will it be placed? (against wall, center of room, outdoors) — affects orientation defaults.

**For T2 / T3:**

1.  Occupants & function (who uses it, primary purpose)
2.  Biome / block palette
3.  Scale & special features (footprint, levels, open-air vs enclosed, chimney/balcony/canopy)
4.  For T3: number of connected buildings/wings, if any

*Skip brainstorming and proceed directly to the next phase if the user has already specified these details or explicitly asks to generate immediately.*

### Phase 1.5: Architectural Review (T2 & T3 only)

**Skip this phase for T1a, T1b, and T1c.**

For **T2 & T3**, consult the reference files in `references/` before locking coordinates:

  - **`architecture-principles.md`** — proportion, facade depth, silhouette details, circulation, lighting, and material coherence rules.
  - **`biome-styles.md`** — palette and roof style for the chosen biome; if the user did not pick a biome, select one and state the choice explicitly.
  - **`room-templates.md`** — minimum room sizes and required functional blocks per room/zone.

Resolve every item before Phase 3:

  - \[ \] Every room/zone meets minimum dimensions for its function (from `room-templates.md`).
  - \[ \] At least one light source per 4×4 interior area (from `architecture-principles.md`).
  - \[ \] Roof style matches the chosen biome and includes a 1-block overhang (from `biome-styles.md` & `architecture-principles.md`).
  - \[ \] Material palette ≤ 3 primary + 1 accent, with vertical gradation (stone/heavy base, wood/main wall).
  - \[ \] Facade depth & silhouette: corner framing pillars (`log`) + ≥ 2 silhouette/trim details (exterior chimney, trapdoor shutters, porch/balcony, dormer, vines).
  - \[ \] **The Sims Navigation Standard:** Clear 1-block walkable zones in front/behind doors & fence gates, at interior bottom/top landings of stairs & ladders, beside beds (plus 2-block air above), in front of chests/workstations/seats, on 90° corridor corners, and **≥ 1 walkable floor row inside any balcony/deck railing** (total depth ≥ 2 blocks).
  - \[ \] Building proportions are **not** 1:1:1 (vary width, depth, and height).
  - \[ \] Circulation: every room reachable via ≥ 1-block-wide path (≥ 2 blocks for main paths) from the entrance.

### Phase 2: Block Registry Lookup (`registry_26_3.json`)

  - Inspect `registry_26_3.json` in the skill directory to verify exact block IDs (`minecraft:<name>`) and valid block-state properties for Minecraft 26.3 (`DataVersion: 5023`), especially for newer blocks (copper variants, pale oak, resin, tuff bricks, etc.).
  - Never guess block-state property names or allowed enum values.
  - If a requested block or property is missing from the registry, surface the gap to the user and propose the closest valid alternative instead of silently inventing IDs.

### Phase 3: Coordinate Anchor Table & Technical Construction Rules

**Do NOT draw ASCII character grids.** Define an exact **Coordinate Anchor Table** (`X, Y, Z`) before writing code so every block placement maps 1:1 to `setBlock()` / `fill()` calls:

1.  **Bounding Box & Elevation Datum:**
      - Declare `SIZE = [W, H, L]` (`X: 0..W-1`, `Y: 0..H-1`, `Z: 0..L-1`).
      - Fix exact elevation levels: `Y_base` (foundation/stilts), `Y_floor1` (ground floor), `Y_floor2` (upper floor, if any), `Y_roof_start`, and `Y_roof_peak`.
2.  **Anchor Coordinates Table:** List exact coordinates and facing directions for:
      - **Exterior Shell & Framing Pillars:** Corner post `(X, Z)` coordinates and wall bounds.
      - **Interior Floor Bounds (`X_min..X_max`, `Z_min..Z_max`):** Explicitly verify that every interior bed, chest, table, and workstation coordinate lies strictly **inside** the interior floor bounds (`X_wall_west + 1 .. X_wall_east - 1`, `Z_wall_north + 1 .. Z_wall_south - 1`) and never overlaps an exterior or partition wall coordinate.
      - **Doors / Entrances:** `(X, Y_floor, Z)` + `facing` + `hinge` + clear front/back tiles.
      - **Staircases:** Interior bottom entry landing `(X0, Y1, Z0)` → First step `(X1, Y1, Z1)` → (if turning: Turning landing `(Xt, Y_turn, Zt)`) → Top step `(X2, Y2-1, Z2)` → Interior top exit landing `(X3, Y2, Z3)` + `facing`.
      - **Balconies / Porches (if any):** Walkable floor bounds vs outer railing coordinates (ensuring ≥ 1 walkable block row inside the fence).
      - **Chimney / Utility Shafts:** Exterior `(X, Z)` column outside the main roof slope with 4-sided cap check.
      - **Key Interior / Functional Elements:** Beds, stair-chairs/sofas (with backrest-facing direction and explicit `shape: 'straight'`), workstations, storage, water/light cores, and furniture with `(X, Y, Z)` coordinates.

#### Technical Detail Rules (Enforced by Design & Layer Order — No Runtime Throw Validators)

Apply these technical construction rules directly while writing the build script:

**Universal Rules (All Tiers):**

  - **Minecraft Stair-Seat Orientation & Explicit Shape Lock Rule:** In Minecraft, `minecraft:*_stairs` `facing` points toward the **high backrest side** (ascending step), NOT the low seat side.
      - Whenever using stairs as chairs, sofas, or walkable staircase steps, **always pass `shape: 'straight'`** explicitly (or `outer_*`/`inner_*` for corner sofas/diagonal stairs) so `autoConnectStairs()` never accidentally mutates a chair or staircase step into a corner roof block if placed adjacent to another stair at the same `Y`.
      - Only leave `shape` omitted (`undefined`) on **roof stairs** that should automatically join into hips/valleys.
      - For low coffee tables, use `bottom` slabs (`type: 'bottom'`) so they sit flush on the floor without floating.
  - **Auto-Connected Roof, Corner & Single-Block Corner Stairs (`autoConnectStairs()`):** Whenever building pitched, hip, or pyramidal roofs with `*_stairs`, `autoConnectStairs()` runs at the end of Pass 5 using **vanilla Minecraft lock-check rules**:
      - Any stair placed with an **explicit `shape`** (`outer_left`, `outer_right`, `inner_left`, `inner_right`, or `straight` — e.g., stair-chairs, walkable staircase steps, standalone 1-block corner stairs, or diagonal staircases) is **preserved untouched** unless `shape` was left omitted (`undefined`).
      - When `shape` is omitted, `autoConnectStairs()` checks orthogonal neighbors at the same `half`: it forms `outer_left`/`outer_right` from a perpendicular stair in front (`+facing`) ONLY if the opposite lateral side is not locked by a parallel stair (`facing === f`), and forms `inner_left`/`inner_right` from a perpendicular stair behind (`-facing`) ONLY if the matching lateral side is not locked by a parallel stair (`facing === f`).
  - **Double Chest Orientation & Auto-Pairing (`connectDoubleChests()`):** In Minecraft, a chest's `type` (`single`, `left`, `right`) is defined from the **chest's own perspective** (looking in the direction of its `facing`), NOT from the viewer's perspective:
      - When standing in front of a double chest looking at its latch: the block on the **viewer's left** is `type: 'right'` (located to the chest's right: `rightOf[facing]`), and the block on the **viewer's right** is `type: 'left'` (located to the chest's left: `leftOf[facing]`).
      - Always run `connectDoubleChests()` at the end of Pass 5 so adjacent matching `minecraft:chest` or `minecraft:trapped_chest` blocks sharing the same `facing` automatically pair into valid `type: 'right'` + `type: 'left'` halves without inverted textures (unless `type: 'single'` was explicitly specified). Never place 3 chests of the same type in a single row.
  - **Auto-Paired Double Doors (`autoPairDoubleDoors()`):** When two matching `*_door` blocks are placed side-by-side sharing the same `facing` and `Y`, `autoPairDoubleDoors()` runs at the end of Pass 5 to assign symmetrical hinges (`hinge: 'left'` on the left-hand door and `hinge: 'right'` on the right-hand door from the viewer's front perspective) across both `lower` and `upper` halves so handles meet cleanly in the center.
  - **Auto-Connected Fences, Walls, Fence Gates & Panes (`connectFencesAndPanes()` & `connectWalls()`):**
      - `connectFencesAndPanes()` computes `north`/`south`/`east`/`west` boolean properties for `*_fence`, `*_pane`, and `iron_bars`, excluding non-solid and non-connecting blocks (`*_glass`, `*_leaves`, `hopper`, `bell`, `beacon`, `glowstone`, slabs, stairs, doors, etc.) via `isFullSolidForConnection()`.
      - `connectWalls()` computes `north`/`south`/`east`/`west` enum states (`'none'`, `'low'`, or `'tall'` when a solid block sits directly above the connection) plus the center pillar `up` (`'true'`/`'false'`) for all `*_wall` blocks, and automatically sets `in_wall: 'true'` on any `*_fence_gate` flanked by `*_wall` blocks so the gate lowers flush with the wall.
  - **Paired Bed Helper (`placeBed(x, y, z, color, facing)`):** Always place beds via `placeBed(x, y, z, color, facing)` where `(x, y, z)` is the **pillow (`part: 'head'`)** coordinate and `facing` points from foot toward head (against the wall); the helper automatically places `part: 'foot'` at `(x - dx, y, z - dz)` so beds never break upon structure load.
  - **Discrete Grid Adjacency & Wall-Attachment Mapping (No Wall Overwrites or Glitches):** Because Minecraft uses a `1 × 1 × 1` block grid, objects attached to or placed flush against a wall NEVER share the wall block's `(X, Y, Z)` coordinate — they occupy the **adjacent air block** (`±1` on the perpendicular axis):
    1.  *Wall-Mounted Blocks (`wall_torch`, `*_wall_sign`, `ladder`, `tripwire_hook`, `lever`, `button`):* Place at `(X_wall ± 1, Y, Z_wall ± 1)` in the air block adjacent to the solid wall, with `facing` pointing **away** from the supporting wall (North wall `Z` → place at `Z + 1`, `facing: 'south'`; South wall `Z` → `Z - 1`, `facing: 'north'`; West wall `X` → `X + 1`, `facing: 'east'`; East wall `X` → `X - 1`, `facing: 'west'`).
    2.  *Vines (`minecraft:vine`):* Place in the adjacent air block `(X_wall ± 1, Y, Z_wall ± 1)` with the directional boolean (`north`/`south`/`east`/`west`) pointing **toward** the wall being climbed.
    3.  *Window Shutters (`*_trapdoor`):* Place in the exterior air block directly outside the window plane (`Z_wall ± 1` or `X_wall ± 1`), NOT overwriting `glass_pane`, with `open: 'true'` and `facing` pointing outward.
    4.  *Flush Wall Furniture (`chest`, `barrel`, `furnace`, `crafting_table`, bed `head`):* Place strictly on the adjacent interior floor tile `(X_wall ± 1, Y_floor, Z_wall ± 1)` inside the room bounds (never overwriting wall coordinates), with `facing` pointing **into the room** (away from the wall).
    5.  *Ceiling-Hung Lanterns (`lantern`):* Place at `(X, Y_ceiling - 1, Z)` directly beneath a solid ceiling/beam block at `Y_ceiling` with `hanging: 'true'`.
  - **Ground Support & No Floating Blocks:** Every structural block must rest on a solid block below (`y - 1`) or be intentionally suspended/wall-attached using the exact adjacency rules above.
  - **5-Pass Constructive Layering Order:** Always build in 5 sequential passes so structural elements never overwrite interior paths or headroom:
    1.  *Pass 1 — Foundation, Pilings & Floor Slabs*
    2.  *Pass 2 — Framing Pillars, Exterior Walls, Windows, Balconies & Doors (`placeDoor`)*
    3.  *Pass 3 — Roof / Canopy & Exterior Chimney (with 4-sided cap check)*
    4.  *Pass 4 — Staircases (`shape: 'straight'`), Exact Stairwell Floor Cutouts, Safety Railings & `carveStairHeadroom()`*
    5.  *Pass 5 — Interior Furniture (strictly inside room floor bounds), `placeBed()`, Wall Attachments, Lighting, `autoConnectStairs()`, `autoPairDoubleDoors()`, `connectDoubleChests()`, `connectWalls()` & `connectFencesAndPanes()`*

**T1a Rules (Simple Occupiable):**

  - Include functional essentials: light source + storage (`chest`/`barrel`) + sleeping spot if occupants sleep there.
  - Keep entrance unobstructed (≥ 1–2 blocks clear in front and behind).

**T1b Rules (Non-Occupiable Structure):**

  - Explicitly place the functional core: `minecraft:water` source for wells/fountains, `lantern`/`campfire`/`glowstone` for lamps, and solid anchor ends for bridges/fences.

**T1c Rules (Furniture — consult `furniture-patterns.md`):**

  - Seat & table height: 1 block (`y + 1` above floor); stair chair `facing` points toward the backrest (away from table) with `shape: 'straight'`.
  - Leave ≥ 1 adjacent floor tile empty in front of seats/storage for player access.

**T2 & T3 Rules — The Sims Navigation Standard (Prevent Character Traps Everywhere):**

1.  **Doors, Fence Gates & Trapdoors:**
      - Always place **both** halves of every door (`half: 'lower'` at `Y` and `half: 'upper'` at `Y + 1`) with identical `facing` and `hinge` (use `placeDoor(x, y, z, wood, facing)`).
      - Never place a door in a room corner or adjacent to a staircase. Keep at least **1–2 blocks of unobstructed walkable floor** (`minecraft:air` at `Y` and `Y + 1` above a solid floor at `Y - 1`) both **in front (`+facing`)** and **behind (`-facing`)** every door, `fence_gate`, and floor/wall `trapdoor` passage.
2.  **Staircases (Straight, L-Corner/U-Turn Landing & Diagonal), Attic Ridge Alignment, Exact Floor Cutouts & Safety Railings:**
      - **Straight Staircases:** To climb from `Y_start` to `Y_upper`, place `Y_upper - Y_start` consecutive bottom-half stair blocks (`half: 'bottom'`, `shape: 'straight'`) shifting `1` block horizontally per `+1` rise, ending with the top stair at `Y_upper - 1` flush against the upper floor slab at `Y_upper`.
      - **Attic / Upper-Story Roof Clearance Rule:** Any staircase ascending into an attic or pitched-roof upper floor MUST terminate beneath the **high central roof ridge (`Y_roof_peak`)** — never ascending toward a low exterior eave slope — so `carveStairHeadroom(x, y, z, 3)` never punches a hole through the exterior roof shell.
      - **L-Corner (90°) & U-Turn (180°) Turning Landings (Preferred for 1-Block-Wide Turning Stairs):** Never force a `90°` turn directly on a sloped stair step. Instead, split the staircase into two straight flights connected by a **flat full-block turning landing** (`1 × 1` for 90° L-turn; `1 × 2` for 180° U-turn):
          - **Flight 1:** Ascends step-by-step (`+1` rise per `1` horizontal block, `shape: 'straight'`) until its top stair reaches `Y_turn`.
          - **Turning Landing (`Y_turn`):** Place a solid full block (or `top` slab) at `Y_turn` immediately after Flight 1's last stair, **flush with the top surface of that last stair** (`Y_turn`). Keep `2` blocks of air above the flat landing (`Y_turn + 1` and `Y_turn + 2`) — do NOT carve 3 blocks on flat landings.
          - **Flight 2:** Turns `90°` (or `180°`) from the turning landing; its **first stair starts at `Y_turn + 1`** (`1` block above the landing, `shape: 'straight'`) and continues ascending `+1` per horizontal step up to `Y_upper - 1`.
      - **Diagonal Corner-Mode Staircases:** Stairs may also ascend diagonally (`+1 X, +1 Y, +1 Z` per rise) using explicit single-block `outer_*` and `inner_*` stair shapes with a `2 × 2` walkable diagonal corridor so characters never clip wall corners.
      - **Exact Stairwell Floor Cutout & Safety Railing Rule:** Do NOT carve giant holes in upper floors\! Only remove upper floor tiles (`Y_upper`) directly above the **ascending stair steps** where `step.y + 3 >= Y_upper` (i.e., the top 3 stair steps) and above a turning landing only if `Y_turn + 2 >= Y_upper`, keeping the upper floor solid everywhere else — including beneath the **top exit landing** (`Y_upper`)\! Always border the open side of the upper stairwell cutout with a `*_fence` safety railing (leaving only the top exit landing side open) so characters cannot fall back down the stairwell.
      - **Interior Landing Bounds (Never Breach Exterior Walls):** Both the **bottom entry landing** (`-facing` at `Y_start`) and the **top exit landing** (`+facing` at `Y_upper`) MUST be **interior floor tiles** (`1 × 2` air height above solid floor, at least 1 block inside the exterior wall line `X_wall ± 1`, `Z_wall ± 1`), completely free of furniture, walls, railings, or doors. Never stack two staircases into the same column or let a landing breach an exterior wall.
      - Place staircases inset along solid interior wall lines (`X = 1` or `X = W - 2`), never cutting open exterior side walls.
      - Call `carveStairHeadroom(x, y, z, 3)` **only on the ascending stair steps** (not on flat entry, turning, or exit landings, where `2` blocks of air `y + 1..y + 2` suffice without punching through the ceiling/floor above).
3.  **Balconies, Porches & Fenced Decks (Minimum 1 Walkable Row Inside Railing):**
      - Because `fence` / `wall` railings occupy a full `1 × 1` block, NEVER make a fenced balcony only 1 block deep. Always include **at least 1 full row/column of unobstructed walkable floor (`1 × 2` air height) inside the outer railing line** (minimum total depth = **2 blocks**: `1` walkable floor + `1` outer railing; minimum total width for a 3-sided projecting balcony = **3 blocks**: `1` left rail + `1` walkable center + `1` right rail).
4.  **Beds (`2 × 1`), Chests, Workstations, Seating & 90° Corners:**
      - **Beds:** Use `placeBed(x, y, z, color, facing)` (`(x, y, z)` = pillow `head` position), keep `Y + 1` and `Y + 2` clear above the bed, and leave **≥ 1 walkable floor block (`1 × 2` air)** along the long side of the bed so characters wake safely without suffocating.
      - **Chests, Barrels, Workstations & Seats:** Leave **≥ 1 walkable floor block** in front (`+facing`) for access, plus **1 empty/transparent block above every `chest`** (`Y + 1`) so the lid opens freely.
      - **90° Corridor & Stair Turns:** Keep the `1 × 1` corner turning tile completely clear (`1 × 2` air in corridors, `1 × 3` in stairwells) with no corner pillars, flower pots, or low lanterns blocking turns.
5.  **Exterior Chimney Placement & 4-Sided Cap Rule:**
      - Place chimneys along an **exterior wall** (e.g., `X = 0` or `X = W - 1` outside the main roof slope) or rising cleanly through the central roof ridge (`Y_roof_peak`), extending ≥ 1 block above the roof peak capped with a lit `campfire`.
      - **4-Sided Chimney Cap Rule:** Because a roof narrows toward the peak, an exterior chimney stack that touches the wall lower down will be exposed to open air on all 4 sides at roof-peak height. Always check all 4 horizontal neighbors (`north`, `south`, `east`, `west`) around the top `campfire` at `Y_cap` and place an open `*_trapdoor` (`open: 'true'`, `half: 'top'`) on **every side that borders `air`**.
6.  **Lighting Density & Placement:**
      - Place ≥ 1 light source per `4 × 4` interior area (ceiling-hung `lantern` or wall-mounted light preferred). Never place torches directly touching beds.
7.  **T3 Circulation Map:**
      - For T3 compounds, include an explicit route list in Phase 3 showing how the main entrance connects to every building/room via ≥ 2-block exterior paths and ≥ 1-block interior doorways.

### Phase 4: Generate and Export the Standalone `.mjs` File (Clean & Crash-Free)

Generate and export the complete, self-contained ESM file (`<build-name>.mjs`) as a **downloadable `.mjs` file** (using the `file_gen` tool if available—do **NOT** dump the full `.mjs` source code as a raw Markdown code block in the chat response unless file generation is unavailable) using only built-in Node.js modules (`node:fs` and `node:zlib`):

  - **Do NOT include runtime `throw new Error` validation functions** (`validateBuild()`, `assertEmpty()`, etc.) in the generated script — architectural and technical compliance is guaranteed by the Coordinate Anchor Table and 5-Pass Constructive Layering in Phase 3 so the script always executes cleanly to completion.
  - Use `carveStairHeadroom(x, y, z, 3)` on ascending staircase steps in Pass 4 so upper floors/roofs never clip stair clearance while keeping flat landings intact.
  - Call `autoConnectStairs()`, `autoPairDoubleDoors()`, `connectDoubleChests()`, `connectWalls()`, and `connectFencesAndPanes()` at the end of Pass 5 so every roof corner stair, double door, double chest, `*_wall`, `*_fence_gate`, `*_fence`, `*_pane`, and `iron_bars` automatically computes valid connection/shape properties.
  - Serialize a gzip-compressed big-endian vanilla structure `.nbt` file with:
      - `DataVersion`: `5023` (Minecraft 26.3)
      - `size`: `[width, height, length]`
      - `palette`: deduplicated list of `{ Name, Properties }` compounds (all property values serialized as NBT strings)
      - `blocks`: deduplicated list of `{ pos: [x, y, z], state }` compounds
      - `entities`: empty list `[]` (unless the build explicitly needs villagers — see optional `addVillager()` helper below)

### Phase 5: Provide Run, Preview, and In-Game Load Commands

Always conclude the response with clear instructions:

1.  **Run with Node.js:**
    ``` bash
    node <build-name>.mjs            # writes <build-name>.nbt
    node <build-name>.mjs my-house   # writes my-house.nbt
    ```
2.  **3D Preview in Browser:** Open **https://www.shulkr.com/en/viewer** and drag-and-drop the generated `<build-name>.nbt` file to inspect the 3D build, facade depth, and interior layout.
3.  **Load in Minecraft:** Copy `<build-name>.nbt` to `.minecraft/saves/<world>/generated/minecraft/structures/<build-name>.nbt`, obtain a Structure Block (`/give @s structure_block`), switch to **LOAD** mode, enter `minecraft:<build-name>`, and click **LOAD**.

### Phase 6: Post-Build Self-Check (T2 & T3 only)

**Skip for T1a, T1b, and T1c.** For those tiers, report a short summary:

  - **T1a:** Dimensions (W×H×L), entrance clearance, furniture list (campfire, chest, bed).
  - **T1b:** Dimensions (W×H×L), functional element(s) present (water, light, connection), support/base integrity.
  - **T1c:** Dimensions (W×H×L), furniture type, seat/capacity count, material palette, orientation.

For **T2 & T3**, verify these architectural criteria against your Coordinate Anchor Table and generated code, and report the results in a concise table:

| Check | Source | Pass criteria | |---|---|---| | Room size vs function | `room-templates.md` | Every room ≥ minimum dimensions for its function | | Light source density | `architecture-principles.md` | ≥ 1 light per 4×4 interior area | | Roof style & overhang | `biome-styles.md` & `architecture-principles.md` | Roof shape matches biome + 1-block overhang + corner stairs joined | | Material palette & gradation | `architecture-principles.md` | ≤ 3 primary + 1 accent; heavy base, lighter upper walls | | Facade depth & silhouette | `architecture-principles.md` | Framing pillars + ≥ 2 exterior trim/silhouette details + 4-sided chimney cap | | Proportion | `architecture-principles.md` | Not 1:1:1 unless intentionally so | | The Sims & Grid Adjacency Standard | `SKILL.md` Phase 3 & `architecture-principles.md` | Doors/gates front & back clear; stairs 3-block headroom + ridge alignment + exact floor cutout + safety railing + interior bottom/top landings; beds/chests/workstations/stair-chairs oriented & locked (`shape: 'straight'`); double chests & double doors auto-paired; balconies ≥ 1 walkable block inside railing; walls/fences/panes auto-connected |

If any check falls short, revise the Coordinate Anchor Table in Phase 3 and update the script before responding.

## Standalone `.mjs` Template

``` javascript
import fs from 'node:fs';
import zlib from 'node:zlib';

class NbtWriter {
  constructor() { this.chunks = []; }
  u8(v) { const b = Buffer.alloc(1); b.writeUInt8(v); this.chunks.push(b); }
  i16(v) { const b = Buffer.alloc(2); b.writeInt16BE(v); this.chunks.push(b); }
  i32(v) { const b = Buffer.alloc(4); b.writeInt32BE(v); this.chunks.push(b); }
  str(s) { const buf = Buffer.from(s, 'utf8'); this.i16(buf.length); this.chunks.push(buf); }
  tagHeader(type, name) { this.u8(type); this.str(name); }
  tagInt(name, v) { this.tagHeader(3, name); this.i32(v); }
  tagString(name, v) { this.tagHeader(8, name); this.str(v); }
  tagListHeader(name, elemType, length) { this.tagHeader(9, name); this.u8(elemType); this.i32(length); }
  tagCompoundStart(name) { this.tagHeader(10, name); }
  tagEnd() { this.u8(0); }
  toBuffer() { return Buffer.concat(this.chunks); }
}

const SIZE = [11, 12, 13];
const palette = [];
const paletteMap = new Map();
const blockGrid = new Map();

const DIR_VEC = {
  north: [0, -1],
  south: [0, 1],
  west: [-1, 0],
  east: [1, 0],
};
const LEFT_OF = { north: 'west', west: 'south', south: 'east', east: 'north' };
const RIGHT_OF = { north: 'east', east: 'south', south: 'west', west: 'north' };

function getStateIndex(name, props = {}) {
  const keys = Object.keys(props).sort();
  const key = `${name}|${keys.map((k) => `${k}=${props[k]}`).join(',')}`;
  if (!paletteMap.has(key)) {
    paletteMap.set(key, palette.length);
    palette.push({ Name: name, Properties: props });
  }
  return paletteMap.get(key);
}

function setBlock(x, y, z, name, props = {}) {
  if (x < 0 || x >= SIZE[0] || y < 0 || y >= SIZE[1] || z < 0 || z >= SIZE[2]) return;
  const posKey = `${x},${y},${z}`;
  if (name === 'minecraft:air') {
    blockGrid.delete(posKey);
    return;
  }
  blockGrid.set(posKey, { pos: [x, y, z], name, props, state: getStateIndex(name, props) });
}

function fill(x1, y1, z1, x2, y2, z2, name, props = {}) {
  for (let x = Math.min(x1, x2); x <= Math.max(x1, x2); x++) {
    for (let y = Math.min(y1, y2); y <= Math.max(y1, y2); y++) {
      for (let z = Math.min(z1, z2); z <= Math.max(z1, z2); z++) {
        setBlock(x, y, z, name, props);
      }
    }
  }
}

function placeDoor(x, y, z, blockId, facing = 'south', hinge = 'left', open = 'false') {
  setBlock(x, y, z, blockId, { facing, half: 'lower', hinge, open, powered: 'false' });
  setBlock(x, y + 1, z, blockId, { facing, half: 'upper', hinge, open, powered: 'false' });
}

function placeBed(headX, y, headZ, blockId = 'minecraft:red_bed', facing = 'north') {
  const [dx, dz] = DIR_VEC[facing] ?? [0, -1];
  setBlock(headX, y, headZ, blockId, { facing, part: 'head', occupied: 'false' });
  setBlock(headX - dx, y, headZ - dz, blockId, { facing, part: 'foot', occupied: 'false' });
}

function carveStairHeadroom(x, y, z, height = 3) {
  for (let dy = 1; dy <= height; dy++) {
    setBlock(x, y + dy, z, 'minecraft:air');
  }
}

function autoConnectStairs(skipPositions = new Set()) {
  for (const b of blockGrid.values()) {
    if (!b.name.endsWith('_stairs')) continue;
    const [x, y, z] = b.pos;
    if (skipPositions.has(`${x},${y},${z}`)) continue;
    if (b.props.shape !== undefined) continue; // preserve explicit single-block corner/straight shapes
    const f = b.props.facing;
    const half = b.props.half ?? 'bottom';
    if (!f || !DIR_VEC[f]) continue;

    const getStairAt = (nx, ny, nz) => {
      const nb = blockGrid.get(`${nx},${ny},${nz}`);
      if (!nb || !nb.name.endsWith('_stairs')) return null;
      if ((nb.props.half ?? 'bottom') !== half) return null;
      return nb.props.facing;
    };

    const [fx, fz] = DIR_VEC[f];
    const [lx, lz] = DIR_VEC[LEFT_OF[f]];
    const [rx, rz] = DIR_VEC[RIGHT_OF[f]];
    const frontFacing = getStairAt(x + fx, y, z + fz);
    const backFacing = getStairAt(x - fx, y, z - fz);
    const leftFacing = getStairAt(x + lx, y, z + lz);
    const rightFacing = getStairAt(x + rx, y, z + rz);
    let shape = 'straight';

    // Vanilla Minecraft stair connection rules with lateral lock checks
    if (frontFacing === LEFT_OF[f] && rightFacing !== f) shape = 'outer_left';
    else if (frontFacing === RIGHT_OF[f] && leftFacing !== f) shape = 'outer_right';
    else if (backFacing === LEFT_OF[f] && leftFacing !== f) shape = 'inner_left';
    else if (backFacing === RIGHT_OF[f] && rightFacing !== f) shape = 'inner_right';

    const nextProps = {
      ...b.props,
      half,
      shape,
      waterlogged: b.props.waterlogged ?? 'false',
    };
    b.props = nextProps;
    b.state = getStateIndex(b.name, nextProps);
  }
}

function autoPairDoubleDoors() {
  const paired = new Set();
  for (const b of blockGrid.values()) {
    if (!b.name.endsWith('_door') || b.name === 'minecraft:iron_door') continue;
    if (b.props.half !== 'lower') continue;
    const [x, y, z] = b.pos;
    const key = `${x},${y},${z}`;
    if (paired.has(key)) continue;

    const f = b.props.facing ?? 'south';
    if (!DIR_VEC[f]) continue;
    // Viewer looking at door front sees LEFT_OF[f] on their right and RIGHT_OF[f] on their left
    const [lx, lz] = DIR_VEC[LEFT_OF[f]];
    const viewerRightKey = `${x + lx},${y},${z + lz}`;
    const neighborLower = blockGrid.get(viewerRightKey);

    if (
      neighborLower &&
      !paired.has(viewerRightKey) &&
      neighborLower.name === b.name &&
      neighborLower.props.half === 'lower' &&
      (neighborLower.props.facing ?? 'south') === f
    ) {
      const setDoorHinge = (dx, dz, hinge) => {
        for (const dy of [0, 1]) {
          const halfBlock = blockGrid.get(`${dx},${y + dy},${dz}`);
          if (halfBlock && halfBlock.name === b.name) {
            halfBlock.props = { ...halfBlock.props, hinge };
            halfBlock.state = getStateIndex(halfBlock.name, halfBlock.props);
          }
        }
      };
      setDoorHinge(x, z, 'left');
      setDoorHinge(x + lx, z + lz, 'right');
      paired.add(key);
      paired.add(viewerRightKey);
    }
  }
}

function connectDoubleChests() {
  const paired = new Set();
  for (const b of blockGrid.values()) {
    if (b.name !== 'minecraft:chest' && b.name !== 'minecraft:trapped_chest') continue;
    const [x, y, z] = b.pos;
    const key = `${x},${y},${z}`;
    if (paired.has(key)) continue;
    if (b.props.type === 'single') continue;

    const f = b.props.facing ?? 'north';
    if (!DIR_VEC[f]) continue;
    const [lx, lz] = DIR_VEC[LEFT_OF[f]];
    const [rx, rz] = DIR_VEC[RIGHT_OF[f]];

    const leftKey = `${x + lx},${y},${z + lz}`;
    const rightKey = `${x + rx},${y},${z + rz}`;
    const leftNeighbor = blockGrid.get(leftKey);
    const rightNeighbor = blockGrid.get(rightKey);

    const canPair = (nb, nKey) =>
      nb &&
      !paired.has(nKey) &&
      nb.name === b.name &&
      (nb.props.facing ?? 'north') === f &&
      nb.props.type !== 'single';

    if (canPair(leftNeighbor, leftKey)) {
      b.props = { ...b.props, facing: f, type: 'right', waterlogged: b.props.waterlogged ?? 'false' };
      b.state = getStateIndex(b.name, b.props);
      leftNeighbor.props = { ...leftNeighbor.props, facing: f, type: 'left', waterlogged: leftNeighbor.props.waterlogged ?? 'false' };
      leftNeighbor.state = getStateIndex(leftNeighbor.name, leftNeighbor.props);
      paired.add(key);
      paired.add(leftKey);
    } else if (canPair(rightNeighbor, rightKey)) {
      b.props = { ...b.props, facing: f, type: 'left', waterlogged: b.props.waterlogged ?? 'false' };
      b.state = getStateIndex(b.name, b.props);
      rightNeighbor.props = { ...rightNeighbor.props, facing: f, type: 'right', waterlogged: rightNeighbor.props.waterlogged ?? 'false' };
      rightNeighbor.state = getStateIndex(rightNeighbor.name, rightNeighbor.props);
      paired.add(key);
      paired.add(rightKey);
    } else {
      b.props = { ...b.props, facing: f, type: 'single', waterlogged: b.props.waterlogged ?? 'false' };
      b.state = getStateIndex(b.name, b.props);
      paired.add(key);
    }
  }
}

function isFullSolidForConnection(x, y, z) {
  const b = blockGrid.get(`${x},${y},${z}`);
  if (!b) return false;
  const n = b.name;
  const nonSolidSuffixes = [
    '_slab', '_stairs', '_door', '_trapdoor', '_carpet', '_bed', '_torch',
    '_sign', '_banner', '_button', '_plate', '_pane', '_fence', '_wall',
    '_glass', '_leaves', '_anvil', '_amethyst_bud', '_cluster',
  ];
  const nonSolidExact = new Set([
    'minecraft:glass', 'minecraft:tinted_glass', 'minecraft:ice',
    'minecraft:lantern', 'minecraft:soul_lantern', 'minecraft:campfire', 'minecraft:soul_campfire',
    'minecraft:iron_bars', 'minecraft:chain', 'minecraft:ladder', 'minecraft:vine',
    'minecraft:chest', 'minecraft:trapped_chest', 'minecraft:ender_chest', 'minecraft:barrel',
    'minecraft:lectern', 'minecraft:cauldron', 'minecraft:composter', 'minecraft:hopper',
    'minecraft:bell', 'minecraft:beacon', 'minecraft:glowstone', 'minecraft:sea_lantern',
    'minecraft:brewing_stand', 'minecraft:enchanting_table', 'minecraft:grindstone', 'minecraft:stonecutter',
  ]);
  if (nonSolidExact.has(n)) return false;
  if (nonSolidSuffixes.some((s) => n.endsWith(s))) return false;
  return true;
}

function connectWalls() {
  const canWallConnect = (x, y, z) => {
    const b = blockGrid.get(`${x},${y},${z}`);
    if (!b) return false;
    if (b.name.endsWith('_wall') || b.name.endsWith('_fence_gate')) return true;
    return isFullSolidForConnection(x, y, z);
  };

  for (const b of blockGrid.values()) {
    const [x, y, z] = b.pos;
    if (b.name.endsWith('_fence_gate')) {
      const f = b.props.facing ?? 'north';
      const [lx, lz] = DIR_VEC[LEFT_OF[f]] ?? [1, 0];
      const [rx, rz] = DIR_VEC[RIGHT_OF[f]] ?? [-1, 0];
      const leftB = blockGrid.get(`${x + lx},${y},${z + lz}`);
      const rightB = blockGrid.get(`${x + rx},${y},${z + rz}`);
      const inWall = (leftB && leftB.name.endsWith('_wall')) || (rightB && rightB.name.endsWith('_wall'));
      b.props = { ...b.props, in_wall: String(Boolean(inWall)) };
      b.state = getStateIndex(b.name, b.props);
      continue;
    }
    if (!b.name.endsWith('_wall')) continue;

    const above = blockGrid.get(`${x},${y + 1},${z}`);
    const tallSide = above && (above.name.endsWith('_wall') || isFullSolidForConnection(x, y + 1, z));
    const sideState = (nx, nz) => (canWallConnect(nx, y, nz) ? (tallSide ? 'tall' : 'low') : 'none');

    const north = sideState(x, z - 1);
    const south = sideState(x, z + 1);
    const west = sideState(x - 1, z);
    const east = sideState(x + 1, z);

    const straightNS = north !== 'none' && south !== 'none' && west === 'none' && east === 'none';
    const straightEW = west !== 'none' && east !== 'none' && north === 'none' && south === 'none';
    const up = !(straightNS || straightEW) || Boolean(above);

    b.props = {
      ...b.props,
      north,
      south,
      west,
      east,
      up: String(up),
      waterlogged: b.props.waterlogged ?? 'false',
    };
    b.state = getStateIndex(b.name, b.props);
  }
}

function connectFencesAndPanes() {
  const isConnectableTarget = (x, y, z, selfType) => {
    const b = blockGrid.get(`${x},${y},${z}`);
    if (!b) return false;
    const n = b.name;
    if (n.endsWith('_fence') && selfType === 'fence') return true;
    if (n.endsWith('_fence_gate')) return true;
    if ((n.endsWith('_pane') || n === 'minecraft:iron_bars') && selfType === 'pane') return true;
    return isFullSolidForConnection(x, y, z);
  };

  for (const b of blockGrid.values()) {
    const isFence = b.name.endsWith('_fence');
    const isPane = b.name.endsWith('_pane') || b.name === 'minecraft:iron_bars';
    if (!isFence && !isPane) continue;
    const [x, y, z] = b.pos;
    const type = isFence ? 'fence' : 'pane';
    const nextProps = {
      ...b.props,
      north: String(isConnectableTarget(x, y, z - 1, type)),
      south: String(isConnectableTarget(x, y, z + 1, type)),
      west: String(isConnectableTarget(x - 1, y, z, type)),
      east: String(isConnectableTarget(x + 1, y, z, type)),
      waterlogged: b.props.waterlogged ?? 'false',
    };
    b.props = nextProps;
    b.state = getStateIndex(b.name, nextProps);
  }
}

// --- Pass 1: Foundation, Pilings & Floor Slabs ---
// --- Pass 2: Framing Pillars, Exterior Walls, Windows, Balconies & Doors (placeDoor) ---
// --- Pass 3: Roof (with 1-block overhang) & Exterior Chimney (with 4-sided cap) ---
// --- Pass 4: Staircases (shape: 'straight'), Exact Floor Cutouts, Safety Railings & carveStairHeadroom() ---
// --- Pass 5: Interior Furniture, placeBed(), Wall Attachments & Lighting ---
autoConnectStairs();
autoPairDoubleDoors();
connectDoubleChests();
connectWalls();
connectFencesAndPanes();

const blocks = Array.from(blockGrid.values());
const w = new NbtWriter();
w.tagCompoundStart('');
w.tagInt('DataVersion', 5023);
w.tagListHeader('size', 3, 3);
SIZE.forEach((n) => w.i32(n));

w.tagListHeader('palette', 10, palette.length);
for (const entry of palette) {
  w.tagString('Name', entry.Name);
  const propKeys = Object.keys(entry.Properties);
  if (propKeys.length > 0) {
    w.tagCompoundStart('Properties');
    for (const k of propKeys) w.tagString(k, String(entry.Properties[k]));
    w.tagEnd();
  }
  w.tagEnd();
}

w.tagListHeader('blocks', 10, blocks.length);
for (const b of blocks) {
  w.tagListHeader('pos', 3, 3);
  b.pos.forEach((n) => w.i32(n));
  w.tagInt('state', b.state);
  w.tagEnd();
}

w.tagListHeader('entities', 10, 0);
w.tagEnd();

const BUILD_NAME = process.argv[2] ?? 'output';
fs.writeFileSync(`${BUILD_NAME}.nbt`, zlib.gzipSync(w.toBuffer()));
console.log(`✅ Wrote ${BUILD_NAME}.nbt (${blocks.length} blocks, palette ${palette.length})`);
```

### Optional: Villager Entities (only if the build needs occupants)

The template above writes `entities: []`, which is valid for any structural build. If a build must spawn villagers, extend `NbtWriter` with `f64`/`tagDouble` and nested compound support before serialization:

``` javascript
// Add to NbtWriter:
//   f64(v)   { const b = Buffer.alloc(8); b.writeDoubleBE(v); this.chunks.push(b); }
//   tagDouble(name, v) { this.tagHeader(6, name); this.f64(v); }

const entities = [];

function addVillager(x, y, z, profession = 'minecraft:none', biome = 'minecraft:plains') {
  entities.push({
    pos: [x + 0.5, y, z + 0.5],
    blockPos: [x, y, z],
    nbt: {
      id: 'minecraft:villager',
      VillagerData: { profession, type: biome, level: 1 },
    },
  });
}
```

If villagers are not required, keep the default `entities: []` so the script stays dependency-free and minimal.

## Reference Files

The `references/` directory is consulted **on demand** so the skill works seamlessly across every scale:

  - **Phase 1.5** (T2/T3 only) reads `architecture-principles.md`, `biome-styles.md`, and `room-templates.md` to lock the architectural plan.
  - **Phase 3** (T1c only) reads `furniture-patterns.md` for ergonomics and exact relative coordinate recipes.
  - **Phase 6** (T2/T3 only) verifies the build against those architectural standards.
  - **T1a & T1b builds skip all reference files** (unless a specific biome palette is requested) — they follow the universal rules in Phase 3 directly.

## Quick Tier Cheat Sheet

| User says... | Tier | Phases | References | |---|---|---|---| | "kursi", "meja makan", "rak", "bangku", "dipan" | T1c | 1, 2, 3 (ergonomics), 4, 5 | `furniture-patterns.md` | | "sumur", "lampu jalan", "patung", "jembatan", "pagar", "ladang" | T1b | 1, 2, 3 (support), 4, 5 | none (or `biome-styles.md`) | | "tenda camping", "shed", "hut", "pos jaga kecil" | T1a | 1, 2, 3 (conditional), 4, 5 | none (or `biome-styles.md`) | | "rumah villager", "toko", "cottage 2 lantai", "menara", "lighthouse" | T2 | 1, 1.5, 2, 3, 4, 5, 6 | all 3 architectural | | "kastil", "balai desa", "kompleks pasar", "katedral", "stadion" | T3 | 1, 1.5, 2, 3 (+routes), 4, 5, 6 | all 3, strict | | "redstone", "gunung", "chest berisi", "armor stand pose" | ❌ | — | out of scope | | Ambiguous | ask | — | — |
