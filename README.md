# Rebuild Editable PPTX

[简体中文](README-zh.md) | English

A Codex skill for rebuilding slide screenshots or reference images as high-fidelity, highly editable 16:9 PowerPoint files.

Instead of placing the original screenshot on a slide, this skill reconstructs the design with independent PowerPoint objects: text boxes, shapes, panels, tables, icons, photos, and decorations. The result preserves the visual hierarchy of the reference while remaining practical to edit.

## Highlights

- Recreates slide screenshots as editable `.pptx` files
- Keeps headings, body text, metrics, cards, and footers as native text objects
- Builds panels, diagrams, and table grids with native PowerPoint shapes
- Separates photos, portraits, logos, icons, and illustrations into movable image objects
- Preserves layout, spacing, colors, typography, layering, and brand character
- Uses a consistent open-source icon family when source icons are unavailable
- Renders and visually inspects every slide before delivery
- Checks for text overflow, clipping, wrapping, misalignment, and accidental overlap
- Records the provenance of externally sourced assets in speaker notes

## Use cases

Use this skill when you need to:

- Recover an editable presentation from screenshots or exported slide images
- Recreate a slide whose original PowerPoint file is unavailable
- Convert a flattened presentation design into independently movable objects
- Modernize or refine a reference slide while retaining its visual identity
- Rebuild a table-heavy or card-based slide for continued editing

## Requirements

- [Codex](https://openai.com/codex/)
- The Codex **Presentations** skill and its local PowerPoint toolchain
- A user-provided slide screenshot or reference image

The workflow uses `@oai/artifact-tool` through a JavaScript ES module. It does not use `python-pptx`.

## Installation

Clone or copy this directory into your Codex skills folder:

```bash
git clone <repository-url> ~/.codex/skills/rebuild-editable-pptx
```

Restart Codex if the skill is not detected immediately.

## Usage

Attach one or more slide screenshots and ask Codex to use the skill. For example:

```text
Use $rebuild-editable-pptx to recreate these slide screenshots as a 16:9
editable PowerPoint. Keep all text, cards, icons, photos, and table cells
independently movable.
```

You can also specify priorities such as exact visual fidelity, a preferred output filename, replacement fonts, or whether unreadable text should be marked for review.

## How it works

1. Inspect each reference image at its original resolution.
2. Inventory the visible text, shapes, panels, images, icons, tables, and decorations.
3. Rebuild the slide on a 16:9 canvas with native PowerPoint objects.
4. Crop reusable photos or illustrations into separate assets when originals are unavailable.
5. Add source attribution for any external assets.
6. Export the presentation, render every slide, run overflow checks, and correct visual defects.
7. Deliver the final editable `.pptx` file.

## Editability standard

A reconstructed slide should not use the source screenshot as a full-slide background. Major visual elements must remain independently selectable and movable. Complex raster content may remain an image, but text, containers, structural shapes, and table cells should be native PowerPoint objects whenever practical.

## Project structure

```text
rebuild-editable-pptx/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── implementation-recipes.md
```

- `SKILL.md` defines the core reconstruction workflow and quality gates.
- `agents/openai.yaml` provides the skill's display metadata and default prompt.
- `references/implementation-recipes.md` contains implementation patterns for editable objects, image handling, icon preparation, source notes, and QA.

## Notes

- Output quality depends on the resolution and legibility of the reference image.
- Unreadable text should be transcribed conservatively; the skill does not invent facts, names, metrics, or claims.
- Exact font matching requires the relevant fonts to be available in the working environment.
- Logos, detailed illustrations, and photographs may remain raster assets, but they are inserted as separate objects rather than flattened into the slide background.

