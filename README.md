# SuperDecor

**SuperDecor** is a decoration framework for **Supermarket Simulator**. It lets you place decorative items
in your store and gives anyone the tools to create more. Items can be simple static props, animated decor
like a spinning sign or a working clock, or, with a code pack, interactive pieces that respond to the
player. It runs on BepInEx 6 for the Unity 6 (IL2CPP) version of the game.

This repository is the public home for SuperDecor and everything around it:

| What | Where |
|---|---|
| **Expansion Pack Generator**, turn a 3D model into a drop-in decor pack with no code | **[Open the tool](https://leptoon.github.io/superdecor-release/)** *(live)* |
| **SuperDecor mod**, downloadable builds (also on [Nexus Mods](https://www.nexusmods.com/supermarketsimulator/mods/1225)) | [Releases](https://github.com/leptoon/superdecor-release/releases) |
| **First-party decor content**, official packs by Leptoon | [`packs/`](packs/) and [Releases](https://github.com/leptoon/superdecor-release/releases) |

> **Status:** SuperDecor for the current Supermarket Simulator (Unity 6 + IL2CPP) is **complete**, with
> decoration placement and editing, animated decor, and a built-in decor shop. The first public release
> will be posted here and on Nexus once final testing wraps. The Expansion Pack Generator above is live.

## What it does in-game

- Buy and place decor from the game's **Furnitures** page and from a built-in **decor shop** that lists
  every installed item by category.
- Fine-tune placement with two editing modes: **Decor Edit Mode** (select, reposition, nudge, rotate,
  swap) and **Precision Mode** (on-screen move/rotate/scale gizmos, a numeric panel, and undo/redo).
- Animated decor plays on its own. Interactive decor from code packs (like the Showcase aquarium's
  switchable light) responds to the player.
- Add more decor by installing expansion packs.

Full details and the keybinds are in [In-game features](docs/FEATURES.md).

## For players

### Install the mod *(once a release is published)*

1. Install **Tobey's BepInEx Pack for Supermarket Simulator** (the IL2CPP variant, 0.10.0 or later).
2. Download the latest **SuperDecor** release from the [Releases](https://github.com/leptoon/superdecor-release/releases) page (or from [Nexus Mods](https://www.nexusmods.com/supermarketsimulator/mods/1225)).
3. Extract it so that the `SuperDecor/` folder lands in `<game>/BepInEx/plugins/`.

### Add expansion packs

Drop a pack's `.zip` straight into `<game>/BepInEx/plugins/SuperDecor/Packs/`. No unzip needed: the mod
reads both `.zip` archives and extracted `<packId>/` folders. Installed packs appear automatically in the
Furnitures page and the decor shop.

## Make your own decor

There are two ways to build a pack, and both register the same items with the same permanent IDs:

- **No code (the common case).** Open the [Expansion Pack Generator](https://leptoon.github.io/superdecor-release/),
  drop in a `.glb` / `.gltf` / `.fbx` / `.obj` model (static or animated), fill in a name, price, and
  category, and export a `<packId>.zip`. Drop that into `…/SuperDecor/Packs/`. No Unity, no .NET, nothing
  to compile. See [Creating packs](docs/CREATING_PACKS.md).
- **With C# (for custom behavior).** When an item needs a light, particles, or logic that reacts to the
  player, write a small code pack. See [Code packs](docs/CODE_PACKS.md).

Item IDs are derived deterministically from `packId` and each item's `internalName`, so a player's
placements survive reinstalls and updates. The flip side: renaming a `packId` or an item re-IDs it and
breaks existing saves, so treat those names as permanent once a pack is released. The full rule is in the
[API reference](docs/API_REFERENCE.md#deterministic-ids).

## Documentation

- [In-game features](docs/FEATURES.md): what the mod does and the keybinds.
- [Creating packs](docs/CREATING_PACKS.md): the no-code path, the generator, and asset prep.
- [Code packs](docs/CODE_PACKS.md): building a C# pack, the spawn hook, and a complete example.
- [API reference](docs/API_REFERENCE.md): the manifest schema, the C# API, and the deterministic-ID rules.

## License

- **Mod framework code, the Expansion Pack Generator, and documentation:** Apache License 2.0, intended to
  facilitate third-party expansion packs.
- **Decor assets (meshes, textures, icons) authored by Leptoon**, in the base mod or in first-party packs,
  are **All Rights Reserved.** Do not redistribute, alter, or sell them without explicit written
  permission.
- **Third-party expansion packs:** governed by their own authors' licenses.

---

*SuperDecor is not affiliated with or endorsed by the creators of Supermarket Simulator.*
