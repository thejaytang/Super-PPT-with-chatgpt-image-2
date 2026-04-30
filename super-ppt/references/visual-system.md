# Visual System Reference

Use this reference when planning and prompting a Super PPT deck.

## Visual System Contract

Define the visual system once, then repeat it in every slide prompt. Do not let each slide invent its own palette, typography, icon style, or composition.

Minimum contract:

- `style_name`: short label for the deck's look.
- `palette`: 5-7 colors with specific hex values.
- `typography`: font style hints, hierarchy, and text density rules.
- `ui_motif`: repeated UI language such as dashboard panels, cards, chips, timelines, maps, or editorial spreads.
- `composition`: grid, margins, title zone, content zones, and chart placement.
- `image_style`: flat vector, 3D clay, editorial photo-composite, data-dashboard, technical diagram, or another consistent mode.
- `negative_rules`: visual elements that must not appear.

Keep these fields short enough to repeat in every prompt. Long visual systems are harder for the image model to follow consistently.

## Palette Guidance

Use one neutral background, one surface color, one primary color, one accent color, one text color, and one muted color. Add a warning or success color only when the content needs it.

Avoid one-note palettes. A polished business deck usually needs contrast between neutral surfaces, a primary action color, and a secondary accent.

Example:

```json
{
  "background": "#F7F8FA",
  "surface": "#FFFFFF",
  "primary": "#1E4FD7",
  "accent": "#18A999",
  "text": "#111827",
  "muted": "#667085",
  "warning": "#F59E0B"
}
```

## Slide Type Patterns

Cover:

- Use one strong visual metaphor.
- Keep text to title, subtitle, date, and owner if needed.
- Avoid busy collage backgrounds.

Executive Summary:

- Use 3-5 key message blocks.
- Make hierarchy clear through size and spacing, not through many colors.

Framework:

- Use a matrix, flywheel, stack, pyramid, or journey map.
- Keep labels short and visually balanced.

Data or Metrics:

- Prefer simple charts with large labels.
- Avoid dense tables in generated images.
- If numeric accuracy matters, use editable PowerPoint chart objects or manual overlays.

Process:

- Use a left-to-right timeline, swimlane, loop, or stepper.
- Keep steps visually uniform.

Closing:

- Return to the core message.
- Use a lighter composition with one clear next action.

## Section-Level Variation

Use one base visual system across the deck. If the deck has sections, vary only one or two controlled tokens:

- section accent color
- background density
- icon category
- chart type

Do not change typography, grid, UI motif, or image style between sections unless the user explicitly asks for a different chapter look.

## Text Strategy

Use generated text only for short, high-level labels. Use editable PowerPoint text for:

- exact numbers
- legal or compliance statements
- citations and source names
- dense tables
- product names that must be spelled exactly
- multilingual text where spelling errors are unacceptable

For `hybrid_editable_text`, ask GPT Image to create visual structure, placeholder panels, diagrams, and background rhythm. Do not ask it to render the final exact title and body copy.

## Prompt Template

Use this structure for each image prompt:

```text
Create a 16:9 full-slide PowerPoint page for a polished business presentation.
Deck: {deck_title}. Slide {number}: {title}. Role: {role}.
Takeaway: {takeaway}.
Visual intent: {visual_intent}.
Content to render: {content}.
Visual system: {style_name}; palette {palette}; typography {typography}; UI motif {ui_motif}; composition {composition}; image style {image_style}.
Make the slide feel consistent with the rest of the deck. Use clean spacing, clear hierarchy, and restrained professional detail.
Negative rules: no unrelated colors, no random logos, no decorative gradient blobs, no photorealistic clutter unless requested, no tiny unreadable paragraphs, no inconsistent icon styles.
```

## Review Checklist

Review every generated page for:

- consistent palette and background treatment
- stable title position and hierarchy
- consistent UI components and icon style
- readable text with no obvious spelling errors
- one dominant visual idea per slide
- no accidental logos, watermarks, or unrelated symbols
- no decorative elements that compete with the message
