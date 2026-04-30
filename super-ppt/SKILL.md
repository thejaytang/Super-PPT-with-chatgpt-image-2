---
name: super-ppt
description: Create visually consistent PowerPoint decks from source content by planning the deck structure, defining a unified visual system, generating a pure-image visual proof deck first with GPT Image models such as gpt-image-2, then producing an editable PowerPoint version with critical text and data as editable PPT layers. Use when Codex needs to turn notes, outlines, reports, product briefs, strategies, lessons, or other source material into a polished Super PPT with coherent color, UI, layout, and slide-by-slide visual storytelling.
---

# Super PPT

## Workflow

Use this skill to build a presentation as a planned visual system, not as unrelated generated images. Always separate visual proof from editable delivery.

1. Gather source content, audience, goal, language, slide count, aspect ratio, and any brand constraints. Ask only for missing inputs that materially change the deck.
2. Read `references/deck-planning.md` when the source content is broad, ambiguous, or strategic.
3. Create a deck plan before generating images. Include narrative arc, slide roles, slide titles, takeaways, content density, and why each page exists.
4. Choose a generation mode:
   - `hybrid_editable_text`: default. Generate the visual proof as full-slide images, then add exact text, numbers, citations, or charts as editable PowerPoint objects.
   - `full_slide_image`: generate final slides as one full-bleed image. Use only when editability is not required.
5. Define one visual system for the whole deck: palette, typography hints, UI motif, layout grid, illustration style, data-display style, and forbidden variations.
6. Write a `plan.json` file using the schema expected by `scripts/super_ppt_pipeline.py`.
7. Run a dry run first to validate the plan and inspect prompts:

```bash
python3 /path/to/super-ppt/scripts/super_ppt_pipeline.py plan.json --dry-run --prompt-out prompts.md
```

8. Generate the pure-image visual proof deck first:

```bash
python3 /path/to/super-ppt/scripts/super_ppt_pipeline.py plan.json --output super-ppt-visual.pptx --image-dir images --model gpt-image-2 --skip-editable
```

9. Review `super-ppt-visual.pptx` and the generated images. Fix weak prompts or visual-system tokens, then regenerate only weak slides when practical.
10. After the visual proof is accepted, create the editable version from the approved images:

```bash
python3 /path/to/super-ppt/scripts/super_ppt_pipeline.py plan.json --output super-ppt-visual.pptx --editable-output super-ppt-editable.pptx --image-dir images --model gpt-image-2 --reuse-images
```

If `gpt-image-2` is unavailable in the active OpenAI account or SDK, stop and ask whether to use an explicitly named replacement. Do not silently downgrade the model.

## Planning Rules

Build the deck around one main message. Prefer fewer slides with clearer narrative beats over many shallow pages.

For each slide, define:

- `role`: cover, agenda, context, insight, framework, comparison, process, data, case, recommendation, closing, or appendix.
- `takeaway`: one sentence that the viewer should remember.
- `visual_intent`: what the generated page should make visually obvious.
- `content`: concise bullets or data labels.
- `prompt`: an image-generation prompt that combines the slide-specific content with the shared visual system.
- `editable_elements`: optional exact PowerPoint text overlays for the editable deck.
- `qa_notes`: specific issues to check after generation, such as exact terms, numbers, axis labels, or brand-sensitive imagery.

For `hybrid_editable_text`, prompt the image model to reserve clean text zones and use abstract placeholders instead of final exact text. Put exact wording into `editable_elements`.

Keep generated slide text short. GPT Image models can render text, but exact long paragraphs, tiny labels, tables, legal text, and dense charts are high risk. For exact text fidelity, generate the visual background and add editable PowerPoint text manually after image generation.

## Visual Consistency

Read `references/visual-system.md` when building or reviewing the visual system.

Use the same palette, typography hints, UI surfaces, icon style, spacing logic, and composition rules across all prompts. Repeat stable design tokens directly inside every image prompt so each slide stays aligned even when generated independently.

Each slide prompt should include:

- deck title and slide number context
- slide role and takeaway
- exact visual system tokens
- layout instructions for the slide type
- clean reserved zones for editable overlays
- negative instructions that prevent unrelated colors, random UI styles, photorealistic clutter, inconsistent logos, and decorative gradients

## Editable PPT

Read `references/editable-ppt.md` before producing the final editable deck.

The editable version is practical, not magical: it uses approved generated images as visual backgrounds and adds critical text, numbers, citations, and labels as editable PowerPoint objects. Do not claim every visual element is editable unless a separate shape-rebuild workflow is performed.

## Plan File

Create a JSON file with this shape:

```json
{
  "deck": {
    "title": "Product Strategy 2026",
    "subtitle": "From fragmented signals to focused execution",
    "language": "zh-CN",
    "aspect_ratio": "16:9",
    "generation_mode": "hybrid_editable_text"
  },
  "visual_system": {
    "style_name": "executive product dashboard",
    "palette": {
      "background": "#F7F8FA",
      "surface": "#FFFFFF",
      "primary": "#1E4FD7",
      "accent": "#18A999",
      "text": "#111827",
      "muted": "#667085"
    },
    "typography": "modern sans-serif, clear hierarchy, large title, compact labels",
    "ui_motif": "clean dashboard panels, thin dividers, small status chips, restrained icons",
    "composition": "12-column grid, generous margins, consistent top title area"
  },
  "slides": [
    {
      "number": 1,
      "role": "cover",
      "title": "Product Strategy 2026",
      "takeaway": "Focus turns scattered opportunities into compounding execution.",
      "visual_intent": "A calm executive cover with a clear strategic focus metaphor.",
      "content": ["Product Strategy 2026", "Focus, sequencing, execution"],
      "editable_elements": [
        {
          "type": "text",
          "text": "Product Strategy 2026",
          "x": 0.7,
          "y": 0.55,
          "w": 8.5,
          "h": 0.7,
          "font_size": 34,
          "bold": true,
          "color": "#111827"
        }
      ],
      "qa_notes": ["Check title spelling", "Check palette consistency"],
      "prompt": "Create a 16:9 full-slide presentation cover..."
    }
  ]
}
```

The script validates required fields, generates image files, creates a visual-proof `.pptx`, and optionally creates an editable `.pptx`.

## Script Notes

`scripts/super_ppt_pipeline.py` is intentionally a thin pipeline:

- validate `plan.json`
- create one prompt per slide
- optionally call the OpenAI Images API with the selected model
- save generated images
- assemble a visual-proof `.pptx` with full-bleed slide images
- optionally assemble an editable `.pptx` with approved images plus editable text layers

Install missing runtime dependencies only when needed. Typical dependencies are `openai` and `python-pptx`.

Use `--dry-run` before any network/API generation. Use `--image-dir` and `--reuse-images` to reuse approved images when building the editable deck.

The script writes a manifest so weak slides can be regenerated without losing traceability. Keep the manifest with the deck when reviewing or iterating.

## Quality Check

Before delivering a deck:

1. Confirm every slide maps to the narrative arc and has one clear takeaway.
2. Check that all prompts include the same visual system tokens.
3. Inspect generated images for text errors, color drift, layout inconsistency, or off-brand elements.
4. Replace or regenerate weak slides before assembly.
5. Open or render the visual-proof `.pptx` and verify that images are full bleed, correctly cropped, and in the intended order.
6. Open or render the editable `.pptx` and verify that editable text overlays align with reserved image zones.

## Stability Rules

Prefer stability over maximum novelty:

- Generate from a validated plan, not directly from raw notes.
- Keep prompts explicit and repetitive about the shared visual system.
- Use consistent slide dimensions and image size across the whole deck.
- Avoid asking the image model to solve heavy reasoning and typography in the same prompt; solve reasoning in the plan first.
- Regenerate one weak slide at a time instead of regenerating the whole deck.
- Use `hybrid_editable_text` for exact text, numbers, citations, source names, and detailed charts.
- Save `plan.json`, `prompts.md`, generated images, manifest, visual-proof `.pptx`, and editable `.pptx` together for reproducibility.
- Treat `super-ppt-visual.pptx` as the visual approval artifact. Treat `super-ppt-editable.pptx` as the final work artifact.
