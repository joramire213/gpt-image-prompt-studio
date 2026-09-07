# GPT Image 2 — Model Capabilities and Prompting Rules

What this model actually is, what it can and cannot do, and how OpenAI says to write for it.
Read this before writing any final prompt. The style library tells you *which* visual direction to
take; this file tells you how to phrase it so GPT Image 2 executes it faithfully.

Sources: OpenAI's *GPT Image Generation Models Prompting Guide* (developer cookbook), the
gpt-image-2 model page, and the launch announcement. Model shipped 2026-04-21.

## Contents

- [Identity](#identity)
- [Size and aspect ratio](#size-and-aspect-ratio)
- [Quality levels](#quality-levels)
- [Prompt order](#prompt-order)
- [Rendering text inside the image](#rendering-text-inside-the-image)
- [Composition, camera, lighting](#composition-camera-lighting)
- [Style and materials](#style-and-materials)
- [Fighting the synthetic look](#fighting-the-synthetic-look)
- [Constraints and exclusions](#constraints-and-exclusions)
- [Known weaknesses](#known-weaknesses)
- [Transparent backgrounds](#transparent-backgrounds)
- [Iterating on a result](#iterating-on-a-result)
- [Worked examples](#worked-examples)

## Identity

- Model ID: `gpt-image-2` (default snapshot `gpt-image-2-2026-04-21`).
- Endpoints: `/v1/images/generations` and `/v1/images/edits` (generation and inpainting).
- In ChatGPT it is branded "ChatGPT Images 2.0" and runs on every plan.
- It reasons before it draws. Unlike a pure diffusion model, it works from stated intent and
  self-checks, which is why explicit constraints pay off and vague adjectives waste the budget.
  Write for a model that reads instructions, not one that matches keywords.
- Stated strengths over the previous generation: multilingual text rendering, structured generation
  (diagrams, infographics, charts, posters, comics), higher resolution, photorealism, and UI
  screenshots.

## Size and aspect ratio

The `size` parameter takes an arbitrary `WIDTHxHEIGHT` string, subject to hard constraints:

- Both edges must be **multiples of 16**.
- Maximum edge: **3840 px**.
- Aspect ratio must fall between **1:3 and 3:1**. Nothing more extreme renders.
- Total pixels between **655,360 and 8,294,400**.
- Reliable up to **2560x1440**. Above that the model is experimental and quality degrades.

Common sizes worth naming:

| Use | Size | Ratio |
|---|---|---|
| Square (social, avatars, product) | `1024x1024` | 1:1 |
| Portrait (stories, posters, phone) | `1024x1536` | 2:3 |
| Landscape (slides, banners, diagrams) | `1536x1024` | 3:2 |
| Native widescreen | `1536x864` | 16:9 |
| Native vertical | `864x1536` | 9:16 |
| 2K / QHD (top of the reliable range) | `2560x1440` | 16:9 |

A prompt written for a human to paste into ChatGPT has no `size` flag, so state the ratio in plain
language: `Aspect ratio 16:9.` Pick the shape that matches where the image will actually live
rather than defaulting to square.

## Quality levels

- `low` — fast exploration, high volume, throwaway drafts.
- `medium` — the general-purpose default.
- `high` — needed for small text, dense information panels, multi-font layouts, infographics, and
  identity-sensitive edits. If the image carries more than a headline's worth of text, say high.

## Prompt order

OpenAI's recommendation: **background/scene → subject → key details → constraints**, kept in a
consistent order so that when a result comes back wrong you can tell which block caused it.

Note this puts scene before subject, the reverse of most prompt guides. It works because the model
establishes the world first and then places the subject inside it.

Format is flexible — "minimal prompts, descriptive paragraphs, JSON-like structures,
instruction-style prompts, and tag-based prompts can all work well" — as long as intent is
unambiguous. Choose the shape deliberately: prose for photographic and narrative work, sectioned
prose for multi-zone posters, numbered panels for grids of specified cells, JSON for style locks and
for any series that must stay visually consistent across renders. `examples.md` has a worked prompt
for each. What fails is not choosing — an unstructured pile of adjectives produces mush.

Flags borrowed from other tools do nothing here. `--ar 2:3`, `--v`, `--style` are Midjourney syntax;
state the ratio in words instead.

Long prompts work, but start from a clean base and add to it. Debugging an overloaded first draft
is far harder than growing a simple one.

## Rendering text inside the image

This is where most image prompts fail, and where GPT Image 2 is strongest if addressed correctly.

- **Quote literal text, or write it in ALL CAPS.** `a neon sign reading "OPEN LATE"`. Quoting is the
  signal that switches the model into faithful text rendering rather than decorative lettering.
- **Spell out tricky words letter by letter** — brand names, invented words, unusual spellings.
- **Demand verbatim output**: "EXACT, verbatim, no extra characters."
- **State that it appears once**: "no extra words", "no duplicate text". Repeated and hallucinated
  text is the most common failure mode.
- **Specify typography**: font style, weight, size relative to the frame, color, placement,
  kerning. `Typography: bold sans-serif, high contrast, centered, clean kerning.`
- **Keep headlines short.** Long strings are harder to render cleanly. If the copy is long, ask for
  high quality and accept fewer words in larger type.
- Non-Latin scripts are supported, including Japanese, Korean, Chinese, Hindi and Bengali.

## Composition, camera, lighting

- **Framing and viewpoint**: close-up, medium shot, wide, top-down.
- **Perspective and angle**: eye-level, low-angle, bird's-eye.
- **Placement, when layout matters**: "logo top-right", "subject centered", "generous negative space
  on the left third".
- **Lighting and mood**: soft diffuse, golden hour, high-contrast, overcast, neon.
- **Depth**: shallow depth of field, bokeh, film grain.

Choose depth of field by what the image needs to prove. Shallow depth of field isolates a subject —
right for a portrait or a product, wrong for a scene whose whole point is that a real, recognizable
place is still legible: a wide environmental or architectural shot needs enough of the frame in
focus that the identity anchors read clearly, not just the nearest object.

Detailed camera specs (exact lens, f-stop) are interpreted loosely. Use them to set an overall look,
not as a technical instruction the model will honour precisely.

Wide, cinematic, low-light, rain or neon scenes need *extra* detail about scale, atmosphere and
color — they degrade fastest when underspecified.

## Style and materials

Be concrete about materials, shapes, textures, and the visual medium: photo, watercolor, 3D render,
vector, ink. Vague style adjectives ("beautiful", "modern", "professional") consume words without
constraining anything.

For photorealism, include the word **"photorealistic"** explicitly.

Add targeted quality levers only when they earn their place — film grain, textured brushstrokes,
macro detail. Piling them all on flattens the result.

When people appear, describe scale, body framing, gaze, and how they interact with objects.

## Fighting the synthetic look

Naming a medium ("photorealistic", "35mm documentary") constrains content but not *feel*. A prompt
can satisfy every content requirement and still read as concept art or a movie still, because
nothing told the model to avoid composing for drama. Seven techniques address feel directly.

**State it outright.** A line like "the scene must feel observed rather than designed; nothing
should appear deliberately arranged for dramatic effect" measurably pushes the result away from
symmetric, curated, hero-shot composition and toward the accidental framing of a real photograph.
Use it whenever the goal is documentary or candid realism, not just when things look posed —
by the time a result looks posed it is too late.

**Give a rendering-cliché checklist for realism.** These recur across image models and are worth
excluding by default whenever "photorealistic" is the goal, not only when a result comes back
looking synthetic:

```
Avoid teal-and-orange color grading, excessive HDR, artificial lens flares, unrealistically deep
blacks or glowing highlights, and any game-engine or matte-painting sheen.
```

**Make elapsed time look like elapsed time.** Any prompt that states a duration since an event —
"twenty years after", "a decade of neglect", "long abandoned" — needs that duration written into
the *kind* of damage, or the model defaults to depicting the event itself rather than its aftermath.
Contrast "burnt debris and broken glass" (reads as yesterday) with "biological growth overtaking
water stains, mineral deposits built up over years, structures weathered rather than freshly broken"
(reads as decades). State explicitly that damage should look cumulative and old, not fresh.

**Use frame scale to tell the story.** Making a human figure small relative to an enormous
environment communicates scale and discovery; filling the frame with a posed figure communicates a
portrait session. When the point of an image is "a vast, transformed place with someone in it"
rather than "a portrait in a location", say so directly: describe the figure occupying a small
portion of the frame, positioned so the environment reads first and the person is found second.

**Name the mechanism, not just the state, for anything that shouldn't obviously persist.** Ordinary
decay needs no justification — rust, overgrowth and cracking are self-evidently what time does. But
a state that isn't obviously permanent on its own — a flood that never drained, a city stuck in
permanent winter, a structure that should have collapsed but hasn't — reads as arbitrary or as a
snapshot of a passing event unless the prompt names *why* it lasted. One clause is enough: "the
lowest streets stayed flooded because the drainage system was never restored" does more work than
repeating that the streets are flooded. This is a physical-plausibility problem, not a wording
problem — solve it with one causal fact, not more adjectives about the water.

One failure mode this invites: describing the same element as both settled-permanent and
still-changing at once — "permanently flooded" alongside "floodwater that sat for years before
slowly receding" is two different temporal claims about the same water. Pick one: either the
element reached a stable state and has stayed there (describe the current state, once), or it is
still in a slow process (describe the process, once). Never both for the same element.

**Keep destruction mundane, not spectacular.** Aftermath and disaster scenes have their own
cliché to fight, distinct from staging or color grading: the pull toward "ruin porn" — collapsed
skyscrapers, giant rubble piles, exposed rebar silhouetted against fire. A scene can pass every other
check in this section and still look like a disaster movie poster if scale of destruction is left
unconstrained. State the restraint directly:

```
The destruction is extensive but visually mundane and believable. Avoid spectacular collapse, giant
rubble piles, explosions, fire, or exaggerated apocalyptic imagery.
```

The target feel is "someone photographed this place after everyone left", not "Hollywood destroyed
this place" — those are different images even when the underlying facts are identical.

**Don't name an archetype when something else needs to win.** A famous artist's name, a well-known
subculture label, a stock character type ("hair-metal fashion icon", "film noir detective") carries
its own strong, prototypical visual template from training — and that template competes with
whatever else the prompt is trying to control. This matters most when a reference photo's identity,
or a specific original composition, has to dominate: naming the archetype pulls toward its generic,
most-represented instance instead. Describe the function or the era ("a television host on a 1980s
music program") and let concrete wardrobe and scene details carry the aesthetic, rather than
reaching for the label that already has a face in the model's memory.

## Constraints and exclusions

There is **no negative-prompt parameter**. Exclusions are written as explicit statements inside the
prompt, and they work well because the model reasons over instructions:

```
Constraints:
- No watermarks
- No extra text beyond the headline
- No logos or trademarks
- Do not add elements that were not requested
```

State invariants — what must be preserved — as plainly as exclusions. Both are honoured.

Exclusions are cheap only when they target something the rest of the prompt could plausibly
produce. A documentary scene of weathered concrete and rising tide has no real chance of generating
zombies, fantasy creatures or military hardware on its own — negating them anyway doesn't protect
against anything, it just spends attention. Reserve the constraints block for failure modes that
this specific prompt could actually hit: the genre it is nowhere near does not need a name-check.

## Known weaknesses

OpenAI documents these. Design the prompt around them rather than hoping:

- **Precise text placement and clarity** can still fail, especially at small sizes.
- **Layout-sensitive compositions** — the model may not place elements exactly where specified in
  rigid grids. Describe hierarchy and relative position rather than pixel coordinates.
- Complex prompts can take up to two minutes to process.

## Transparent backgrounds

Requires all of: `background="transparent"`, `output_format="png"` or `"webp"`, no
`output_compression` for PNG, and an explicit request in the prompt for an isolated subject on a
fully transparent background. For edits, re-state the transparent background every time.

Ask for "clean alpha edges, no halos or fringing, no solid backdrop, no checkerboard, no scenery, no
drop shadow" — otherwise the model invents a background substitute.

## Iterating on a result

Refine with small, single-change follow-ups: "make the lighting warmer", "remove the extra tree".
Do not rewrite the whole prompt to fix one thing.

References like "same style as before" work, but re-specify the critical details as soon as they
start to drift. Edits accumulate error; restating invariants resets it.

## Worked examples

These are OpenAI's own examples. Note the shape: scene, subject, concrete detail, then a short
constraint block. Study the pattern rather than reusing the content.

**Photorealistic portrait** — `1024x1536`, quality medium:

> Create a photorealistic candid photograph of an elderly sailor standing on a small fishing boat.
> He has weathered skin with visible wrinkles, pores, and sun texture, and a few faded traditional
> sailor tattoos on his arms. He is calmly adjusting a net while his dog sits nearby on the deck.
> Shot like a 35mm film photograph, medium close-up at eye level, using a 50mm lens. Soft coastal
> daylight, shallow depth of field, subtle film grain, natural color balance. The image should feel
> honest and unposed, with real skin texture, worn materials, and everyday detail. No glamorization,
> no heavy retouching.

**Explanatory diagram** — `1536x1024`, quality high:

> Create a simple biology diagram titled "Cellular Respiration at a Glance" for high school
> students. Show how glucose turns into energy inside a cell. Include glycolysis, the Krebs cycle,
> and the electron transport chain. Use arrows to connect the steps, and label the main molecules:
> glucose, pyruvate, ATP, NADH, FADH2, CO2, O2, and H2O. Make it look like a clean classroom handout
> or slide, with a white background, simple icons, clear labels, and easy-to-read text. Avoid tiny
> text, extra decoration, or anything that makes the diagram hard to understand.

**Campaign ad with exact copy** — `1024x1536`, quality medium:

> Give me a cool in culture ad / fashion shot for a brand called Thread. It's a hip young street
> brand. The ad shows a group of friends hanging out together with the tagline "Yours to Create."
> Make it feel like a polished campaign image for a youth streetwear audience: stylish,
> contemporary, energetic, and tasteful. Use clean composition, strong color direction, natural
> poses, and premium fashion photography cues. Render the tagline exactly once, clearly and legibly,
> integrated into the ad layout. No extra text, no watermarks, no unrelated logos.

**Logo on transparency** — `1024x1536`, quality medium, transparent background, PNG:

> Create an original, non-infringing logo for a company called Field & Flour, a local bakery. The
> logo should feel warm, simple, and timeless. Use clean, vector-like shapes, a strong silhouette,
> and balanced negative space. Favor simplicity over detail so it reads clearly at small and large
> sizes. Flat design, minimal strokes, no gradients unless essential. Fully transparent background.
> Deliver a single centered logo with generous padding, clean alpha edges, and no solid backdrop,
> scenery, checkerboard, or watermark.
