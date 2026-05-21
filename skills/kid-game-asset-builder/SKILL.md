---
name: kid-game-asset-builder
description: Build cohesive, kid-friendly 2D game asset packs in Codex with ImageGen, including characters, sprites, backgrounds, tilesets, props, effects, and UI icons.
---

# Kid Game Asset Builder

Use this skill when a user wants fun, game-ready visual assets for making small games with kids. Prefer ImageGen for bitmap art generation, currently ImageGen2-style workflows, while keeping the process model-agnostic for future Codex image models.

## Goal

Create cohesive asset packs for small 2D games:
- playable characters and animation frames
- enemies, sidekicks, bosses, and NPCs
- backgrounds and parallax layers
- platformer or top-down tilesets
- props, collectibles, hazards, projectiles, and effects
- UI icons, buttons, badges, hearts, counters, and simple title art
- predictable file layout and manifest metadata

## Default assumptions

When details are missing, optimize for fast kid-game momentum:
- audience: playful, readable, non-scary
- style: colorful clean pixel art or soft cartoon sprites
- character frame size: 64x64
- small objects: 32x32 or 48x48
- walk cycles: 4 frames
- idle cycles: 2 frames
- background: 16:9 static plus optional parallax layers
- tiles: 32x32 or 48x48, with clear collision edges
- scope: one cohesive theme before expanding variants

If the project needs exact engine constraints, ask one concise clarification round. Otherwise proceed with these defaults and list assumptions.

## Kid collaboration intake

Ask kid-friendly questions when the user is brainstorming with children:
- Who is the hero?
- What silly thing do they collect?
- Where does the game happen?
- What is the funniest enemy?
- What happens when the player wins?

Convert the answers into an art direction block:
- theme
- mood
- palette
- game type
- camera perspective
- world rules
- asset list

## Pack modes

Choose the smallest pack that will make the game playable.

### Tiny playable pack

Use for a first prototype:
- 1 hero: idle, walk, jump or action
- 1 enemy: idle or walk
- 3 collectibles or props
- 1 hazard
- 1 background
- 1 small tileset
- 3 UI icons

### Weekend game pack

Use when the user wants a fuller kit:
- 1 hero and 1 sidekick
- 3 enemies
- 8 props or collectibles
- 4 hazards
- 1 tileset with variants
- 2 or 3 background/parallax layers
- basic UI set
- simple win/lose/title art

### Expansion pack

Use after a base style exists:
- new biome
- new character skins
- more enemies
- additional tiles
- visual effects
- boss or vehicle assets

## Workflow

1. Establish an art direction anchor: theme, palette, camera, line weight, shading, and target frame sizes.
2. Generate a hero neutral pose first as the visual anchor.
3. Reuse anchor tokens for every related character frame.
4. Generate one asset family at a time: characters, then tiles, then backgrounds, then props/UI/effects.
5. Keep gameplay readability first: silhouettes, collision edges, and important interactables must be obvious.
6. Save assets into the standard layout and write or update the manifest.
7. Run a quality pass for transparency, cropping, scale, style consistency, and naming.

## Prompt recipes

### Character sprite

Create a [STYLE] game sprite of [CHARACTER], [VIEW], designed for [FRAME_SIZE] pixels per frame. Full body visible, centered, uncropped. Transparent background only. Strong silhouette, readable at small scale. Keep design consistent with: [ANCHOR TOKENS]. Mood: playful and kid-friendly. Output: [STATE/ACTION], frame [N] of [TOTAL].

### Background

Create a [STYLE] 2D game background for a [GAME_TYPE] set in [WORLD], designed for [RESOLUTION]. Keep the gameplay area readable and uncluttered. No characters, no text. Use cohesive colors that match: [ART DIRECTION]. Output as [STATIC BACKGROUND / FAR PARALLAX LAYER / MID PARALLAX LAYER / FOREGROUND ACCENT LAYER].

### Tileset

Create a cohesive [STYLE] tileset for [GAME_TYPE], [TILE_SIZE] pixels per tile, set in [WORLD]. Include ground center, left edge, right edge, inner corner, outer corner, floating platform, wall, background filler, decorative variant, and hazard tile. Clear readable collision edges. No text. Output on a clean grid.

### Props and collectibles

Create [COUNT] [STYLE] game props for [WORLD], each readable at [SIZE] pixels, transparent background, centered, uncropped, consistent palette. Include: [OBJECT LIST]. Make each object distinct in silhouette and easy for a child to recognize.

### UI icons

Create a [STYLE] kid-friendly game UI icon set with transparent background, readable at [SIZE] pixels. Include: hearts, star counter, coin counter, pause, restart, locked, unlocked, trophy, and settings. Match the asset pack palette: [PALETTE].

### Effects

Create [STYLE] 2D game effect frames for [EFFECT], transparent background, centered, no text. Designed for [FRAME_SIZE] pixels per frame. Motion should read clearly across [TOTAL] frames.

## Output layout

When saving packaged assets, use:

```text
assets/game-assets/<project>/
  manifest.json
  art-direction.md
  characters/<character>/<animation>/<character>_<animation>_<index>.png
  enemies/<enemy>/<animation>/<enemy>_<animation>_<index>.png
  backgrounds/<background_name>.png
  backgrounds/parallax/<layer_name>.png
  tilesets/<tileset_name>/<tile_name>.png
  props/<prop_name>.png
  collectibles/<collectible_name>.png
  hazards/<hazard_name>.png
  effects/<effect>/<effect>_<index>.png
  ui/<icon_name>.png
```

## Manifest shape

Use JSON so game code can consume the pack:

```json
{
  "project": "project-name",
  "style": "clean pixel art",
  "palette": ["#000000"],
  "frameSize": {
    "characters": 64,
    "tiles": 32,
    "ui": 32
  },
  "characters": {},
  "enemies": {},
  "backgrounds": {},
  "tilesets": {},
  "props": {},
  "collectibles": {},
  "hazards": {},
  "effects": {},
  "ui": {},
  "recommendedCollision": {}
}
```

## Quality bar

Accept only if:
- transparent-background assets are actually transparent
- frame contents are centered and uncropped
- animation frames look like the same character or object
- tiles align to a clean grid and have readable collision edges
- backgrounds do not hide gameplay objects
- assets share one palette and rendering style
- filenames are lowercase, stable, and deterministic

## Failure handling

If results drift or blur:
- regenerate fewer assets per pass
- tighten anchor tokens and palette language
- specify "hard edges, readable at small scale" for pixel work
- regenerate only outliers
- separate background, tiles, and character prompts instead of combining them

If generated tiles are not usable:
- ask for fewer tile types per image
- generate each critical tile separately
- assemble the sheet after individual tiles pass quality checks

## Future model policy

Use Codex's latest supported image generation model for asset work unless the user asks for a specific model. Keep prompts explicit about framing, transparency, grid alignment, and gameplay readability so the workflow continues to work with ImageGen3+ or later models.
