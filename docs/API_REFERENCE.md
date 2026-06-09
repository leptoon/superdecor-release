# API reference

The shared contract for SuperDecor packs. Both authoring paths use it:

- **Data packs (no code)** describe items in a `manifest.json` (see [Manifest schema](#manifest-schema)).
- **Code packs (C#)** register the same items through the `DecorExpansionAPI` (see
  [The C# API](#the-c-api)).

Both produce the **same** items with the **same** permanent IDs, so a pack can move from a data pack to a
code pack later without changing any item ID and without breaking players' saves.

- **Framework:** `com.leptoon.superdecor` (`SuperDecor.dll`).
- **API version:** `2.2.0`.
- **Game:** Supermarket Simulator 9.6.0.0+ (Unity 6, IL2CPP), with Tobey's BepInEx Pack 0.10.0+.

---

## Manifest schema

A data pack is a folder, shipped as a `.zip`, under `BepInEx/plugins/SuperDecor/Packs/<packId>/`:

```
com.yourname.yourpack/
  manifest.json
  models/modern_lamp.glb        runtime glTF: mesh, PBR material, and textures in one file
  icons/modern_lamp.png         optional; the generator can render this
  sounds/place.ogg              optional
```

The loader reads both extracted `<packId>/` folders and `<packId>.zip` archives.

### Full example

```jsonc
{
  "formatVersion": 1,
  "packId": "com.yourname.yourpack",      // STABLE, feeds the permanent ID, never rename
  "packName": "Your Pack",
  "version": "1.0.0",
  "author": "Your Name",
  "dependencies": [],                      // other packIds this pack needs, optional

  "items": [
    {
      "internalName": "modern_lamp",       // STABLE, feeds the permanent ID, never rename
      "displayName": "Modern Lamp",
      "description": "A sleek LED floor lamp.",
      "category": "Lighting",              // groups the item in the SuperDecor Shop
      "price": 150.0,
      "baseReferenceId": 2,                // base furniture to clone, also sets the box size

      "model": "models/modern_lamp.glb",   // a .glb path (data pack)
      "icon": "icons/modern_lamp.png",     // optional; rendered for you if omitted
      "textures": [],                      // loose textures only; a .glb embeds its own

      "transform": { "scale": [1,1,1], "offset": [0,0,0], "rotation": [0,0,0] },
      "placement": { "tags": ["Floor","Ground"], "isWalkable": false, "allowClipping": false },
      "collision": { "mode": "convexMesh" },
      "scaling": { "allowed": true },
      "interaction": { "isInteractable": true, "text": "Move" },
      "lighting": { "castShadows": true, "receiveShadows": true },
      "sounds": { "placement": "", "interaction": "" },
      "materialOverrides": [],

      "animation": { "autoplay": false, "clip": "", "loop": true, "speed": 1.0 }
    }
  ]
}
```

### Conventions

- Vectors are `[x, y, z]` arrays.
- Colors are `"#RRGGBBAA"` hex strings.
- Paths (`model`, `icon`, `sounds.*`) are relative to the pack folder, with forward slashes.
- Unknown fields are ignored. Omitted optional fields take their default.

### Pack fields

| Field | Type | Required | Notes |
|---|---|---|---|
| `formatVersion` | int | yes | must be `1` |
| `packId` | string | yes | reverse-domain; **permanent**, feeds the ID |
| `packName` | string | yes | display name |
| `version` | string | yes | bump per release |
| `author` | string | no | your name |
| `dependencies` | string[] | no | other packIds |
| `items` | object[] | yes | one entry per decoration |

### Item fields

| Field | Type | Default | What it does |
|---|---|---|---|
| `internalName` | string | (required) | **permanent**, feeds the ID; `lowercase_with_underscores`, unique in the pack |
| `displayName` | string | `internalName` | name shown in the SuperDecor Shop |
| `description` | string | empty | shown in the shop detail panel |
| `category` | string | `"Fixtures"` | groups items in the SuperDecor Shop; expansion decor is hidden from the vanilla Furnitures page by default (config `HideExpansionFromVanillaPage`) |
| `price` | number | `100` | purchase price; sell price is automatically 50% |
| `baseReferenceId` | int | `100` | base furniture to clone, which also sets the box size |
| `model` | string | empty | the item's `.glb` path |
| `icon` | string | empty | store icon; rendered automatically by the generator if omitted |
| `textures` | string[] | `[]` | loose textures; a `.glb` already embeds its own |
| `transform.scale` / `.offset` / `.rotation` | vec3 | `[1,1,1]` / `[0,0,0]` / `[0,0,0]` | applied on top of the model; scale is baked into the collider so collision matches the visual |
| `placement.tags` | string[] | `["Floor","Ground"]` | the surfaces the item can be placed on |
| `placement.isWalkable` | bool | `false` | `true` makes the collider a trigger: the player walks through, but the placement raycast still hits it so items can stack on top |
| `placement.allowClipping` | bool | `false` | `true` lets a placement commit while overlapping. Default `false` rejects an overlapping commit (there is no green/red placement tint, so strict is the safe default) |
| `collision.mode` | string | `"convexMesh"` | `"convexMesh"` (solid, the default), `"box"` (an axis-aligned box sized to the mesh, best for thin or decal items), or `"meshExact"` (exact concave shape) |
| `scaling.allowed` | bool | `true` | whether the item can be resized in Precision Mode |
| `interaction.isInteractable` | bool | `true` | parsed; the built-in move/rotate/height interaction applies to all decor today |
| `interaction.text` | string | `"Move"` | parsed; see note above |
| `lighting.castShadows` / `.receiveShadows` | bool | `true` / `true` | shadow flags |
| `sounds.placement` / `.interaction` | string | empty | file paths; parsed now, in-game playback is planned |
| `materialOverrides` | object[] | `[]` | optional tints or variants; see [MaterialOverride](#materialoverride) |
| `animation` | object | off | runtime glTF animation; see [Animation](#animation) |

> **Collision is automatic.** The framework builds a collider from your model's geometry, with the
> visual scale baked in, so players cannot walk through your item. You only set `collision.mode` if you
> want a different shape. Beginners can ignore the collision, scaling, and clipping fields entirely.

### Animation

A no-code data pack animates by shipping an animated `.glb` and turning on `animation.autoplay`. The
framework plays the clip at runtime with its own keyframe sampler. No Unity Animator and no AssetBundle
are involved.

| Field | Type | Default | What it does |
|---|---|---|---|
| `animation.autoplay` | bool | `false` | play the clip when the item spawns; required to start playback |
| `animation.clip` | string | empty | which clip to play by name (case-insensitive); empty means the first clip in the file |
| `animation.loop` | bool | `true` | loop the clip |
| `animation.speed` | number | `1.0` | playback-rate multiplier |

Supported glTF animation: node motion (translation, rotation, scale), morph or blend-shape weights, and
full skinned or skeletal rigs. The framework only takes the animated path when the `.glb` actually
contains a skin, an animation, or morph targets; a plain static model is unchanged.

### MaterialOverride

Optional. A `.glb` already carries its PBR materials, so use overrides only for tints or variants.

| Field | Type | Default |
|---|---|---|
| `materialIndex` | int | `0` |
| `shaderName` | string | `"Standard"` |
| `baseColor` | `"#RRGGBBAA"` | `#FFFFFFFF` |
| `baseTexture` | string | empty |
| `metallic` | number (0 to 1) | `0` |
| `smoothness` | number (0 to 1) | `0.5` |
| `normalMap` | string | empty |
| `normalStrength` | number | `1` |

---

## Deterministic IDs

Every item gets a permanent in-game ID computed only from `packId` and `internalName`:

```csharp
const int EXPANSION_ID_START = 760000;
const int EXPANSION_ID_END   = 999999;

string combined  = $"{packId}:{internalName}";
byte[] hashBytes = SHA256.HashData(Encoding.UTF8.GetBytes(combined));
uint   hashValue = BitConverter.ToUInt32(hashBytes, 0);    // first 4 bytes, little-endian
int    range     = EXPANSION_ID_END - EXPANSION_ID_START;  // 239999
int    id        = EXPANSION_ID_START + (int)(hashValue % range);  // 760000 to 999998
```

- The ID depends only on `packId` and `internalName`, never on the framework, so it is the same whether a
  pack ships as data or code, and it survives reinstalls and game updates.
- If two items happen to hash to the same ID, registration retries with `internalName + "_1"` through
  `"_99"`.

**Why the two names are frozen for life.** The computed ID is written into a player's save when they place
your item. Rename `packId` or any `internalName` and the recomputed ID no longer matches what is saved, so
the placed item disappears from saved stores. Everything else (display name, description, price, model
file, transform, placement settings) is safe to change between versions. Pick the two ID names once and
keep them.

---

## The C# API

Namespace `SuperDecor.Api`. All members of `DecorExpansionAPI` are static. Method shapes are a
source-compatibility contract: new overloads may be added, but existing methods are not removed or renamed
without a major version bump. For a full walkthrough, see [Code packs](CODE_PACKS.md).

### Constants

```csharp
DecorExpansionAPI.API_VERSION    // "2.2.0"
DecorExpansionAPI.BASE_MOD_GUID  // "com.leptoon.superdecor"  (your [BepInDependency] target)
```

### Readiness and registration

```csharp
// True once the framework is loaded and ready.
bool IsBaseModReady()

// Register a pack. Call this BEFORE registering any of its items.
bool RegisterExpansionPack(string packId, string packName, string packVersion,
                           string author = "", string[] dependencies = null)

// Register a pack and record its assembly (so embedded resources resolve).
bool RegisterExpansionPackWithAssembly(string packId, string packName, string packVersion,
                                       string author, System.Reflection.Assembly assembly,
                                       string[] dependencies = null)

// Register one item. Returns the assigned furniture ID, or -1 on failure.
int RegisterDecorItem(string packId, DecorItemData itemData)

// True if id is in the expansion-item range.
bool IsExpansionItem(int id)

// Flush queued items into the game once it is ready (called automatically).
void TryProcessPendingItems()
```

Registration order matters: `RegisterDecorItem` returns `-1` if its pack is not registered yet, so call
`RegisterExpansionPack` (or the `...WithAssembly` overload) first.

### Queries

```csharp
List<ExpansionPackInfo> GetRegisteredPacks()
List<DecorItemData>     GetAllDecorItems()
List<DecorItemData>     GetDecorItemsByCategory(string category)
DecorItemData           GetDecorItemById(int furnitureId)        // null if unknown
bool                    IsPackRegistered(string packId)
ExpansionPackInfo       GetPackInfo(string packId)               // null if unknown
DecorItemData           GetDecorItem(string packId, string internalName)
List<DecorItemData>     GetPackItems(string packId)
System.Reflection.Assembly GetPackAssembly(string packId)
```

### Management and logging

```csharp
bool UnregisterExpansionPack(string packId)   // removes the pack and all its items
void LogInfo(string message)
void LogWarning(string message)
void LogError(string message)
```

### Spawn hook (live behavior)

Attach your own behavior (animation, particles, physics, interaction) to a placed decor instance.

```csharp
// Raised once per live decor instance, after its visual is applied, on every spawn route
// (initial placement, save-reload, precision-edit swap). Filter by furnitureId. Runs fail-soft on the
// main thread; a handler that throws is caught and logged, never breaking the spawn.
event Action<UnityEngine.GameObject, int> ItemSpawned;

// Registers T once and attaches a fresh T to every spawned instance of furnitureId. You never touch
// Harmony or the IL2CPP class injector. T needs the constructor `public T(IntPtr ptr) : base(ptr) {}`.
void RegisterItemBehavior<T>(int furnitureId) where T : UnityEngine.MonoBehaviour
```

### Disabled mode

If the framework detects an incompatible game (or its core is missing), it runs in a safe disabled mode
instead of breaking the game: the whole API stays loadable, but `IsBaseModReady()` returns `false`,
registration returns `false` or `-1`, `RegisterItemBehavior<T>` is a no-op, the spawn hook never fires,
and queries return empty. Nothing throws. A compliant pack only needs to guard on `IsBaseModReady()` and
return early. Two accessors report the reason:

```csharp
LoaderStatus GetLoaderStatus();   // Healthy | Disabled | Degraded
string GetDisabledReason();       // a short reason when Disabled or Degraded, else null
```

---

## Data structures

### DecorItemData

The data you pass to `RegisterDecorItem`. Defaults reproduce sensible behavior when unset.

```csharp
public class DecorItemData
{
    // Identity
    public string InternalName { get; set; }        // STABLE, feeds the ID (required)
    public string ItemName { get; set; }            // display name
    public string Description { get; set; }
    public string Category { get; set; }            // = "Fixtures"; groups items in the shop
    public int    FurnitureID { get; internal set; } // framework-assigned; you never set it

    // Economic
    public float  Price { get; set; }               // = 100f

    // Base furniture selector (also sets the box size)
    public int    BaseReferenceID { get; set; }     // = 100

    // Assets (string-based)
    public string   MeshAssetName { get; set; }     // .glb path or name
    public string   AssetBundlePath { get; set; }   // optional AssetBundle reference
    public string[] TextureAssetNames { get; set; }

    // Visual transform
    public Vector3 VisualScale { get; set; }        // = Vector3.one
    public Vector3 VisualOffset { get; set; }       // = Vector3.zero
    public Vector3 VisualRotation { get; set; }     // = Vector3.zero (Euler)

    // Placement and collision
    public string[] PlacementTags { get; set; }     // = { "Floor", "Ground" }
    public bool     IsWalkable    { get; set; }     // = false
    public bool     AllowClipping { get; set; }     // = false
    public bool     AllowScaling  { get; set; }     // = true (resize in Precision Mode)
    public DecorCollisionMode CollisionMode { get; set; }  // = ConvexMesh | Box | MeshExact

    // Interaction, audio, lighting
    public bool   IsInteractable { get; set; }      // = true
    public string InteractionText { get; set; }     // = "Move"
    public string PlacementSound { get; set; }      // file path (parsed; playback planned)
    public string InteractionSound { get; set; }    // file path (parsed; playback planned)
    public bool   CastShadows { get; set; }         // = true
    public bool   ReceiveShadows { get; set; }      // = true

    // Materials and animation
    public MaterialOverride[] MaterialOverrides { get; set; }
    public MeshAnimationData  Animation { get; set; }   // off by default; played at runtime

    public void Validate();         // fills safe defaults
    public DecorItemData Clone();   // deep copy
}
```

### MeshAnimationData

```csharp
public class MeshAnimationData
{
    public bool   Autoplay { get; set; }   // = false (required to start playback)
    public string Clip { get; set; }       // = "" (clip name; empty means the first clip)
    public bool   Loop { get; set; }       // = true
    public float  Speed { get; set; }      // = 1f
}
```

### ExpansionPackInfo

```csharp
public class ExpansionPackInfo
{
    public string PackId { get; set; }
    public string PackName { get; set; }
    public string Version { get; set; }
    public string Author { get; set; }
    public string[] Dependencies { get; set; }
    public List<int> RegisteredItems { get; set; }
    public bool IsEnabled { get; set; }                        // = true
    public Dictionary<string, object> CustomData { get; set; }
}
```

### DecorCategories

Standard category strings. They group items in the SuperDecor Shop; expansion decor is hidden from the
vanilla Furnitures page by default.

```
Fixtures, WallDecor, FloorDecor, Lighting, Outdoor, Seasonal, Furniture, Decorations
```

### Base reference IDs

`BaseReferenceID` selects the game furniture your item clones for placement behavior, which also sets the
box size. Box size is not set separately. Common references:

- `2`: small furniture base
- `3`: medium furniture base
- `4`: large furniture base

---

## Notes

- **Automatic pricing.** Sell price is always 50% of the purchase price. Delivery behavior comes from the
  chosen base reference.
- **Placement is tag-based** through `placement.tags`.
- **A missing model is not fatal.** If a model does not resolve, the item shows with a box-collider
  placeholder and is still purchasable and placeable.
