# Changelog

All notable changes to SuperDecor. The newest release is at the top. Dates are year-month-day.

## Framework Version 2.3.0 July 7th 2026
## API Version 2.3.0 July 7th 2026

This update localizes SuperDecor's own UI and cleans up the BepInEx config file so settings are easier
to read and adjust. It does not change the save format or expansion pack IDs.

### Added

- Localized SuperDecor-owned text for the current Steam interface languages: English, French, Italian,
  German, Spanish, Simplified Chinese, Brazilian Portuguese, Dutch, Japanese, Korean, Portuguese,
  Russian, Turkish, Danish, Finnish, Polish, Hungarian, Romanian, Czech, and Lithuanian.
- Localized in-game labels, buttons, tooltips, popups, notifications, status markers, sort labels,
  empty states, and fallback item or pack names.
- Localized BepInEx config section names, setting names, and setting descriptions.
- An embedded localization catalog inside the mod DLL, so the release package includes the language data
  automatically.

### Changed

- Rewrote awkward config text into clearer, player-facing names and descriptions.
- Existing config values migrate from the old section and key names to the new names when the mod loads,
  so current users keep their settings.
- Runtime UI now follows the game's selected language when possible and falls back to English when a
  language or string is missing.

### Notes

- Config descriptions are created when the plugin loads. If you change the game language, restart the
  game before checking the config file text.
- The public API version is now `2.3.0`, but no public method shape changed in this update.
- Expansion pack IDs, placed decor saves, and pack install paths are unchanged.

---

## Framework Version 2.0.1 June 9th 2026
## API Version 2.2.1 June 9th 2026

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
- First-party test objects: Blue cube, magenta breathing cube, cyan rotating cube, white rotating cube, chrome torus, yellow ribbon, orange ball
- Documentation for players and creators: in-game features, pack authoring, the API reference, and code
  packs.

### Notes

- Decor is saved with a stable ID, so your placements survive reinstalls and game updates.
- A missing or broken model shows a placeholder instead of crashing.

---

Older Mono-era releases are not tracked here. This changelog starts with the rebuild.
