---
name: minecraft-builder-img
description: "Analyze an uploaded image or screenshot of a Minecraft build, reverse-engineer its exact dimensions, block palette, roof profile, facade depth, and hidden rear/interior layout into a detailed Component Manifest and Coordinate Anchor Table, and immediately output a downloadable standalone zero-dependency Node.js (.mjs) generator file that exports a vanilla Minecraft .nbt structure file (Minecraft 26.3 / DataVersion 5023) without brainstorming questions. Use whenever the user shares an image/screenshot of a Minecraft structure, house, statue, furniture, or monument and wants it analyzed and rebuilt as an .nbt file or JS generator."
---
# Minecraft Image Builder

Reverse-engineer any uploaded image or screenshot of a Minecraft structure into an exact **Component Manifest**, **Coordinate Anchor Table**, and a **downloadable standalone, zero-dependency ESM (`.mjs`) file** (using the file generation tool rather than dumping raw code blocks in chat) that exports a gzip-compressed vanilla `.nbt` file for Minecraft 26.3 (`DataVersion: 5023`) **immediately in a single pass without brainstorming questions**.

## When to Use

  - The user provides an image, screenshot, or render of a Minecraft build (house, cottage, tower, bridge, well, statue, furniture, temple, or compound) and asks to recreate, copy, reverse-engineer, or generate it into `.nbt` or Node.js code.
  - The user wants direct visual breakdown + structured block/component list + immediate `.mjs` file output without back-and-forth clarification.

## Out of Scope

  - **Text-only ideation from scratch** (no image provided) - defer to `minecraft-builder` for brainstorming and architectural design from scratch.
  - **Redstone circuits, custom terrain/terraforming, chest NBT contents, or entity posing** - state the limitation clearly and reconstruct the static block shell instead.

## Folder Layout

    minecraft-image-builder/
    ├── SKILL.md
    └── registry_26_3.json

## Workflow (Direct Execution - No Brainstorming)

Do **not** ask clarifying or brainstorming questions when an image is provided. Execute all four phases sequentially in a single response:

### Phase 1: Deep Visual Breakdown & Component Manifest

Inspect the image carefully from bottom to top and front to back, then output a concise, structured **Blueprint & Component Manifest**:

1.  **Multi-Panel Technical Sheet & Axis Alignment (If Applicable):**
      - When the uploaded image is a multi-panel technical reference sheet (e.g., `01. FRONT-RIGHT 3/4 VIEW`, `02. REAR-LEFT 3/4 VIEW`, `03. INTERIOR / STRUCTURAL DETAIL VIEW`), synthesize all panels together rather than treating them as separate builds.
      - Lock `X`, `Y`, and `Z` orientation strictly to Panel 01's 3D XYZ axis gizmo (red `X`, green `Y` up, blue `Z`) and any `"FRONT"` floor label. Ignore flipped or duplicated gizmo arrows in Panel 02 if they conflict with Panel 01's camera rotation.
      - Cross-check Panel 01 and Panel 02 to extract all 4 exterior facades without guessing, and prioritize Panel 03's cutaway layout for interior room partitions, staircases, and furniture placement.
      - Ignore accidental duplicate props in Panel 03 (such as an extra lantern or sign) that do not appear in both Panel 01 and Panel 02.
2.  **Overall Dimensions & Proportions (Grid Triangulation):**
      - Estimate exact bounding box `SIZE = [W, H, L]` by counting visible blocks along width (`X`), height (`Y`), and depth (`Z`), keeping non-1:1:1 proportions faithful to the screenshot.
      - If walls or surfaces appear smooth/non-voxel in an AI-generated reference image, triangulate exact block counts from the `1x1` floor grid lines, corner framing pillars, window panes, and roof stairs rather than guessing from smooth wall height.
      - Define elevation datum levels: `Y_base`, `Y_floor1`, `Y_floor2` (if multi-story), `Y_roof_start`, and `Y_roof_peak`.
3.  **Material Palette Breakdown (Matched to `registry_26_3.json`):**
      - **Foundation / Plinth:** e.g., `cobblestone`, `stone_bricks`, `tuff_bricks`, `deepslate_bricks`.
      - **Framing Pillars & Beams:** e.g., `oak_log`, `spruce_log`, `stripped_dark_oak_log` with `axis`.
      - **Primary & Secondary Walls:** e.g., `oak_planks`, `white_concrete`, `calcite`, `mud_bricks`.
      - **Roofing & Trim:** e.g., `spruce_stairs`, `brick_stairs`, `deepslate_tile_stairs`, slabs, and overhang trim.
      - **Openings, Lighting & Exterior Details:** windows (`glass_pane`), shutters (`*_trapdoor`), doors (`*_door`), fences/walls, lanterns, chimneys, foliage/vines, and decorative props visible in the shot.
4.  **Rear & Interior Reconstruction Plan (Occluded Sides):**
      - For any faces or interior corners still hidden across all panels, infer a symmetrical or functionally coherent layout (matching window rhythm and roof slope) and furnish the interior cleanly following **The Sims Navigation Standard** (beds/workstations/displays, lighting every `4x4` area, and walkable `1x2` paths) without asking the user.

### Phase 2: Block Registry Verification (`registry_26_3.json`)

  - Verify every chosen block ID (`minecraft:<name>`) and state property against `registry_26_3.json` in the skill directory (Minecraft 26.3 / `DataVersion: 5023`).
  - Never invent block IDs or enum values. If a modded, smooth, or non-voxel prop appears in the screenshot (e.g., smooth plaster wall, AC unit, display fridge, text sign), translate it into the closest valid vanilla 26.3 block combination from `registry_26_3.json` and note the mapping in the Component Manifest.

### Phase 3: Coordinate Anchor Table & 5-Pass Constructive Rules

Map the Component Manifest into an exact **Coordinate Anchor Table** (`X, Y, Z`) before writing code:

1.  **Shell & Framing Pillars:** Corner post `(X, Z)` coordinates and exterior wall planes.
2.  **Interior Floor Bounds (`X_min..X_max`, `Z_min..Z_max`):** Ensure all interior furniture stays strictly inside `X_wall_west + 1 .. X_wall_east - 1` and `Z_wall_north + 1 .. Z_wall_south - 1` so Pass 5 never overwrites walls.
3.  **Doors, Windows & Balconies:** Exact `(X, Y, Z)` + `facing`, keeping `1-2` walkable floor tiles clear in front and behind doors/gates, and ensuring any fenced balcony has `>= 1` walkable floor row inside its outer railing (`depth >= 2`).
4.  **Staircases & Attic Ridge Alignment:**
      - Lock walkable staircase steps and stair-chairs with explicit `shape: 'straight'` so `autoConnectStairs()` only shapes roof stairs.
      - Any staircase rising into a pitched-roof upper floor/attic MUST terminate beneath the high central roof ridge (`Y_roof_peak`) so `carveStairHeadroom(x, y, z, 3)` never punches through the exterior roof slope.
      - For turning stairs, use a flat `1x1` (L-turn) or `1x2` (U-turn) full-block landing flush with Flight 1's top stair (`Y_turn`), keeping `2` blocks of air above flat landings and starting Flight 2 at `Y_turn + 1`.
      - Remove upper floor tiles only directly above ascending steps where `step.y + 3 >= Y_upper` and border the open stairwell edge with a `*_fence` safety railing.
5.  **Discrete Grid Adjacency & Chimney Cap:**
      - Wall-mounted items (`wall_torch`, `*_wall_sign`, `ladder`, `trapdoor` shutters, `vine`) occupy the adjacent air block (`X_wall +/- 1` or `Z_wall +/- 1`), never overwriting wall blocks.
      - For exterior chimneys extending above the roof peak, check all 4 horizontal neighbors around the top `campfire` and place open `*_trapdoor` (`open: 'true'`, `half: 'top'`) on every side bordering `air`.

### Phase 4: Immediate Standalone `.mjs` File Output & Run Instructions

Generate and export the complete, self-contained ESM file (`<build-name>.mjs`) as a **downloadable `.mjs` file** (using the `file_gen` tool if available—do **NOT** dump the full `.mjs` source code as a raw Markdown code block in the chat response unless file generation is unavailable) using only `node:fs` and `node:zlib` in **5-Pass Constructive Layering** order:

  - **Pass 1:** Foundation, pilings, and floor slabs.
  - **Pass 2:** Framing pillars, exterior walls, windows, balconies, and doors (`placeDoor`).
  - **Pass 3:** Roof (with 1-block overhang) and exterior chimney (with 4-sided cap).
  - **Pass 4:** Staircases (`shape: 'straight'`), exact stairwell floor cutouts, safety railings, and `carveStairHeadroom()`.
  - **Pass 5:** Interior furniture (inside room bounds), `placeBed()`, wall attachments, lighting, followed by `autoConnectStairs()`, `autoPairDoubleDoors()`, `connectDoubleChests()`, `connectWalls()`, and `connectFencesAndPanes()`.

Do **not** include runtime `throw new Error` validation functions so the script always executes cleanly. Always conclude with brief run, Shulkr 3D Viewer preview (`https://www.shulkr.com/en/viewer`), and Minecraft Structure Block load commands.

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
    if (b.props.shape !== undefined) continue;
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
