# Editing an Existing Image

Prompts that change an image the user already has, rather than creating one from nothing. The
deliverable is the same — prompt text the user pastes alongside their uploaded image — but the
craft is different enough that the generation templates mislead here.

Why it is different: in generation, everything you don't say is a blank the model fills however it
likes, so the work is *describing*. In editing, everything you don't say is something that already
exists and might get destroyed, so the work is *protecting*. A generation prompt that forgets to
mention the background produces a background. An edit prompt that forgets to mention the background
loses the one the user had.

Source: OpenAI's [Image prompting guide](https://developers.openai.com/api/docs/guides/image-prompting).

## Contents

- [The spine of every edit prompt](#the-spine-of-every-edit-prompt)
- [Pointing with markup](#pointing-with-markup)
- [Local edits](#local-edits) — Remove an object · Swap an object · Change one condition
- [Identity-preserving edits](#identity-preserving-edits) — Change clothing · Insert a person into a scene
- [Working with several references](#working-with-several-references) — Combine references · Transfer a visual style
- [Transformations](#transformations) — Translate in place · Sketch to photorealism · Transparent cutout
- [Series consistency](#series-consistency) — Refine across turns · Keep a character consistent
- [Universal edit pitfalls](#universal-edit-pitfalls)

## The spine of every edit prompt

Four moves, in this order. Everything below is a variation on them.

**1. Name the change, narrowly.** "Change only X." One change per request. Two changes in one
prompt make it impossible to tell which instruction caused a bad result, and the model is more
likely to reinterpret the whole image when it thinks it has broad licence.

**2. List what must survive.** This is the part people skip and it is the part doing the work.
OpenAI's own phrasing for edits is to *"list the details to preserve, such as identity, geometry,
layout, lighting, or labels."* For precise local edits, extend the list to the things nobody thinks
to name until they are gone: *"saturation, contrast, arrows, camera angle, and surrounding
objects."*

**3. Give each reference a role.** With more than one input image, say which is which and what each
one contributes: *"Identify each input by number and purpose: subject, style, clothing, or
background. Explain how the inputs should combine and which elements should move where."* Leaving
the model to infer which photo is the subject and which is the style reference is the easiest
avoidable failure in this whole file.

**4. Iterate one change at a time.** Feed the previous output back in as the next input, request a
single change, and repeat the preserve list. Drift accumulates: *"Repeated edits can still change
details you intended to preserve. Restate those constraints and inspect each result."* And compare
results before piling on more instructions — an extra sentence added to a prompt that was already
working is how a good result gets talked out of existence.

**When prompting is the wrong tool.** If a region has to come back *pixel-identical*, no prompt
guarantees that. OpenAI says it plainly: composite the approved edit back into the original image
instead. Tell the user this rather than selling them a prompt that cannot deliver it.

**Settings that protect an edit (2.6).** For an edit that changes one detail, recommend
`complexity: low` — OpenAI's advice is to *"start with `low` when you want to change one detail"*,
and the default `high` plans more and can rework things the preserve list was meant to keep. Omit
`input_fidelity` on edits, per OpenAI. Mention these in the header line when the user works through
the API; in ChatGPT they do not exist — recommend the fast reasoning level for a one-detail edit
instead (see "In ChatGPT: the reasoning level" in `gpt-image.md`), and let the preserve list carry
the rest.

## Pointing with markup

Describing *where* in words — "the vase on the second shelf, left of the clock" — is where local
edits most often miss. Markup replaces the description with a drawing: the user circles or boxes
the target on a transparent overlay, and the prompt refers to the mark. Available on 2.6 and 2.5
through `/v1/images/edits` (the `markup` field); in ChatGPT, the same move is drawing on the image
before sending it.

Reach for it when the target is hard to name uniquely — one of several similar objects, a patch of
background, a spot with no landmark — or when there are several targets at once.

How to write the prompt:

- **Name each mark by colour and shape, exactly as drawn**, and give it one instruction. OpenAI's
  example: *"Recolor the striped vase inside the red circle to solid cobalt blue ceramic."* With
  several marks, one line per mark: "Red circle: … Blue box: … Green arrow: …"
- **Ask for the marks to be removed** from the result: "Remove the red circle from the finished
  photo." Otherwise the annotation can survive into the output.
- **The spine still applies.** The mark says where; the preserve list still says what must not move.
- **Several marks are the one exception to "one change per request".** Each mark scopes its own
  change, so the model is not left guessing which instruction to apply where, and one pass avoids
  the drift that two full re-renders would add. Keep it to one instruction per mark and nothing
  outside the marks. If one mark lands and another fails, take the good output as the next input
  and redo the failed mark alone.

Mechanics to pass on to an API user: the overlay is a transparent PNG with the **same pixel
dimensions** as the first input image, drawn on a separate layer, never flattened onto the photo.
It applies only to the first image; any further images act as references. Do not send `mask` in
the same request. A mask and markup do the same job by different means, and markup is the easier
one to make — a mask needs an alpha channel and is, in OpenAI's words, guidance rather than a
pixel-exact boundary anyway.

---

## Local edits

### Remove an object

```
Remove [the specific object] from [where it is]. Do not change anything else.

Preserve: the person's pose and expression, the lighting and shadow direction, the camera angle,
the framing, the background, and the colour grading.
```

**Pitfalls**

- Name the object and its location, not just the object. "Remove the flower" is ambiguous in a
  photo with a vase and a lapel; "remove the flower from the man's hand" is not.
- "Do not change anything else" is short and does real work. Keep it even when the preserve list
  already says more.
- The area left behind has to be invented. Expect the reconstruction to be plausible rather than
  true, and check it against what was logically behind the object.

### Swap an object

The furniture-replacement case, and anything shaped like it: one element changes, the room stays.

```
In this [room / scene] photo, replace ONLY [the specific object] with [the specific replacement].
Preserve camera angle, lighting, floor and contact shadows, and surrounding objects.
Keep all other aspects of the image unchanged.
Photorealistic contact shadows and [material] texture.
```

**Pitfalls**

- Ask explicitly for contact shadows. A swapped object that doesn't touch the floor convincingly is
  the giveaway that separates a real photo from a paste-up.
- Name the replacement's material, not just its category — "chairs made of wood" gives the model a
  surface to render; "different chairs" gives it nothing.
- Say ONLY, capitalised or not, and name what stays. Object swaps are where the model is most
  tempted to redesign the whole space.

### Change one condition

Weather, time of day, season — the whole scene stays and one environmental variable moves. These
prompts are famously short, and that is correct.

```
Make it look like [a winter evening with snowfall].
```

**Pitfalls**

- Resist padding it. This is the one place where a single sentence outperforms a specification,
  because the model already has the scene in front of it and every extra instruction is a new
  chance to drift.
- If something specific must survive the change — a legible sign, a product label — that is the
  one thing worth adding.

---

## Identity-preserving edits

### Change clothing

The pattern generalizes to any edit where a person, product or object must stay recognizable while
one attribute changes.

```
Edit the image to dress [the person] using the provided clothing images.

Do not change [his/her] face, facial features, skin tone, body shape, pose, or identity in any
way. Preserve [his/her] exact likeness, expression, hairstyle, and proportions.

Replace only the clothing, fitting the garments naturally to the existing pose and body geometry
with realistic fabric behaviour. Match lighting, shadows, and colour temperature to the original
photo so the outfit integrates photorealistically, without looking pasted on.

Do not change the background, camera angle, framing, or image quality, and do not add accessories,
text, logos, or watermarks.
```

**Pitfalls**

- The preserve list here is long on purpose, and it is not padding: each item names something that
  drifts in practice. Identity, expression and proportions go first because they are what makes the
  result *that person*.
- "Without looking pasted on" plus the lighting-match instruction are what separate a garment
  composited onto a body from a photograph of someone wearing it.
- Bar accessories explicitly. Given clothing references, the model tends to add jewellery, bags and
  hats nobody asked for.

### Insert a person into a scene

```
Generate a highly realistic scene where this person is [doing the action] in [the setting].
The image should look like a real photograph someone could have taken, not an overly enhanced or
cinematic movie-poster image.

[Their] position in frame, gaze and interaction: [centred but looking away from the camera,
running, clearly afraid but focused on escaping].
Wardrobe and condition: [outdoorsy camping attire, dirt on the face, tears in the clothing].
Setting detail: [Yosemite National Park at dusk, believable natural detail, natural lighting].

Preserve [the person's] facial features, proportions and identity exactly as in the reference.

Everything should feel grounded, authentic and unstyled, as if captured in a real moment. Avoid
cinematic lighting, dramatic colour grading, or stylized composition.
```

**Pitfalls**

- Body framing, gaze and interaction with the scene do more than adjectives. *"Full body visible,
  feet included"*, *"looking down at the open book"*, *"hands naturally gripping the handlebars"* —
  concrete physical facts the model can place.
- The anti-cinematic clause matters more here than almost anywhere. Dropping a real person into a
  dramatic scenario pulls hard toward movie-poster treatment, which destroys the documentary
  credibility that makes the insert believable.
- See also "Fighting the synthetic look" in `gpt-image.md` — the observed-not-designed line and the
  rendering-cliché checklist both apply.

---

## Working with several references

### Combine references

```
Place [the element] from the second image into the setting of image 1, [where exactly — right next
to the woman]. Use the same style of lighting, composition and background as image 1.
Do not change anything else.
```

**Pitfalls**

- Number the inputs and say what each is for. This is the whole technique.
- Say where the moved element goes, in relation to something already in the destination image.
  "Into the scene" leaves placement to chance.
- Name which image wins on lighting and colour. Two photos shot under different light will not
  merge convincingly unless one is declared the reference.

### Transfer a visual style

```
Use the same style from the input image and generate [the new subject] on [the background].
```

**Pitfalls**

- Assign the reference a *specific* role — its palette, its texture, or its visual medium — rather
  than the vague "this style". Medium transfers most reliably; a colour palette transfers well; a
  particular artist's composition transfers badly.
- Describe the new subject separately and completely. The reference supplies the how, not the what.
- Short works here. The reference image is carrying most of the specification already.

---

## Transformations

### Translate in place

```
Translate the text in the [infographic] to [Spanish]. Do not change any other aspect of the image.
```

**Pitfalls**

- Check for words left in the original language, especially in small labels, legends and axis
  titles. Partial translation is the normal failure, not total failure.
- Re-verify the layout. Translated strings change length, and the model may resize or reflow to
  fit — which is a change to "any other aspect" even though it did not intend one.
- Dense diagrams deserve `quality="high"` for this even when the original was generated at medium.

### Sketch to photorealism

```
Turn this drawing into a photorealistic image.
Preserve the exact layout, proportions, and perspective.
Choose realistic materials and lighting consistent with the sketch intent.
Do not add new elements or text.
```

**Pitfalls**

- "Do not add new elements" is the load-bearing line. Given a rough drawing, the model's instinct
  is to enrich it — and every enrichment is a departure from what the sketch was specifying.
- Preserve perspective explicitly. A hand-drawn perspective is often slightly impossible, and the
  model will silently "correct" it into something that no longer matches the intent.
- Let materials and lighting be the model's contribution. That is the part a sketch genuinely
  lacks.

### Transparent cutout

```
Extract the product from the input image and isolate it on a fully transparent background.
Output: centred product, crisp silhouette, no halos or fringing.
Preserve product geometry and label legibility exactly.
Add only light polishing. Do not add a solid backdrop, checkerboard, scenery, or shadow.
Do not restyle the product; remove the background and preserve clean alpha transparency.
```

Requires `background="transparent"` and PNG or WebP output, with no `output_compression` for PNG.

**Pitfalls**

- **A drawn checkerboard is not transparency.** The model will sometimes paint the grey-and-white
  grid that image editors *display* for transparency. Verify the actual alpha channel — and check
  it at hair, glass, shadows and object edges, where it fails first.
- "Do not restyle the product" earns its place: asked to isolate something, the model often also
  cleans, straightens or relights it.
- For any follow-up edit on a transparent asset, repeat the transparency requirement. It does not
  carry over on its own.

---

## Series consistency

### Refine across turns

The working loop for getting from a good result to the right one.

```
Turn 1: [the full prompt, with exact text quoted]
Turn 2: [one short change — "make it look like a winter evening with snowfall"]
Turn 3: [one more]
```

Pass each output back in as the next input. Keep every follow-up narrow enough that you can tell
which change helped.

**Pitfalls**

- One change per turn. Two changes and a worse result tells you nothing about which to undo.
- Restate the constraints that matter on every turn, especially exact text. Text that rendered
  perfectly on turn one is not safe on turn three.
- Compare before adding. The temptation after a near-miss is to add instructions; often the better
  move is to re-run and look at the variation first.

### Keep a character consistent

For picture books, comic series, brand mascots — anything where the same character appears across
many images. Two stages.

**Establish**, as a generation with a plain background so the character reads clearly:

```
Create a [children's book illustration] introducing a main character.

Character: [appearance, outfit, proportions, expression, demeanour, signature prop].
Theme: [what the character does].
Style: [technique, palette, line quality, proportions suited to the format].

Constraints:
- Original character, no copyrighted characters
- No text
- No watermarks
- Plain [background] to clearly showcase the character
```

**Continue**, as an edit using that output as the input:

```
Continue the [story] using the same character.

Scene: [what is happening now, and the character's posture within it].

Character consistency:
- Same [outfit]
- Same facial features, proportions, and colour palette
- Same [personality read]

Style: [same technique, new environment and lighting].

Constraints:
- Do not redesign the character
- No text
- No watermarks
```

**Pitfalls**

- The establishing image is an investment, not a throwaway. A plain background and a clear,
  well-lit pose make it a usable reference for every later scene; a busy first illustration makes
  every continuation harder.
- Repeat the appearance constraints verbatim each time. Paraphrasing them is how a character drifts
  across ten pages.
- "Do not redesign the character" is worth stating even with the list above it. It names the failure
  directly.

---

## Universal edit pitfalls

- **Every edit prompt needs a preserve list.** If you wrote only what changes, you wrote half a
  prompt.
- **One change per request.** The debuggability is worth more than the round trip you saved.
- **Number your references and give each a job.** Subject, style, clothing, background.
- **Restate constraints on every iteration.** Drift is cumulative and silent.
- **Verify the specific thing you protected**, not just overall quality. The result can look good
  and still have changed the one detail you needed frozen.
- **Pixel-identical is not a prompting outcome.** Composite instead, and say so.
