---
name: rebuild-editable-pptx
description: Rebuild slide screenshots or slide images as high-fidelity, highly editable 16:9 PowerPoint files. Use when the user asks to recreate, restore, reverse-engineer, beautify, or convert a PPT screenshot/image into an editable .pptx with native text boxes, shapes, tables, separately movable photos or decorations, consistent replacement icons, and no full-slide background screenshot.
---

# Rebuild Editable PPTX

Recreate each reference slide as independent PowerPoint objects while preserving its visual hierarchy, layout, palette, spacing, and brand character.

## Required foundation

Use the available Presentations skill and follow its local PPTX workflow. Read its `SKILL.md`, required style guidance, and Artifact Tool API documentation before implementation. Use `@oai/artifact-tool` from a JavaScript ES module; do not use `python-pptx`.

## Workflow

1. Inspect every reference image at original resolution.
2. Inventory the slide into background, branding, headings, body text, panels, shapes, icons, photos, tables, diagrams, and decorations.
3. Choose a 16:9 canvas, normally `1600 × 900` for screenshot reconstruction.
4. Rebuild structural elements with native PowerPoint shapes.
5. Rebuild all visible text as native text boxes. Preserve wording, emphasis, alignment, and intended line breaks.
6. Insert photographs, portraits, logos, and complex illustrations as separate image objects. Crop only the required region; never embed the full screenshot as the slide background.
7. Replace icons with a single coherent open-source icon family when suitable assets are not supplied.
8. Export the PPTX, render every slide, inspect it at full size, run overflow tests, fix defects, and repeat until clean.
9. Deliver only the final `.pptx` unless the user asks for previews or build files.

## Reconstruction rules

- Keep title, subtitle, metrics, cards, labels, and footers independently editable.
- Prefer one text box per semantic text block. Use separate text boxes for differently colored or emphasized phrases.
- For maximum table editability, build the grid with native cell rectangles plus independent text boxes. A native PowerPoint table is acceptable when cell-level repositioning is not required.
- Preserve object layering: background accents first, containers next, images and icons next, text last.
- Use native lines, rectangles, rounded rectangles, circles, and simple arrows for structural graphics.
- Use separate image objects for complex icons, portraits, logos, textures, and illustrations.
- Do not use emoji or font-dependent symbol glyphs when a stable image icon is available.
- Match the reference's compact typography when necessary; reference-slide fidelity overrides generic minimum font defaults.
- Avoid unexpected title wrapping. Expand the text box or reduce the title font slightly before allowing a one-line title to wrap.
- Do not invent facts, names, credentials, metrics, or claims. If text is unreadable, use the most defensible transcription and avoid unsupported additions.

## Icon sourcing and preparation

- Prefer one icon family for the entire slide. Start with official [Bootstrap Icons](https://github.com/twbs/icons) or Google Material Symbols.
- Search the web when the user requests icon matching or when the supplied screenshot lacks reusable icon assets.
- Prefer official repositories and verify the license.
- Download SVG icons, recolor them consistently, set SVG width and height to at least 128 pixels, then rasterize to transparent PNG before insertion. Avoid scaling a 16-pixel PNG because it will blur.
- Place white icons on blue/teal/purple circular or rounded-square backgrounds to match enterprise presentation styling.
- Record icon URLs and licenses in a local `source-notes.txt` and a `[Sources]` block in slide speaker notes.

## Photo and screenshot asset handling

- Crop portraits or illustrations from the user-provided screenshot into separate raster assets when no original image is available.
- Use `sips` or an equivalent image utility for simple cropping. Inspect the crop before placing it.
- Use `fit: "cover"` for portrait frames and `fit: "contain"` for icons.
- Preserve sufficient source resolution; replace or re-crop blurry assets before delivery.

## Visual QA gate

Do not deliver until all checks pass:

- Render every slide with the Presentations helper.
- Run `slides_test.py`; resolve every overflow warning.
- Inspect every rendered slide individually at full size.
- Fix clipped text, wrapped headings, misaligned columns, stretched images, weak contrast, inconsistent icon sizes, and accidental overlaps.
- Verify that the full-slide screenshot is absent from the PPTX.
- Confirm that all major text, panels, icons, photos, and table cells can be selected and moved independently.
- Keep source/asset provenance in speaker notes when external assets are used.

## Implementation reference

Read [references/implementation-recipes.md](references/implementation-recipes.md) when writing the build module or preparing external icon and photo assets.

