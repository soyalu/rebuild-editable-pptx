# Implementation Recipes

## Contents

- Workspace and outputs
- Editable object patterns
- Image insertion
- Icon preparation
- Portrait cropping
- Source notes
- QA commands

## Workspace and outputs

Use a writable build directory and absolute paths:

```text
TMP_DIR=<conversation workspace>/ppt-build
FINAL_PPTX=<conversation workspace>/<slide-name>_可编辑.pptx
```

Initialize Artifact Tool using the Presentations skill helper before running the generated `.mjs` module.

## Editable object patterns

Use native shapes for panels and a separate textbox for text:

```js
const panel = slide.shapes.add({
  geometry: "roundRect",
  position: { left: 48, top: 260, width: 420, height: 300 },
  fill: "#FFFFFF",
  line: { style: "solid", fill: "#C9DBF4", width: 1 },
  borderRadius: 12,
});

const title = slide.shapes.add({
  geometry: "textbox",
  position: { left: 72, top: 276, width: 350, height: 34 },
  fill: "none",
  line: { style: "solid", fill: "none", width: 0 },
});
title.text = "可编辑标题";
title.text.style = {
  fontFamily: "Microsoft YaHei",
  fontSize: 26,
  bold: true,
  color: "#0C55C7",
};
```

For highly editable tables, create a rectangle and textbox per cell. Keep header fills, alternate-row fills, borders, and text alignment explicit.

## Image insertion

Read local bytes and insert them as independent objects:

```js
const bytes = await fs.readFile(iconPath);
slide.images.add({
  blob: bytes,
  contentType: "image/png",
  alt: "Team icon",
  fit: "contain",
  position: { left: 92, top: 144, width: 42, height: 42 },
});
```

For portraits, use `fit: "cover"`, `geometry: "roundRect"`, and an editable native shape behind the image for borders or shadows.

## Icon preparation

Use an official repository and download only the required SVGs. For Bootstrap Icons, raw files follow this pattern:

```text
https://raw.githubusercontent.com/twbs/icons/main/icons/<icon-name>.svg
```

Before rasterizing, replace `fill="currentColor"` with the intended color and change `width="16" height="16"` to at least `128 × 128`. Then run the Presentations skill's `ensure_raster_image.py` helper. Keep each icon as a separate PNG object.

Useful Bootstrap icon names include:

- `people`, `person-badge`, `graph-up-arrow`, `award`
- `robot`, `eye`, `boxes`, `headset-vr`
- `code-slash`, `shield-check`, `clipboard2-check`
- `mortarboard`, `folder-check`, `trophy`

## Portrait cropping

For a user-provided screenshot, inspect its pixel dimensions and crop only the portrait:

```bash
sips -c <height> <width> --cropOffset <top> <left> reference.png --out portrait.png
```

Inspect the result with the local image viewer before using it.

## Source notes

Write asset provenance into speaker notes:

```js
slide.speakerNotes.textFrame.setText([
  "[Sources]",
  "- User-provided reference image: /absolute/path/reference.png",
  "- Bootstrap Icons (MIT License): https://github.com/twbs/icons",
]);
slide.speakerNotes.setVisible(false);
```

Also keep a plain-text `source-notes.txt` in the temporary build directory.

## QA commands

Use the Presentations skill helpers to render and validate:

```bash
python3 "$SKILL_DIR/container_tools/render_slides.py" "$FINAL_PPTX"
python3 "$SKILL_DIR/container_tools/slides_test.py" "$FINAL_PPTX"
```

After rendering, inspect every slide image at full size. Iterate whenever text is clipped, a title wraps unexpectedly, icons differ in scale or stroke, or footer content is too close to the slide edge.
