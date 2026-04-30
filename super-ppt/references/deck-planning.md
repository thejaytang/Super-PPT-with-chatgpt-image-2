# Deck Planning Reference

Use this reference before image generation. Quality depends more on planning than on prompt length.

## Planning Contract

Every deck needs:

- `main_message`: one sentence that defines the deck's point.
- `audience`: who will read or hear it.
- `decision_or_action`: what the audience should decide, understand, or do.
- `narrative_arc`: the order of ideas.
- `slide_budget`: target slide count and maximum slide count.
- `generation_mode`: `hybrid_editable_text` by default, or `full_slide_image` when editability is not required.

## Narrative Patterns

Use the simplest pattern that fits the source.

Problem to Solution:

- Situation
- Problem
- Root cause
- Solution
- Evidence
- Next steps

Executive Recommendation:

- Recommendation
- Why now
- Options considered
- Chosen path
- Risks and mitigations
- Ask or decision

Teaching or Explainer:

- Hook
- Concept map
- Core idea 1
- Core idea 2
- Example
- Recap

Product or Strategy:

- Context
- Market/user shift
- Strategic focus
- Roadmap or operating model
- Success metrics
- Call to action

## Slide Budget

Use these defaults unless the user gives a target:

- 5-7 slides: short update, memo summary, class explainer.
- 8-12 slides: normal business presentation.
- 12-18 slides: strategy, research, or investor-style deck.
- 20+ slides: only when source material has clear sections or appendices.

Avoid adding slides only to use more visuals. Each slide needs a distinct role and takeaway.

## Content Density

For `full_slide_image` mode:

- 1 title or headline.
- 1 takeaway.
- 3-5 short labels or bullets.
- 0-1 simple chart or diagram.

For `hybrid_editable_text` mode:

- Let the generated image handle background, layout, illustration, and visual metaphor.
- Add exact titles, bullets, numbers, citations, and charts as editable PowerPoint objects after generation.
- Define `editable_elements` for exact text and ask the image model to reserve readable space for those layers.

## Prompt Stability

Do:

- Repeat the same palette, typography, composition, and UI motif in every slide prompt.
- State the slide role and takeaway clearly.
- Keep slide text short and provide exact text only when needed.
- Ask for one dominant visual idea.

Do not:

- Ask for multiple unrelated metaphors on one slide.
- Ask for dense tables or tiny footnotes inside generated images.
- Mix visual styles across slides unless the plan explicitly uses section-level variants.
- Use vague style words without design tokens.

## Failure Recovery

If a generated slide is weak, diagnose before regenerating:

- Text wrong: shorten text, switch to `hybrid_editable_text`, or add text manually.
- Layout crowded: reduce content items and ask for more whitespace.
- Colors drift: repeat hex values and add stronger negative rules.
- Style inconsistent: restate `style_name`, `ui_motif`, `image_style`, and slide family.
- Visual metaphor unclear: replace abstract prompt wording with concrete objects or diagrams.
- Chart inaccurate: use editable charts instead of generated chart pixels.
