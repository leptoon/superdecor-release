# Creating packs (no code)

This is the path most people will take, you have a 3D model and want it to appear in Supermarket Simulator as
a purchasable, placeable decoration, without writing any C#, installing Unity, or installing the .NET SDK. If
you can export a `.glb` (or even just an `.fbx` or `.obj`), you can make a pack.

If you need custom in-game behavior (a toggle, particles, scripted logic), see [Code packs](CODE_PACKS.md)
instead. Everything else, including meshes, materials, textures, placement rules, price, and animation, is
data, and data packs cover it.

## The idea: a pack is data, not a program

A decor item is a set of values: a name, a price, a model file, a few placement settings. The framework
already implements the behavior (collision, placement, move, rotate, height adjust, box-up, selling at
50%); your pack just selects that behavior with data. So a pack is a folder of files described by one
`manifest.json`. No compiled DLL is required.

```
com.yourname.yourpack/
  manifest.json                 the file that describes your pack and its items
  models/coffee_machine.glb     your 3D model (mesh, material, and textures in one .glb)
  icons/coffee_machine.png      optional store icon (the generator can render this for you)
```

Release that folder as a `.zip`. Players drop it into `BepInEx/plugins/SuperDecor/Packs/`. Done.

## The recommended path: the Expansion Pack Generator

The [Expansion Pack Generator](https://leptoon.github.io/superdecor-release/) is a free browser tool. No
install, no account, nothing leaves your machine.

1. Open the generator.
2. Set your **pack details**: a name, your author name, a version, and a **pack id** in reverse-domain
   form (`com.yourname.yourpack`). Lock the pack id. It is permanent once you release (see
   [The two names you can never change](#the-two-names-you-can-never-change)).
3. For each item, click **Add item** and drop in your model (`.glb`, `.gltf`, `.fbx`, or `.obj`, plus any
   loose texture files). The tool converts it to a single `.glb` with the textures embedded, so you never
   have to pick a format.
4. Fill in the short form: display name, description, price, category, a base reference (pick by
   footprint), and placement tags.
5. The tool renders a store **icon** for you automatically. You can upload your own instead.
6. If your model is **animated**, enable **autoplay** and select the clip in the Animation section. (The
   section only appears when your file actually contains an animation. If it is missing, your export
   dropped the clip; see [Asset tips](#asset-tips).)
7. Click **Export data pack**. You get a `<packId>.zip`. Drop it into
   `<game>/BepInEx/plugins/SuperDecor/Packs/`.

The generator can author every supported manifest field, including material tints, sounds, scaling, pack
dependencies, and a custom base reference. For the full field list, see the
[API reference](API_REFERENCE.md#manifest-schema).

## The two names you can never change

Two values become a permanent in-game ID:

- `packId` (per pack), for example `com.yourname.cafepack`.
- `internalName` (per item), for example `espresso_machine`.

The framework turns `packId` plus `internalName` into the item's permanent ID. That ID is written into a
player's save when they place your item. **If you rename either in a later version, the item gets a new ID
and players who placed the old one will see it vanish from their saved stores.** Pick these two carefully,
once, and treat them as permanent. Display names, descriptions, prices, and even the model file are safe
to change anytime. Only the two ID inputs are frozen. Full rules: [Save-safe naming](API_REFERENCE.md#deterministic-ids).

Use reverse-domain form for `packId` and `lowercase_with_underscores` for `internalName`, unique within
your pack.

## Choosing a base reference

The base reference tells the framework which existing furniture your item should behave like, and that
choice also sets the item's delivery box size. You do not set box size separately. Start with a small,
medium, or large base that roughly matches your item's footprint, then test in-game and adjust. Common
references are `2` (small), `3` (medium), and `4` (large).

## Asset tips

### Model

- A single `.glb` carries the mesh, the PBR material, and the textures together, which is why it is the
  no-fuss format. Export `.glb` from Blender (File, Export, glTF 2.0, format glTF Binary) or from Unity
  with a glTF exporter, or let the generator convert your `.fbx` or `.obj`.
- Keep meshes reasonable: a few thousand triangles for a typical prop, not hundreds of thousands.
- Textures of 512 by 512 or 1024 by 1024 are plenty for a store prop. Embed them in the `.glb`.
- Model to roughly real-world scale and face the model forward. The framework applies your manifest
  transform (scale, offset, rotation) on top of the model, so you can nudge placement without
  re-exporting.
- Author a standard PBR metallic-roughness material. That maps cleanly onto the game's renderer. Exotic
  shader graphs do not survive export; bake what you need into the base-color, normal, and
  metallic-roughness maps.

### Animation (optional, still no code)

Animated decor is a normal data pack with an animated `.glb`. The framework plays the clip at runtime.

- Put the motion in an animation clip and give it a clear name (`Spin`, `Idle`, `Swim`).
- Export with animation enabled, then confirm the clip survived by opening the `.glb` in any glTF viewer.
  If there is no animation track, it will not play in-game.
- In the generator (or the manifest), enable autoplay and select the clip.

The framework supports node motion (a spinning blade), morph or blend-shape deformation, and full
skeletal rigs (a fish that bends as it swims).

### Icon

Optional. If you skip it, the generator renders one. For a hand-made icon, supply a square PNG (128 by 128
or 256 by 256) of your model on a clean background.

## Authoring a pack by hand (optional)

You do not have to use the generator. With a text editor and a `.glb` exporter you can write the
`manifest.json` yourself, drop your model under `models/`, add an optional icon under `icons/`, and zip
the folder. That is exactly what the generator produces. The full schema is in the
[API reference](API_REFERENCE.md#manifest-schema).

## Install and test

1. Copy the `.zip` (or the extracted `<packId>/` folder) into
   `<game>/BepInEx/plugins/SuperDecor/Packs/`.
2. Launch the game. Open the in-game computer, open the **SuperDecor Shop**, locate your item, purchase it, and place it.
3. If an item does not appear, the most common causes are a malformed `manifest.json`, a `model` path
   that does not match the file on disk, or a duplicate `internalName` within the pack. Check
   `BepInEx/LogOutput.log` for registration warnings.

## Versioning safely

- **Safe to change anytime:** display name, description, price, category, the model file, icon, transform,
  placement settings, lighting, sounds, and the pack `version`.
- **Never change after release:** `packId` and any item's `internalName`. These set the permanent IDs.
- Bump `version` on every release so players can tell builds apart.
