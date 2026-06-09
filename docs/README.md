# SuperDecor documentation

How to use SuperDecor, and how to build your own decor. Start with the section that fits you.

## For players

- **[Features and controls](FEATURES.md):** what SuperDecor adds to the game and how to
  operate it. Buying decor from the in-game SuperDecor Shop, placing and arranging it in
  Decor Edit Mode, fine-tuning with Precision Mode, and the full keybind list.

## For pack creators

Two paths produce a pack. Most creators never write a line of code; the second path exists
for the rare item that needs custom behavior.

- **[Creating packs, no code](CREATING_PACKS.md):** the recommended path. Turn a 3D model
  into a purchasable, placeable decoration with the Expansion Pack Generator, or hand-author
  the same pack in a text editor. Covers models, icons, animation, and save-safe naming.
- **[Code packs](CODE_PACKS.md):** for items that need C#. A toggleable light, particles,
  scripted interaction, or a physics prop you can bump and roll, with the showcase aquarium
  as a worked example.
- **[API reference](API_REFERENCE.md):** the complete reference. The full `manifest.json`
  schema, the `DecorExpansionAPI` surface, the deterministic-ID rules that keep player
  placements stable across updates, and the decor categories.

## Build a pack right now

The [Expansion Pack Generator](https://leptoon.github.io/superdecor-release/) runs in your
browser. Drop in a model, set a name and a price, and export a pack. Nothing to install, and
nothing leaves your machine.

## Elsewhere in this repository

- **[Main README](../README.md):** what SuperDecor is, how to install it, and a tour of what
  it does.
- **[Changelog](../CHANGELOG.md):** what changed in each release.
