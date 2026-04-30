# Editable PPT Reference

Use this reference when turning an approved pure-image visual proof into an editable PowerPoint deck.

## Two-Stage Rule

Do not try to make the first generated image both visually rich and text-perfect. Use two stages:

1. Visual proof: generate a full-slide image for each page and verify composition, palette, hierarchy, and story flow.
2. Editable deck: reuse the approved images as visual backgrounds and add exact text, numbers, citations, and key labels as editable PowerPoint objects.

This creates a practical editable deck. The AI-generated background is still a raster image unless a separate full shape-rebuild workflow is requested.

## Editable Elements

Use `editable_elements` for any text that must be exact:

```json
{
  "type": "text",
  "text": "Three bets drive 2026 growth",
  "x": 0.7,
  "y": 0.5,
  "w": 8.8,
  "h": 0.55,
  "font_size": 28,
  "bold": true,
  "color": "#111827",
  "align": "left",
  "panel": false
}
```

Supported element types:

- `text`: a single editable text box.
- `bullet_list`: editable bullet list from `items`.

Coordinates use inches on the PowerPoint slide canvas. For 16:9 decks, the default canvas is 13.333 x 7.5 inches.

## Prompting for Editable Overlays

When using `hybrid_editable_text`, tell the image model:

- reserve clean areas for titles and labels
- use abstract placeholder blocks instead of final small text
- avoid rendering exact numbers, citations, legal text, or dense tables
- make backgrounds readable behind later text overlays

## When to Rebuild Fully Editable Slides

Use a separate shape-rebuild workflow only when the user needs all objects to be editable, including diagrams, charts, icons, and backgrounds. That is slower and should be planned as a dedicated conversion step.
