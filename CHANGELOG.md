# Changelog

All notable changes to SuperDecor. The newest release is at the top. Dates are year-month-day.

## [version] (release date)

The first release of SuperDecor for the current Supermarket Simulator (Unity 6, IL2CPP). This is a
ground-up rebuild for the new engine. The previous version targeted the old Mono build of the game and is
not compatible with it.

### Added

- Buy and place decorative items from the game's Furnitures page.
- A built-in decor shop: browse installed items by category, open a detail panel, and buy through the
  game's normal purchase flow. Installed packs show up automatically.
- Decor Edit Mode (press B): pick up a placed item, reposition it, change its height, rotate it, nudge it
  along the surface, and swap your selection, with highlight outlines.
- Precision Mode (press G): on-screen move, rotate, and scale gizmos plus a numeric panel, with undo and
  redo and snapping.
- Animated decor that moves on its own, such as a clock, a fan, or a spinning sign.
- Interactive decor through code packs, including a showcase aquarium with swimming fish, rising bubbles,
  and a light you can switch on and off.
- An expansion-pack ecosystem: install a pack by dropping its `.zip` into
  `BepInEx/plugins/SuperDecor/Packs/`.
- The Expansion Pack Generator, a free browser tool that turns a 3D model into a drop-in pack with no
  code, including animated models.
- First-party example decor: a starter set plus the showcase aquarium.
- Documentation for players and creators: in-game features, pack authoring, the API reference, and code
  packs.

### Notes

- Decor is saved with a stable ID, so your placements survive reinstalls and game updates.
- A missing or broken model shows a placeholder instead of crashing.

---

Older Mono-era releases are not tracked here. This changelog starts with the rebuild.
