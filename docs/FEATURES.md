# In-game features

What SuperDecor adds to Supermarket Simulator, and how to use it.

## Decor items

SuperDecor adds decorative items you can buy and place in your store. The base mod and most expansion
packs are cosmetic props, but the framework supports more:

- **Static props:** signs, plants, carts, baskets, bins, and so on.
- **Animated decor:** items that move on their own, such as a wall clock, a pedestal fan, or a spinning
  sign. Animation plays automatically once the item is placed. No setup is needed by the player.
- **Interactive decor:** items that respond to the player. These come from code packs. The first-party
  Showcase aquarium, for example, has fish that swim, rising bubbles, and an internal light you can switch
  on and off.

Every item appears in the game's **Furnitures** page (in the in-game computer) with its own icon and
price, alongside the vanilla furniture.

## The decor shop

SuperDecor adds its own decor shop: a browsing overlay you open from the in-game market computer. It
lists every installed decor item, groups them by category, shows a detail panel for the selected item,
and lets you add items to your cart and buy them through the game's normal purchase flow. The shop reads
whatever packs you have installed, so new packs show up automatically.

## Placing and editing decor

Bought decor is placed like any furniture. On top of the game's placement, SuperDecor adds two editing
modes for fine control.

### Decor Edit Mode (press B)

The everyday editing mode for items already placed in your store. With it on:

- Aim at a placed item and **Left-Click** to pick it up for editing, or to swap your selection to a
  different item.
- Adjust **height**, **anchor or follow** the surface, and **rotate** the item.
- **Fine nudge** the item along the surface with the arrow keys (and `Y` / `U` for the surface normal).
- Aimed items show a highlight outline. Press **H** to cycle the highlight between aimed, all, and off.

Press **B** again (with empty hands) to leave Decor Edit Mode.

### Precision Mode (press G)

A sub-mode of Decor Edit Mode for exact placement. While editing an item, press **G** to get on-screen
**move, rotate, and scale gizmos** plus a numeric control panel with **undo and redo** and snapping. The
camera freezes so you can line up the shot; move it with `WASD` and middle-mouse look. Press **Esc** or
right-click to step back out to Decor Edit Mode.

### Keybinds

| Key | When | Action |
|---|---|---|
| `B` | anytime | Toggle Decor Edit Mode. With an item held, the first press drops it in place; a second press (empty hands) closes the mode. |
| Left-Click | Decor Edit Mode, empty hands | Select the item you are aiming at. |
| `G` | editing an item | Toggle Precision Mode (gizmos and numeric panel). |
| `H` | Decor Edit Mode | Cycle the highlight: aimed, all, off. |
| Arrow keys, `Y`, `U` | item held | Fine nudge along the surface and its normal (hold a modifier for a coarser step). |
| Scroll, `Q`, `E` | item held | Rotate (hold `Alt` to pitch, `Ctrl` for 45-degree steps). |
| `Tab` | item held | Anchor or unanchor to the surface. |
| Right-Click | item held | Two-stage: anchor, then commit. In Precision Mode, the first right-click exits Precision Mode. |
| `Esc` | layered | Back out one layer at a time: Precision Mode, then Decor Edit Mode, then the game menu. |
| `F4` | anytime | Show or hide the edit-mode info bar (resets to shown on restart). |

## Expansion packs

Anyone can add more decor through expansion packs. As a player, you install one by dropping its `.zip`
into `<game>/BepInEx/plugins/SuperDecor/Packs/`. No unzip needed: the mod reads both `.zip` archives and
extracted pack folders. Installed packs appear automatically in the Furnitures page and the decor shop.

To make your own packs, see [Creating packs](CREATING_PACKS.md) (no code) and
[Code packs](CODE_PACKS.md) (custom C#).

## Save compatibility

Each decor item has a permanent in-game ID derived from its pack and item names, so the decor you place
stays put across reinstalls and game updates. See [Save-safe naming](API_REFERENCE.md#deterministic-ids)
in the API reference for the details that matter to pack authors.
