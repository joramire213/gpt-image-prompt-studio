# GPT Image — Model Capabilities and Prompting Rules

What these models actually are, what they can and cannot do, and how OpenAI says to write for them.
Read this before writing any final prompt. `templates.md` tells you *which* visual direction to
take; this file tells you how to phrase it so the model executes it faithfully.

Sources: OpenAI's [Image prompting guide](https://developers.openai.com/api/docs/guides/image-prompting),
the [image generation guide](https://developers.openai.com/api/docs/guides/image-generation), and the
per-model pages and the deprecations page. Current as of 2026-11-16.

## Contents

- [The lineup](#the-lineup)
- [Choosing a model](#choosing-a-model)
- [Size and aspect ratio](#size-and-aspect-ratio)
- [Quality levels](#quality-levels)
- [Complexity](#complexity)
- [In ChatGPT: the reasoning level](#in-chatgpt-the-reasoning-level)
- [Prompt order](#prompt-order)
- [Rendering text inside the image](#rendering-text-inside-the-image)
- [Working from reference images](#working-from-reference-images)
- [Composition, camera, lighting](#composition-camera-lighting)
- [Style and materials](#style-and-materials)
- [Fighting the synthetic look](#fighting-the-synthetic-look)
- [Constraints and exclusions](#constraints-and-exclusions)
- [Known weaknesses](#known-weaknesses)
- [Transparent backgrounds](#transparent-backgrounds)
- [Vector (SVG) output](#vector-svg-output)
- [Animated GIF output](#animated-gif-output)
- [Iterating on a result](#iterating-on-a-result)
- [Checking the output](#checking-the-output)
- [Worked examples](#worked-examples)

## The lineup

GPT Image 2.6 launched 2026-11-04 and is the current generation. All models below take text and
image input, output images, and support both `/v1/images/generations` and `/v1/images/edits`.

| Model | Snapshot | Character |
|---|---|---|
| `gpt-image-2.6-sunburst` | none listed | Base model, *"for workflows that need more detail or precise edits."* The default. |
| `gpt-image-2.6-flare` | none listed | Smaller model, *"for workflows where speed matters most."* |
| `gpt-image-2.5-sunburst` | `-2026-09-08` | Previous generation, quality-oriented. Still available, no deprecation notice. |
| `gpt-image-2.5-flare` | `-2026-09-08` | Previous generation, speed-oriented. Still available, no deprecation notice. |
| `gpt-image-2` | `-2026-04-21` | Deprecated 2026-11-10, **shuts down 2027-02-10**. Quality ladder stops at `high`. |

Only 2.6 has `complexity`, SVG output and animated GIF output. Markup-guided edits exist on both
2.6 and 2.5. The 2.6 model pages list no dated snapshot, so the model ID is the identifier to use.

Do not recommend anything older: `gpt-image-1.5`, `gpt-image-1-mini` and `chatgpt-image-latest`
shut down **2026-12-01**; `gpt-image-1` already shut down on 2026-10-23. OpenAI names the 2.6 pair
as the replacement for all of them.

**Sunburst and Flare cost exactly the same**, in 2.6 as in 2.5: $5 per million input text tokens
($1.25 cached), $8 per million input image tokens ($2 cached), $30 per million output image tokens.
Choosing Flare over Sunburst buys latency, never money — OpenAI's own guide warns against assuming
otherwise: *"Confirm current pricing rather than assuming the faster model costs less."*

In ChatGPT, image generation runs on "ChatGPT Images 2.6" across all plans. ChatGPT also lets the
user draw on an image to mark where an edit goes (the counterpart of API markup), and animated GIF
creation is rolling out gradually in the app. SVG output is documented only for the API.

What 2.6 improved over 2.5, per OpenAI: faster generation, and better preservation of details
during edits — a character's clothing surviving a background change, for instance. OpenAI publishes
no side-by-side quality benchmark, so treat these as the vendor's claims.

What 2.5 had improved over 2.0: more natural lighting and richer textures, better preservation of
subjects from the user's reference photos, more reliable adherence to editing instructions across
multiple turns, and sketch- and template-based generation.

These models reason before they draw. They work from stated intent and self-check, which is why
explicit constraints pay off and vague adjectives waste the budget. Write for a model that reads
instructions, not one that matches keywords.

## Choosing a model

OpenAI's stated decision rule, which carries over unchanged to 2.6:

- If GPT Image 2's quality already met your bar, **start with Flare** and see how much latency you
  can recover.
- If you have a hard case where GPT Image 2 fell short, **start with Sunburst**, establish the
  quality first, then test Flare with the same prompts, references, sizes and quality.

Between generations, prefer 2.6 for new work: same price, and the only line with `complexity`, SVG
and GIF. Keep a user on 2.5 only when they already have an approved 2.5 workflow they do not want
to re-validate.

One caution worth carrying: *"The same quality label does not imply the same image quality or
response time across models."* A `high` on Flare is not a `high` on Sunburst. Compare by running
identical prompts, references and dimensions, not by trusting the label.

## Size and aspect ratio

The `size` parameter takes an arbitrary `WIDTHxHEIGHT` string (or `auto`), subject to hard
constraints that are **unchanged from GPT Image 2**:

- Both edges must be **multiples of 16**.
- Maximum edge: **3840 px**.
- Aspect ratio must fall between **1:3 and 3:1**. Nothing more extreme renders.
- Total pixels between **655,360 and 8,294,400**.
- Above **3,686,400 px** — the pixel count of `2560x1440` — output is **experimental**.

These are documented for 2.5. The 2.6 guide restates only the multiple-of-16 and 1:3–3:1 rules;
apply the full set to 2.6 as well, since nothing indicates a change, but the experimental threshold
for 2.6 is not separately confirmed.

Common sizes worth naming:

| Use | Size | Ratio | Note |
|---|---|---|---|
| Square (social, avatars, product) | `1024x1024` | 1:1 | |
| Portrait (stories, posters, phone) | `1024x1536` | 2:3 | |
| Landscape (slides, banners, diagrams) | `1536x1024` | 3:2 | |
| Native widescreen | `1536x864` | 16:9 | |
| Native vertical | `864x1536` | 9:16 | |
| 2K landscape | `2048x1152` | 16:9 | |
| Top of the non-experimental range | `2560x1440` | 16:9 | |
| 2K square | `2048x2048` | 1:1 | experimental |
| 4K landscape | `3840x2160` | 16:9 | experimental |
| 4K portrait | `2160x3840` | 9:16 | experimental |

The last three are listed by OpenAI as common sizes *and* sit above the experimental threshold —
both are true. They are legitimate for print or large-format work; just name the tradeoff when you
pick one. "Experimental" means the quality guarantee stops there, not that it fails.

A prompt written for a human to paste into ChatGPT has no `size` flag, so state the ratio in plain
language: `Aspect ratio 16:9.` Pick the shape that matches where the image will actually live
rather than defaulting to square.

## Quality levels

`low`, `medium`, `high`, `xhigh`, `max`, `auto`. `xhigh` and `max` arrived with 2.5, continue in
2.6, and are not supported by earlier GPT Image models, whose ladder stops at `high`.

Quality *"controls how much visual detail the model can render, such as texture or small
lettering."* That definition matters now that 2.6 has a second lever next to it.

`auto` is the API default. Everything below is about what to *choose* deliberately, which is a
different question from what happens when you choose nothing.

- `low` — fast exploration, high volume, throwaway drafts.
- `medium` — the sensible general-purpose pick.
- `high` — small text, dense information panels, multi-font layouts, infographics,
  identity-sensitive edits. If the image carries more than a headline's worth of text, say high.
- `xhigh` / `max` — reach for these only with a reason. OpenAI is explicit: *"Use xhigh or max only
  when they improve an unmet quality requirement within your latency budget. A higher setting
  doesn't guarantee a better result for every prompt."*

The recommended process is to set a baseline, step up only if the baseline misses a requirement,
then step back down once approved to recover latency. Tune one setting at a time; compare quality
levels before rewriting the prompt.

## Complexity

New in 2.6, and only there: `low`, `medium`, `high`. It *"controls how much the model plans before
rendering"*, and both Sunburst and Flare use `high` when the field is omitted. Quality decides how
finely the image is drawn; complexity decides how much the model rethinks the composition first.

- `high` (the default) earns its place when the prompt leaves design decisions open. OpenAI's
  comparison ran one under-specified sports illustration at all three levels: `low` added a crown
  to the trophy, `medium` added a laurel wreath and shirt crests, `high` moved the banner into the
  stands and cleared the foreground for the players. More planning means more of the model's own
  ideas.
- *"When the prompt already sets the layout, text, and style, higher complexity may make little
  visible difference."* A fully specified prompt gains little from it.
- `low` — *"Start with `low` when you want to change one detail."* High complexity can add latency
  and introduce changes nobody asked for, which is exactly the wrong trade for a targeted edit.

Keep the two levers apart when tuning: hold quality fixed while comparing complexity. *"If the
layout is correct but small text is hard to read, compare quality settings at the same
complexity."* A layout problem is a complexity or prompt question; a legibility problem is a quality
question.

## In ChatGPT: the reasoning level

ChatGPT exposes none of the API settings. The one lever the user holds is the reasoning level in
the model picker — a fast mode, or a Thinking/Pro model on paid plans. What is documented:

- OpenAI's Images 2.6 system card describes it as giving users *"more control over generation and
  editing speed"* — officially, a speed control.
- Since ChatGPT Images 2.0, image generation with thinking *plans and refines before generating*.

**Inference, not documented:** the reasoning level most likely plays the role `complexity` plays in
the API — more planning, more of the model's own composition decisions. Recommend it on that basis,
and say it is a recommendation, not a guarantee:

- **Con razonamiento** — open-ended compositions, dense or exact text, infographics, diagrams,
  multi-zone layouts, combining several references.
- **Rápido** — prompts that already fix layout, text and style; targeted edits that change one
  detail (more planning can rework what the preserve list protects); quick exploration.

## Prompt order

Start by defining the result: *"Name the subject and intended use, such as a product photograph,
advertisement, or diagram."* Then organize scene, subject, details and constraints — for complex
requests, in labeled sections.

The ordering that works: **background/scene → subject → key details → constraints**, kept
consistent so that when a result comes back wrong you can tell which block caused it. Note this
puts scene before subject, the reverse of most prompt guides. It works because the model
establishes the world first and then places the subject inside it.

Format is flexible — *"Short prompts, descriptive paragraphs, JSON-like structures, instructions,
and tags can all express the same intent."* Choose the shape deliberately: prose for photographic
and narrative work, sectioned prose for multi-zone posters, numbered panels for grids of specified
cells, JSON for style locks and for any series that must stay visually consistent across renders.
`examples.md` has a worked prompt for each. What fails is not choosing — an unstructured pile of
adjectives produces mush. Pick whatever will be easiest to re-read and update later.

Flags borrowed from other tools do nothing here. `--ar 2:3`, `--v`, `--style` are Midjourney syntax;
state the ratio in words instead.

Long prompts work, but start from a clean base and add to it. Debugging an overloaded first draft
is far harder than growing a simple one.

## Rendering text inside the image

This is where most image prompts fail, and where these models are strongest if addressed correctly.

- **Quote literal text, or write it in ALL CAPS.** `a neon sign reading "OPEN LATE"`. Quoting is the
  signal that switches the model into faithful text rendering rather than decorative lettering.
- **Describe its position and typography** alongside the wording.
- **Spell out tricky words letter by letter** — brand names, invented words, unusual spellings.
- **Demand verbatim output**: "EXACT, verbatim, no extra characters."
- **State that it appears once**: "no extra words", "no duplicate text". Repeated and hallucinated
  text is the most common failure mode.
- **Keep headlines short.** Long strings are harder to render cleanly. If the copy is long, ask for
  higher quality and accept fewer words in larger type.
- Non-Latin scripts are supported, including Japanese, Korean, Chinese, Hindi and Bengali.
- Check spelling and legibility in the output. This is not a step you can skip on trust.

## Working from reference images

2.5 preserves reference subjects better than 2.0 did, and 2.6 better still across edits, but the prompt still carries most of the
weight. Three techniques, all from OpenAI's guide.

**Assign roles to the references.** *"Identify each input by number and purpose: subject, style,
clothing, or background. Explain how the inputs should combine."* With more than one reference,
leaving the model to infer which is which is the single easiest thing to get wrong.

**Separate changes from constraints.** *"For edits, say 'change only X' and list the details to
preserve, such as identity, geometry, layout, lighting, or labels."* The preserve list is not
padding — it is the instruction doing the work. OpenAI's own example:

```
Do not change her face, facial features, skin tone, body shape, pose, or identity in any way.
Preserve her exact likeness, expression, hairstyle, and proportions. Replace only the clothing...
```

**Repeat the preserve list on every iteration.** Drift accumulates across turns; restating the
invariants resets it. If you need a pixel-identical region kept, prompting alone is the wrong tool —
*"composite the approved edit into the original image instead."*

## Composition, camera, lighting

- **Framing and viewpoint**: close-up, medium shot, wide, top-down.
- **Perspective and angle**: eye-level, low-angle, bird's-eye.
- **Placement, when layout matters**: "logo top-right", "subject centered", "generous negative space
  on the left third".
- **Lighting and mood**: soft diffuse, golden hour, high-contrast, overcast, neon.
- **Depth**: shallow depth of field, bokeh, film grain.

For people, describe *"body framing, relative scale, gaze, and interaction with objects."*
Instructions like "full body visible, feet included", "looking down at the open book", or "hands
naturally gripping the handlebars" make the intended pose concrete in a way adjectives never do.

Choose depth of field by what the image needs to prove. Shallow depth of field isolates a subject —
right for a portrait or a product, wrong for a scene whose whole point is that a real, recognizable
place is still legible: a wide environmental or architectural shot needs enough of the frame in
focus that the identity anchors read clearly, not just the nearest object.

Camera specs are *"cues for appearance, not a guarantee of exact physical simulation."* Use them to
set an overall look, not as a technical instruction the model will honour precisely.

Wide, cinematic, low-light, rain or neon scenes need *extra* detail about scale, atmosphere and
color — specify those instead of relying on mood words alone. They degrade fastest when
underspecified.

## Style and materials

Be concrete about materials, shapes, textures, and the visual medium: photo, watercolor, 3D render,
vector, ink. Vague style adjectives ("beautiful", "modern", "professional") consume words without
constraining anything.

For photorealism, include the word **"photorealistic"** explicitly.

Add targeted quality levers only when they earn their place — film grain, textured brushstrokes,
macro detail. Piling them all on flattens the result.

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
- **Visual consistency across multiple generations** is hard to hold. A JSON style lock helps; it
  does not guarantee.
- Complex prompts can take up to two minutes to process.

## Transparent backgrounds

Requires all of: `background="transparent"`, `output_format="png"` or `"webp"`, no
`output_compression` for PNG, and an explicit request in the prompt for an isolated subject on a
fully transparent background. For edits, re-state the transparent background every time.

Ask for "clean alpha edges, no halos or fringing, no solid backdrop, no checkerboard, no scenery, no
drop shadow" — otherwise the model invents a background substitute.

Then verify it actually worked: *"Check the decoded image's alpha channel, including hair, glass,
shadows, and object edges."* A painted checkerboard is not transparency, and it is a common enough
substitution that the check is worth doing every time.

## Vector (SVG) output

2.6 only, generations only: `output_format="svg"` on `/v1/images/generations` with `n=1`.
For a transparent background, pass `background="transparent"` alongside it and ask for it in the
prompt, as with PNG. Omit `stream`, `partial_images` and `output_compression`. The result
is static XML: no scripts, no external resources, no embedded raster images. It cannot be produced
through the edits endpoint, so any reference-guided refinement happens on a raster image first;
the SVG prompt then describes the finished design from scratch. Whether ChatGPT returns SVG from
the image model is unclear: the Images 2.6 system card limits SVG output to the API, while the
launch announcement mentions "SVG exports" without saying where. For a ChatGPT user, prefer the
code route below; if they want the image-model route anyway, tell them to check they got a real
vector file and not a PNG.

**Two routes to an SVG.** The image model's SVG output is one. The other is asking a text model —
ChatGPT itself, or any LLM — to *write* the SVG code. Choose by what the asset is:

- **Code route:** simple UI icons, pictograms, geometric marks — anything a designer would draw
  with a handful of strokes on a grid. The result is minimal, editable markup that can match an
  existing icon set (a `24x24` viewBox, `stroke="currentColor"`). It works in ChatGPT today with no
  image feature involved. It is weak at anything organic or illustrative: a text model placing
  curves by coordinates produces stiff shapes.
- **Image-model route:** logos with character, illustrations, stickers — shapes that need an eye,
  not coordinates. API only.

When the code route fits, the deliverable is still a prompt, but a code-writing one, in English like
every other prompt. Open with "Write the SVG code as text. Do not generate an image." — ChatGPT
tends to trigger image generation when asked for an icon and returns a PNG. Then specify: viewBox
and display size; outline or filled, with stroke width, caps and joins; the composition as simple
shapes; minimal markup (only `<path>` and basic shapes, no `<style>`, ids, classes, transforms or
metadata, coordinates rounded); legibility at the smallest size it will display; and
`aria-hidden="true"` when it sits next to a text label. If the user's other icons have a known
style, match it. In the header, write **Plantilla:** `SVG code (icon)` · **Modelo:** text model
(ChatGPT or any LLM), and omit size and quality, which do not apply.

How the image-model route works shapes how to write for it: the model first renders an internal reference image, and
`quality` controls that reference, then vector content is drawn from it. So the prompt is an image
prompt for a picture that must survive vectorization:

- Describe a **graphic, not a photograph**: flat color, clean geometric shapes, a strong silhouette,
  balanced negative space, minimal strokes. Photographic language — lens, depth of field, film
  grain, skin texture — produces a reference that vectorizes into noise.
- OpenAI's own SVG example reuses the Field & Flour logo prompt (see Worked examples) and keeps its
  key line: *"Favor simplicity over detail so it reads clearly at small and large sizes."*
- Keep the palette small and say so — a few named flat colors, "no gradients unless essential".
  Fewer colors mean fewer paths and an asset the user can recolor by editing the file.
- Text follows the normal text rules: quoted verbatim, exactly once. If small lettering comes out
  wrong, raise `quality`, since that is what sharpens the reference.
- Right fits: logos, icons, stickers, simple flat illustrations, pictograms. Wrong fits: anything
  whose value lies in texture, lighting or realism. When the user asks for an SVG of a photograph,
  say plainly it will be simplified into a flat graphic.
- `size` and `quality` act on the reference image, not on the vector, which scales freely. Pick the
  size for the composition — wide for a horizontal lockup, square for a stacked mark or an icon —
  and the rule that print work justifies `xhigh`/`max` does not apply: raise quality only when
  lettering or fine shapes come out wrong. What `size` sets in the SVG file itself (the `viewBox`)
  is not documented.
- For a standalone logo, the Brand identity package template's advice to request applied
  touchpoints and a pure white background does not apply: one SVG is one mark on transparency, and
  a mockup scene vectorizes badly. Keep its other pitfalls (subtract elements, check legibility at
  small size).

## Animated GIF output

2.6 only: `output_format="gif"` on generations or edits. Two extra parameters, which apply only to
GIFs:

- `frame_count` — 2 to 16, default 9.
- `frame_duration_ms` — 20 to 2000, rounded to 10 ms, default 200. Loop length is
  `frame_count × frame_duration_ms`: the defaults give a 1.8-second loop.

`size` describes **one frame**, in multiples of 16. All frames share one total pixel budget (the
8,294,400-pixel ceiling of a single image), so frame count and frame size trade against each other:
OpenAI's example uses `512x512` with 9 frames, and 9 frames at `960x960` already use the whole
budget. Keep frames modest — around 512 to 768 px on the long edge — and cut `frame_count` or size
when the request is rejected. The 655,360-pixel minimum for still images does not apply per frame.
No streaming, no partial images, no custom compression. Transparency for GIF output is not
documented: ask for a plain, flat, uniform background that is easy to remove afterwards.

Timing follows the motion. The defaults (9 × 200 ms) suit a slow, deliberate action; fast cyclic
motion — wings, flicker, a spinning wheel — needs shorter frames, around 60 to 100 ms, and more of
them.

The prompt describes **motion**, not just a picture. OpenAI's rule: *"For a smooth loop, give the
subject a starting pose to return to and say whether the camera should move."* Build it as:

1. The subject and its starting pose, described as for a still image.
2. The action, as a short sequence of verbs: lifts, looks up, blinks, lowers.
3. The return: the action ends back in the starting pose.
4. The camera: "keep the camera still", or the one move it makes.
5. What must stay constant across frames: props in hand, outfit, background.
6. The loop: "make the motion loop smoothly."

OpenAI's example (Sunburst, `512x512`, `quality="high"`, 9 frames at 200 ms) ends with exactly that
tail: *"…slowly lifts it, looks up and blinks, then lowers it back to its starting pose. Keep the
lantern in its hands and the camera still. Make the motion loop smoothly."*

One small motion per GIF. Nine frames cannot carry a story; a wave, a blink, a flicker, steam rising
or a logo settling into place is the right scale. A continuous cycle can run underneath that one
action — wings beating throughout while the bird dips to a flower once — as long as the cycle is
described as constant texture and only the action has a start and a return. To animate a character the user already has,
send the still image to the edits endpoint with `output_format="gif"` and describe only the motion.
In ChatGPT, the same request is the prompt plus the uploaded image. There, the first line of the
prompt must say what it is — "Create an animated GIF that loops seamlessly: 16 frames, about 120
milliseconds per frame. Aspect ratio 2:3." — or ChatGPT may return a still. Frame count and pace
written this way are honoured: a real test on 2026-11-18 asked for 16 frames and got 16, with a
continuous loop (one observation). ChatGPT also lets the user adjust the speed and edit individual
frames after generating, which is the fallback when the pace comes out wrong. GIF creation is still
rolling out, so an account without it returns a still image.

## Iterating on a result

Refine with small, single-change follow-ups: "make the lighting warmer", "remove the extra tree".
Do not rewrite the whole prompt to fix one thing.

The working loop: *"Pass the previous output as the next edit input, request one change, and repeat
the details to preserve."*

References like "same style as before" work, but re-specify the critical details as soon as they
start to drift. *"Repeated edits can still change details you intended to preserve. Restate those
constraints and inspect each result."*

## Checking the output

Before using a result, run it against the requirements — OpenAI's own list:

- Is required text accurate and legible?
- Are diagram labels and relationships correct?
- Do identities, product shapes, labels, and reference details remain intact?
- Did the edit change only what you requested?
- If transparency is required, does the file contain a real alpha channel?

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
