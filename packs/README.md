# Decor packs

This folder tracks decor packs published for SuperDecor.

A **data pack** is a single `<packId>.zip` — a `manifest.json` + `models/*.glb` + `icons/*.png` — that drops into `<game>/BepInEx/plugins/SuperDecor/Packs/`. Build one with no code using the **[Expansion Pack Generator](https://leptoon.github.io/superdecor-release/)**.

You provide the mesh (and optionally a scale/offset/rotation, an icon, materials, and a few placement tags). The framework handles the rest — a solid mesh collider is built from your model automatically, so players can stand on top, walk around, and never clip through. There is no collision-mode or walkability knob to set.

- **First-party packs** (by Leptoon) are published as `.zip` assets on the [Releases](https://github.com/leptoon/superdecor-release/releases) page. Their decor assets are All Rights Reserved — see the root [README](../README.md#license).
- **Community packs:** authors host their own; notable ones may be linked here.

*No first-party packs are published yet — this is their home once the IL2CPP release ships.*
