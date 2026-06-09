# In-game features

What SuperDecor adds to Supermarket Simulator, and how to use it.

## Decor items

SuperDecor lets you purchase and place decorative items throughout your store. The base mod and most expansion packs are cosmetic props, but the framework supports considerably more:

- **Static props:** signs, plants, carts, baskets, bins, and the like.
- **Animated decor:** items that animate on their own, such as a wall clock, a pedestal fan, or a spinning sign. The motion plays automatically once the item is placed; no player setup is required.
- **Interactive decor:** items that respond to the player, supplied by code packs. The first-party Showcase aquarium, for instance, has fish that swim, rising bubbles, and an internal light you can toggle.
- **Physics decor:** props with genuine physics that you can bump and roll around the store, like the example Orange Physics Ball. The framework holds them steady while you reposition them and restores their resting place when you reload.

## Buying decor: the SuperDecor Shop

Expansion decor is purchased through the **SuperDecor Shop**, a browsing overlay you open from the in-game market computer. It presents every installed item, organized by category, with a detail panel for the current selection, and it adds your choices to the cart and checks out through the game's standard purchase flow. The Shop reads whatever packs you have installed, so newly added packs appear without any configuration.

By default, expansion decor is kept off the vanilla Furnitures page, so the Shop is its single, uncluttered home. If you would rather browse it alongside the stock furniture as well, set `HideExpansionFromVanillaPage` to `false` in the `[SuperDecorShop]` section of the config file.

## Placing and editing decor

Placed decor behaves like any other furniture. On top of the game's placement, SuperDecor provides two editing modes for precise control.

### Decor Edit Mode (press B)

The primary mode for adjusting items already placed in your store. While it is active:

- Aim at a placed item and **Left-Click** to select it for editing, or to switch your selection to a different item.
- Adjust its **height**, **anchor it to or release it from** the surface, and **rotate** it.
- **Fine-nudge** the item along the surface with the arrow keys (and `Y` / `U` along the surface normal).
- The item you are aiming at is outlined. Press **H** to cycle the outline between aimed, all, and off.

Press **B** again with empty hands to leave Decor Edit Mode.

### Precision Mode (press G)

A sub-mode of Decor Edit Mode for exact placement. While editing an item, press **G** for on-screen **move, rotate, and scale gizmos** plus a numeric panel with **undo and redo** and snapping. The camera detaches for free navigation: **WASD** moves you across the floor, **Space** and **Z** ascend and descend (a creative-style fly, with no gravity), and **holding the middle mouse button** lets you look around freely with no screen-edge limit. Press **Esc** or right-click to return to Decor Edit Mode; an airborne exit settles gently back to the ground.

### Keybinds

| Key | When | Action |
|---|---|---|
| `B` | anytime | Toggle Decor Edit Mode. With an item held, the first press releases it in place; a second press (empty hands) closes the mode. |
| Left-Click | Decor Edit Mode, empty hands | Select the item you are aiming at. |
| `G` | editing an item | Toggle Precision Mode (gizmos and numeric panel). |
| `H` | Decor Edit Mode | Cycle the outline: aimed, all, off. |
| Arrow keys, `Y`, `U` | item held | Fine-nudge along the surface and its normal (hold a modifier for a coarser step). |
| Scroll, `Q`, `E` | item held | Rotate (hold `Alt` to pitch, `Ctrl` for 45-degree steps). |
| `Tab` | item held | Anchor to, or release from, the surface. |
| Right-Click | item held | Two-stage: anchor, then commit. In Precision Mode, the first right-click exits Precision Mode. |
| `WASD`, `Space`, `Z`, MMB | Precision Mode | Free navigation: WASD moves, Space and Z fly up and down, hold the middle mouse button to look around. |
| `Esc` | layered | Step back one layer at a time: Precision Mode, then Decor Edit Mode, then the game menu. |
| `F4` | anytime | Show or hide the edit-mode info bar (returns to shown on restart). |

## Expansion packs

Anyone can add more decor through expansion packs. To install one, drop its `.zip` into `<game>/BepInEx/plugins/SuperDecor/Packs/`. No extraction is needed: the mod reads both `.zip` archives and unpacked pack folders. Installed packs appear in the SuperDecor Shop automatically.

To create your own, see [Creating packs](CREATING_PACKS.md) (no code) and [Code packs](CODE_PACKS.md) (custom C#).

## Save compatibility

Each decor item carries a permanent in-game ID derived from its pack and item names, so the decor you place persists across reinstalls and game updates. See [Save-safe naming](API_REFERENCE.md#deterministic-ids) in the API reference for the details that matter to pack authors.
