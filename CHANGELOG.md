# Changelog

All notable changes to SuperDecor. The newest release is at the top. Dates are year-month-day.

## [version] (release date)

The first release of SuperDecor for the current Supermarket Simulator (Unity 6, IL2CPP). This is a
ground-up rebuild for the new engine. The previous version targeted the old Mono build of the game and is
not compatible with it.

### Added

- Purchase and place decorative items throughout your store.
- A built-in SuperDecor Shop: browse installed decor by category, inspect it in a detail panel, and
  purchase it through the game's standard checkout. Expansion decor is sold through the Shop and kept off
  the vanilla Furnitures page by default (configurable). Newly installed packs appear automatically.
- Decor Edit Mode (press B): pick up a placed item, reposition it, change its height, rotate it, nudge it
  along the surface, and swap your selection, with highlight outlines.
- Precision Mode (press G): on-screen move, rotate, and scale gizmos plus a numeric panel, with undo and
  redo, snapping, and a free-fly camera (WASD, ascend and descend, hold the middle mouse button to look).
- Animated decor that moves on its own, such as a clock, a fan, or a spinning sign.
- Interactive decor through code packs, including a Showcase aquarium with swimming fish, rising bubbles,
  and a light you can toggle.
- Physics decor: props you can bump and roll, such as the Orange Physics Ball. The framework holds them
  steady while you edit and restores their resting place on reload.
- An expansion-pack ecosystem: install a pack by dropping its `.zip` into
  `BepInEx/plugins/SuperDecor/Packs/`.
- The Expansion Pack Generator, a free browser tool that turns a 3D model into a drop-in pack with no
  code, including animated models.
- First-party example decor: a starter set plus the Showcase aquarium.
- Documentation for players and creators: in-game features, pack authoring, the API reference, and code
  packs.

### Notes

- Decor is saved with a stable ID, so your placements survive reinstalls and game updates.
- A missing or broken model shows a placeholder instead of crashing.

---

Older Mono-era releases are not tracked here. This changelog starts with the rebuild.
