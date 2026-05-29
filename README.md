# SuperDecor

**SuperDecor** is a decoration framework for **Supermarket Simulator** — it adds non-functional decorative items to dress up your store, plus a third-party **expansion-pack ecosystem** so anyone can add their own decor. It's a BepInEx 6 / Unity 6 (IL2CPP) mod.

This repository is the **public home** for SuperDecor and everything around it:

| | What | Where |
|---|---|---|
| 🧰 | **Expansion Pack Generator** — turn a 3D model into a drop-in decor pack, no code | **[Open the tool ▸](https://leptoon.github.io/superdecor-release/)** *(live)* |
| 📦 | **SuperDecor mod** — downloadable builds (also on [Nexus Mods](https://www.nexusmods.com/supermarketsimulator/mods/1225)) | [Releases](https://github.com/leptoon/superdecor-release/releases) |
| 🎨 | **First-party decor content** — official packs by Leptoon | [`packs/`](packs/) + [Releases](https://github.com/leptoon/superdecor-release/releases) |

> **Status (2026-05):** the IL2CPP rewrite for current Supermarket Simulator (Unity 6 + IL2CPP) is **code-complete** and in final in-game verification. The first IL2CPP release will be published here and on Nexus once verified — the **Expansion Pack Generator above is live now.**

## For players

### Install the mod *(once a release is published)*

1. Install **Tobey's BepInEx Pack for Supermarket Simulator** — the IL2CPP variant (0.10.0+).
2. Download the latest **SuperDecor** release from the [Releases](https://github.com/leptoon/superdecor-release/releases) page (or from [Nexus Mods](https://www.nexusmods.com/supermarketsimulator/mods/1225)).
3. Extract it so that the `SuperDecor/` folder lands in `<game>/BepInEx/plugins/`.

### Add expansion packs

Drop a pack's `.zip` straight into `<game>/BepInEx/plugins/SuperDecor/Packs/` — no unzip needed. The mod loads `.zip` archives and extracted `<packId>/` folders alike.

## Make your own decor — no code

Open the **[Expansion Pack Generator](https://leptoon.github.io/superdecor-release/)**, drop in a `.glb` / `.gltf` / `.fbx` / `.obj` model, fill in a name / price / category, and export a `<packId>.zip`. Drop that into `…/SuperDecor/Packs/`. No Unity, no .NET, nothing to compile.

Item IDs are derived deterministically by the framework — `SHA-256(packId + ":" + internalName)` — so a player's placements survive reinstalls. The flip side: renaming a `packId` or an item re-IDs it and breaks existing saves, so treat those names as permanent once a pack is released.

## First-party decor content

Official decor packs by Leptoon are published on the [Releases](https://github.com/leptoon/superdecor-release/releases) page as `.zip` assets; [`packs/`](packs/) tracks what's available. Their **decor assets are All Rights Reserved** (see License).

## License

- **Mod framework code, the Expansion Pack Generator, and documentation:** Apache License 2.0 — intended to facilitate third-party expansion packs.
- **Decor assets (meshes, textures, icons) authored by Leptoon** — in the base mod or in first-party packs — are **All Rights Reserved.** Do not redistribute, alter, or sell them without explicit written permission.
- **Third-party expansion packs:** governed by their own authors' licenses.

---

*SuperDecor is not affiliated with or endorsed by the creators of Supermarket Simulator.*
