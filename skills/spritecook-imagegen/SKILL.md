---
name: spritecook-imagegen
description: Build game-ready sprite sheets and animation frames in Codex using ImageGen2-style prompting, with a path for future model upgrades.
---

# SpriteCook for Codex (ImageGen2-first)

Use this skill when a user wants sprite art generation similar to SpriteBook.ai workflows, but implemented directly in Codex with OpenAI image generation models.

## Goal

Generate cohesive sprites (single frame, directional set, animation strip, or full sprite sheet) with:
- style consistency
- transparent background
- grid-safe framing
- predictable naming/export layout

## Model policy

1. **Default**: use the best available ImageGen model in Codex (currently referred to as *ImageGen2* in user language).
2. **Forward compatibility**: if a newer generation model is available and stable, prefer it without changing the workflow.
3. If model selection is ambiguous, explicitly state: “Using Codex’s latest supported image generation model for sprite work.”

## Intake checklist

Before generating, collect or infer:
- Subject (character/object)
- Perspective (top-down, side, isometric, front)
- Pixel vibe (clean pixel, painterly pixel, retro console, modern HD-2D)
- Palette constraints (optional fixed palette)
- Frame size (e.g., 32x32, 48x48, 64x64, 96x96)
- Required states/animations (idle, walk, attack, hurt, etc.)
- Output format (individual PNGs, strip, sheet)

If missing, ask 1 concise clarification round or proceed with safe defaults and list assumptions.

## Prompt recipe (core)

For each generation, include:
- clear subject and silhouette
- strict framing and no cropping
- transparent background
- sprite readability at target size
- consistency anchor (same character design tokens across frames)

Template:

> Create a [STYLE] game sprite of [SUBJECT], [VIEW], designed for [FRAME_SIZE] pixels per frame. Keep full body visible, centered, and uncropped. Transparent background only. Strong silhouette, readable at small scale, limited palette ([PALETTE or “cohesive retro palette”]). Keep character design consistent with: [ANCHOR TOKENS]. Output: [STATE/ACTION], frame [N] of [TOTAL].

## Multi-frame workflow

1. Produce a **style anchor** frame first (idle/front or neutral pose).
2. Reuse anchor tokens for every subsequent frame.
3. Generate one animation at a time (idle → walk → action).
4. Run a quick continuity check:
   - head/body proportions stable
   - costume colors stable
   - camera/view stable
5. If drift occurs, regenerate only drifted frames with stricter anchor wording.

## Assembly workflow in repo

When users want packaged assets:
1. Save frames under:
   - `assets/sprites/<character>/<animation>/<character>_<animation>_<index>.png`
2. Optionally produce a manifest:
   - `assets/sprites/<character>/sprite_manifest.json`
3. If requested, assemble strips/sheets with deterministic ordering.

## Example starter prompts

### 1) Single idle frame
Create a crisp pixel-art forest ranger sprite, side view, designed for 48x48 pixels per frame. Full body visible and centered, uncropped, transparent background only. Readable silhouette, earthy green/brown palette, subtle cloak and satchel, consistent face mark. Output: idle pose.

### 2) 4-frame walk cycle
Create frame 1 of 4 for a pixel-art desert mech scout sprite, 64x64 per frame, side view, transparent background, full body centered and uncropped. Preserve anchor design: narrow torso, one large shoulder pad, antenna on left, cyan visor slit, sand-yellow armor with rust accents. Output action: walk cycle.

## Quality bar

Accept only if:
- background is truly transparent
- no frame is cropped
- all frames feel like the same character
- motion progression is readable in sequence
- exported files are named consistently

## Failure handling

If results are blurry/inconsistent:
- tighten prompt with explicit “pixel-art, hard edges, no painterly blur”
- restate frame size and centering constraints
- reduce simultaneous requirements (generate fewer frames per pass)
- regenerate outliers only

## Notes for future ImageGen versions

This skill is model-agnostic by design. When ImageGen3+ becomes default in Codex, keep the same structure and only update prompt details if the model benefits from different control phrasing.
