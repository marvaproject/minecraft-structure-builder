# 🧱 Minecraft Structure Builder Skills

A collection of AI agent skills for brainstorming, reverse-engineering, and generating static block-based **Minecraft structures** as standalone, zero-dependency **Node.js (`.mjs`) generators** that export vanilla **Minecraft `.nbt` structure files** (*Minecraft 26.3 / DataVersion 5023*).

Compatible with **Claude Code**, **Cursor**, **Windsurf**, **GitHub Copilot**, **Gemini CLI**, **Codex**, and other [Agent Skills](https://agentskills.io) compatible tools.

---

## 📦 Installation

Install all skills in this repository at once using [`npx skills`](https://github.com):

```bash
npx skills add marvaproject/minecraft-structure-builder --all
```

Or install a specific skill individually:

```bash
# Install only the text-to-structure generator skill
npx skills add marvaproject/minecraft-structure-builder --skill minecraft-builder

# Install only the image-to-structure reverse-engineering skill
npx skills add marvaproject/minecraft-structure-builder --skill minecraft-builder-img
```

To inspect available skills before installing:

```bash
npx skills add marvaproject/minecraft-structure-builder --list
```

---

## 🛠️ Available Skills

| Skill | Trigger & Purpose | Key Capabilities |
| :--- | :--- | :--- |
| **`minecraft-builder`** | Design & generate **any** Minecraft build from text prompts—ranging from single furniture pieces to complex compounds. | • **Coordinate Anchor Table** & **5-Pass Constructive Layering**<br>• Tiered architectural quality rules (furniture, civic builds, shelters, buildings, compounds)<br>• Built-in reference guides for architecture, biomes, furniture, and room templates<br>• Outputs a standalone `.mjs` script that generates vanilla `.nbt` files |
| **`minecraft-builder-img`** | Reverse-engineer an **uploaded image or screenshot** of a Minecraft build into a rebuildable structure file. | • Extracts exact dimensions, block palette, roof profile, and facade depth<br>• Infers hidden rear and interior layouts via a **Component Manifest**<br>• Skips brainstorming and immediately generates a standalone `.mjs` → `.nbt` script |

---

## 🚀 How It Works

1. **Prompt your AI Agent** with a build idea (e.g., *"Build a medieval blacksmith cottage with a working forge"*) or attach a screenshot of an existing Minecraft build.
2. **Structured Planning**: The skill guides the agent to map out coordinates, block states (`registry_26_3.json`), and layered construction passes so blocks connect properly without floating or clipping.
3. **Zero-Dependency Export**: The agent produces a self-contained Node.js script (e.g., `generate-build.mjs`). Run it locally with Node.js to output your `.nbt` file:
   ```bash
   node generate-build.mjs
   ```
4. **Load in Minecraft**: Place the generated `.nbt` file inside your world's `generated/minecraft/structures/` folder and load it in-game using a **Structure Block**.

---

## 📂 Repository Structure

```text
minecraft-structure-builder/
├── README.md
├── minecraft-builder/
│   ├── SKILL.md
│   ├── registry_26_3.json
│   └── references/
│       ├── architecture-principles.md
│       ├── biome-styles.md
│       ├── furniture-patterns.md
│       └── room-templates.md
└── minecraft-builder-img/
    ├── SKILL.md
    └── registry_26_3.json
```

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
