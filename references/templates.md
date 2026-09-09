# Prompt Template Library

26 industrial templates distilled from ~390 real GPT Image 2 cases, plus the pitfall guide for
each — rescued from the source project's `docs/templates.md`, translated from Chinese, and adapted
away from China-specific platforms and aesthetics — plus two templates added later from unrelated
prompt-engineering exercises ("Real place, transformed", "Identity-preserving persona
transformation"), each covering a real use case none of the 26 addressed.

Each entry gives you a skeleton to fill and the failure modes that template actually hits. The
skeleton is scaffolding for *your* thinking, not text to hand the user verbatim — fill it, then
rewrite it as flowing prose in the scene → subject → details → constraints order that
`gpt-image.md` prescribes.

**A note on camera parameters.** The source guide claims the model "eats up" precise specs like
`f/1.4` and `50mm`. OpenAI's own guide says camera specs are interpreted loosely and should set an
overall look, not a technical instruction. Trust OpenAI: use them for feel, never assume the model
honours them literally.

## Contents

| Category | Templates |
|---|---|
| [UI & interfaces](#ui--interfaces) | App/web screen · Social post screenshot · Livestream overlay |
| [Charts & infographics](#charts--infographics) | Infographic engine · Scientific scale diagram |
| [Posters & typography](#posters--typography) | Poster layout · Sports campaign · Conceptual typography · Double exposure portrait · Premium science poster |
| [Products & e-commerce](#products--e-commerce) | Product hero shot · Personalized recommendation report |
| [Brand & identity](#brand--identity) | Brand identity package · Touchpoint board · Brand-enveloped product ad · Brand persona comic |
| [Architecture & spaces](#architecture--spaces) | Space render |
| [Photography & realism](#photography--realism) | Realistic photography · Candid street moment |
| [Illustration & art](#illustration--art) | Illustration style |
| [Characters & people](#characters--people) | Character design sheet · 3D collectible toy · Identity-preserving persona transformation |
| [Scenes & storytelling](#scenes--storytelling) | Narrative scene · Real place, transformed |
| [Historical & period](#historical--period) | Period piece |
| [Documents & publishing](#documents--publishing) | Publication layout |
| [Other](#other) | Concept product breakdown |

Cross-cutting pitfalls that apply everywhere are collected in [Universal pitfalls](#universal-pitfalls).

---

## UI & interfaces

### App/web screen

```
Generate a [platform: iOS / Android / web] interface for [product type].
Core functions on screen: [A], [B], [C].
Visual style: [minimal / technical / skeuomorphic], primary [color], accent [color].
Layout: [top nav / two-column / card feed], clear hierarchy, generous whitespace.
Output: high-fidelity UI screenshot, all text legible, aspect ratio [9:16 / 16:9].
```

**Pitfalls**

- Vague instructions produce intern-grade layout. Always pin platform + ratio + layout together.
- Force text lock: state that all label text must be exactly as written and legible. Otherwise you
  get garbled buttons and nonsense glyphs.
- Non-phone screens have fixed ratios — car dashboards, smart-home panels, TV UI often need 21:9 or
  16:9. Put the ratio at the very top of the prompt or the model defaults to 9:16 phone.

### Social post screenshot

```
Generate a [Instagram / X / TikTok / LinkedIn / WhatsApp] content screenshot, [dark / light] mode.
Aspect ratio [9:16 / 4:5 / 1:1], phone screenshot look.
Account: [avatar description / handle / verification badge].
Body: "[exact post text]".
Engagement: [likes / comments / shares / saves counts].
Chrome — top: [status bar / nav bar / search]. Bottom: [action bar / tab bar / input field].
Extras: [floating overlay / product card / story ring].
Constraint: text must render exactly as specified, no gibberish, no placeholder text, ratio fixed.
```

**Pitfalls**

- Each platform has signature furniture: X has the blue check and quote-tweet block; TikTok has the
  spinning disc and like burst; Instagram has the story ring and double-column grid; LinkedIn has
  the reaction row. Name the platform explicitly or the model blends them into something that
  exists nowhere.
- Engagement numbers are text. They get hallucinated like any other text — quote them.

### Livestream overlay

```
Generate a [Twitch / Instagram Live / TikTok Live / YouTube Live] stream screenshot.
Host: [description], posture [seated / standing / gesturing], wearing [outfit].
Background: [set description], lighting [warm / cool / mixed].
Overlay: top — avatar + follow button + viewer count. Lower left — chat list ([N] messages,
sample content). Right or center — product card / gift animation / progress bar.
Bottom — input field + icons (share / like / gift / cart).
Style: [realistic stream capture / high-fidelity UI / dark / pastel], ratio 9:16.
Constraint: text legible, chat plausible, UI must not cover the host's face.
```

**Pitfalls**

- Lock the stream type first. A shopping stream and a talent stream have very different layouts —
  shopping puts a product list top-right, talent weights the chat interaction.

---

## Charts & infographics

### Infographic engine

```
Generate an infographic on [topic — specific, never broad: "daily health management for seniors",
not "health"], for [audience: age, profession, interest].
Structure: title area + [3-5] modules, each with icon, short title, and a 1-2 sentence note.
Show the relationship between modules with arrows, color coding, or connectors.
Chart type: [flowchart / comparison / relationship map / timeline].
Style: [professional report / popular science illustration / children's education],
primary [color], background [light / dark].
Output: clear hierarchy, high legibility.
```

**Pitfalls**

- Force the module count and chart type. This single constraint kills most visual chaos.
- Restraint in copy. Short phrases only. The model is not a typesetter — never push paragraphs into
  the frame.

### Scientific scale diagram

```
Generate a scientific scale-zoom infographic for [topic].
Structure: 6-8 circular or hexagonal frames arranged micro → macro.
Each frame carries: scale name, a 3-5 word insight, the unit or magnification, and a
high-detail render at that scale.
Connect frames with thin lines. Avoid repeating a level.
Title: "[TOPIC]: AT EVERY SCALE" or "ZOOM: THE WORLD OF [TOPIC]".
Style: scientific editorial infographic, precise macro light, clean hierarchy, short readable text.
Constraints: no generic magnifying-glass icons, no identically sized frames, no long body copy.
```

---

## Posters & typography

### Poster layout

```
Design a [event / product / film] poster on the theme of [theme].
Hero visual: [subject]. Headline: "[exact headline]". Subhead: "[exact subhead]".
Layout: [centered / left-aligned / diagonal]. Style: [retro / futuristic / minimal].
Color: [primary + secondary]. Mood: [emotional keywords].
Output: high-resolution poster suitable for social distribution.
```

**Pitfalls**

- Do not be lazy about the hero visual. "Make a poster" without naming what is actually depicted
  yields nothing usable.
- Hard-code the copy. Both headline and subhead must be written out, or the model invents
  additional text nobody asked for.

### Sports campaign

```
Design a commercial campaign poster for [sport / fitness category].
Subject: [athlete / model / product], pose [seated / sprinting / swinging / power move].
Hero prop: [racket / dumbbell / shoe / jersey], at exaggerated scale or on a diagonal, acting as
the visual anchor.
Layout: [single hero visual / triptych / data-overlay poster].
Headline: "[exact]". Support copy: "[short line / stat / slogan]".
Style: premium sportswear advertising, dramatic light, reflective floor, clean composition,
brand palette [primary + accent].
Constraints: subject clear, text legible, unified tone, no messy collage, no wrong equipment.
Output: 1:1 or 4:5 for social.
```

**Pitfalls**

- Sports campaigns collapse into cluttered collage faster than any other poster type. Lock the
  layout archetype before writing the subject.
- Props must be structural. Specify angle, scale and position or the model demotes the racket or
  shoe to background decoration.

### Conceptual typography

This one is used near-verbatim; it is already tuned. Substitute the title and go.

```
Create ONE finished premium conceptual typography poster for the exact title: "[TITLE]"

Single poster only. No moodboard, grid, presentation board, mockup, captions, prompt text,
process sheet, or sample labels.

The title must be the dominant visual structure: huge, readable, powerful, spelled exactly. Do not
translate, shorten, replace, or misspell it. Do not add other large readable text.

Silently interpret the title's meaning, mood, cultural aura, symbolic associations, psychological
tension, and visual rhythm. Turn that into one strong visual metaphor.

Typography is the hero. Design custom-looking letterforms whose weight, width, contrast, spacing,
rhythm, distortion, negative space, edge quality, and ink texture express the temperament of the
title. The type should feel intentionally designed, not like a default font.

If the title refers to a widely known person, make a large editorial portrait a major visual
presence, occupying roughly 40-70% of the composition, interacting with the typography:
overlapping the letters, emerging from them, framed by them, casting shadows on them, breaking
through them, or partially hidden behind them.

For abstract titles, use a figure, landscape, object or atmospheric setting only when it
strengthens the meaning. It must interact with the typography, not decorate it.

Use a restrained 4-6 color system: dominant background, primary typography color, figure tone,
emotional accent, muted support, subtle paper/ink texture.

Composition: high-end editorial poster, museum-quality graphic design, dramatic scale, strong
hierarchy, few elements, intelligent whitespace, bold flat color areas, sharp cropping,
silkscreen/lithograph/risograph grain, paper fibers, subtle ink imperfections.

Avoid generic word art, glossy 3D lettering, random icons, stock-photo realism, cluttered collage,
excessive grunge, tourist clichés, official logos, copied slogans, unrelated text, and misspelled
typography.
```

**Pitfalls**

- Lock the title first and demand exact spelling as the hero. Otherwise you get a beautiful,
  illegible type experiment.
- The image and the type must interact — embedded, occluded, passing through, supporting. Placed
  side by side, it reads as stock decoration.
- Ban the moodboard. State "single poster only" or the model returns a multi-option presentation
  board or a process sheet.

### Double exposure portrait

```
Generate a double-exposure portrait poster of [person / character / founder / athlete].
Format: 9:16 vertical, cinematic poster composition.
Upper zone: enlarged head, facial contour or half-body silhouette — the strongest recognition
anchor.
Lower zone: the same person full or half body, posture [standing / in action / meeting the lens].
Inside the silhouette: blend [key scene], [symbolic object], [narrative fragment],
[environmental texture] into a double-exposure narrative.
Visual linkage: use mist, ink diffusion, feathered edges, negative space and soft tonal transitions
to connect the upper silhouette, the interior collage and the lower subject into one top-to-bottom
visual line.
Style: [ink-wash / analog film / editorial] aesthetic plus photographic realism — restrained,
premium, generous whitespace, layered but not cluttered.
Text: optional [title / name / short line], sparse and legible, like a poster caption rather than
an infographic label.
Constraints: no hard collage, no filled-in background, no cheap effects, no copying an existing
poster layout, silhouette and subject must not compete for focus.
```

### Premium science poster

An Apple-keynote treatment for a single subject. Excellent for any specimen, artifact, product or
organism that deserves a hero presentation.

```
Generate a 9:16 premium science poster for [subject].

Overall direction: minimal, pure white, clean, modern — Apple product-launch visual language.
Background pure white or a very light grey-white gradient, with abundant whitespace.

Principles:
1. The subject is extremely enlarged and is the strongest visual center, occupying 50-70% of the frame.
2. Strong dimensionality, real texture, high-definition detail, soft studio light.
3. Few facts, precisely chosen. Never crowded.
4. No cards, rounded containers, complex patterns, aged-paper texture or decorative borders.
5. The bottom information strip uses exactly four minimal columns: thin-line icon + colored
   sub-heading + a 1-3 line note, separated by hairline vertical rules.
6. Typography reads like a keynote: enormous title, restrained subtitle, small clear body.

Structure — top left title block:
Headline: "[subject name]"
Subhead: "[one compelling positioning line]"
Thin rule
Secondary name: "[latin or technical name]"
Distribution/context: "[region or context]"

Center: the subject, real shadow so it stands in the frame like premium product photography.
Keep the white background; a minimal support such as a branch, rock, snow or sand is allowed.

Bottom: four columns, each "[feature title]" + "[short note]".
Centered grey summary line at the very bottom: "[one restrained, memorable closing line]".

Color: white / very light grey background, near-black headline, neutral grey body. Low-saturation
accents (warm brown, cool blue, teal, violet, orange) only on icons and sub-headings.

Quality: 2K, sharp subject, believable texture — fur, scales, shell, skin folds, feathers, markings.
Avoid deformation, wrong anatomy, blur, plastic or cartoon feel.

Forbidden: aged yellow paper, infographic grids, rounded cards, thick borders, large decorative
shapes, unrelated logos, extra small print, undersized subject, text covering the subject,
crowded bottom strip, children's-science or cheap exhibition-board styling.
```

**Pitfalls**

- Enlarge the subject aggressively — 50-70% of the frame. Undersized subjects are the main failure.
- Hold the line on restraint. Four columns, short notes. The moment you add a fifth idea it turns
  into a cluttered exhibition board.

---

## Products & e-commerce

### Product hero shot

```
Generate an e-commerce hero image for [product], selling points [A], [B].
Setting: [seamless studio backdrop / lifestyle scene]. Framing: [macro / three-quarter / full].
Material detail: [material keywords]. Lighting: [soft / side / rim light].
Added elements: [price badge / benefit icons / promo copy].
Output: ready for a product detail page.
```

**Pitfalls**

- Material and light are the whole game. Stack material words ("matte", "brushed aluminium",
  "condensation on glass") and lighting words ("rim light", "softbox overhead"). A product shot
  without specified light looks like a street-stall photo instantly.
- Do not paper the frame with promotional copy. One or two short lines maximum.

### Personalized recommendation report

A diagnosis → recommendation → try-on → context layout. Written here for cosmetics, but the
structure generalizes to any advisory report driven by an input photo.

```
Act as a professional [domain] consultant plus analysis system plus brand visual system.
Goal: from [user photo] and [brand], produce a vertical recommendation report combining analysis,
recommendation, simulated try-on and usage context.

Inputs: user image [photo]; brand [brand]; style preference [optional]; number of picks [3-5].

Analysis layer: determine [attribute 1], [attribute 2], [attribute 3]; output one summary
sentence of the form "better suited to [direction]".

Recommendation layer: select [3-5] differentiated options from [brand], each with name, category
tag, applied effect, and recommended occasion.

Brand visual layer: derive tone from [brand]; use a small amount of the brand accent color on
titles, hairlines, small icons and local detail only.

Layout: top left — input image + analysis. Top right — one-sentence conclusion. Center — a
matrix of [3-5] applied results on the same face, one per column. Bottom — a decisive
personal recommendation.

Requirements: premium editorial visual, structured information design, real skin texture, accurate
color, unified lighting, 9:16.
```

**Pitfalls**

- Analyze before rendering. Ask the model to reason about the inputs and only then map conclusions
  to picks. Jumping straight to a swatch grid produces arbitrary results.
- Brand as accent only. Tone lives in hairlines, accent color, type character and light — never a
  logo or a flood of brand color.
- Lock the identity in a comparison matrix: "same face, only [the varying attribute] changes", or
  the model draws a different person per column.

---

## Brand & identity

### Brand identity package

```
Act as creative director at a top brand agency. Deliver a complete identity system covering logo,
palette, typography, tone and applied touchpoints for [business].

Inputs: business name [name]; description [one line]; industry [industry]; audience [detail];
competitors [3-5]; brand personality [5 keywords]; feeling to trigger [trust / excitement /
luxury / intimacy / power]; identities admired [3 references]; identities rejected [3].

Deliver:
1. Strategy: archetype, core promise, positioning, differentiation, single key word.
2. Logo concepts: 3-5 genuinely different directions, each with visual idea, shape language,
   symbolism, type direction, first-glance emotion and suitable touchpoints.
3. Color system: primary, secondary, accent, neutrals, HEX codes, psychological rationale,
   usage rules and forbidden combinations.
4. Type system: display, body, accent, size hierarchy, tracking, leading, free alternatives.
5. Touchpoints: business card, app icon, website home, social template, billboard or packaging.
6. Three brand rules that must never be broken.

Output: a structured brand book any designer, developer or AI tool can pick up in ten minutes.
```

**Pitfalls**

- Subtract. Define the keywords before asking for visuals. "A fire-breathing dragon coiled around
  a pillar with lightning" is not a logo, it is an illustration.
- Force a pure white background so the mark can be cut out afterwards.
- Strategy precedes the mark. Without audience, competitors and emotional target, a logo is just a
  pretty shape with no argument for why it fits this brand.
- Always request applied touchpoints. That is how you discover the mark is illegible at small size
  or breaks in horizontal lockup.
- A brand book needs prohibitions, not just permissions.

### Touchpoint board

```
Generate a premium brand touchpoint board for [brand] — a full applied system, not a single poster.
Positioning: [industry / lifestyle / category].
Core character: [keyword 1], [keyword 2], [keyword 3].
Hero scene: [core product / service / experience] on [surface / setting], lit with [light],
shot [lens].
The board must include: hero product shot; packaging, bag, cup, label, sticker or seal; menu card,
price list or small typographic sample; a lifestyle or in-use fragment; and the palette, type and
graphic language applied consistently across all of them.
Design language: [modern minimal / Japanese negative space / luxury editorial / tech brand],
primary [color], secondary [color], abundant whitespace, fine materials, real shadows, small text
legible.
Composition: like a top agency proposal page — tidy but not rigid, hero dominant, supporting
material clearly subordinate, coherent and produceable.
Constraints: not just a logo; no cluttered collage; no gibberish text; packaging, menu and stickers
must not diverge in style.
```

### Brand-enveloped product ad

A four-phase construction that keeps a visual world stable while the product inside it changes.

```
Inputs: [product image], [brand identity], [output format].

PHASE 1 / ANCHOR: describe [brand identity] in two lines — palette, materials, light, mood.
PHASE 2 / INJECT: place [product] inside that world. The product obeys the brand's character and
environmental language.
PHASE 3 / FORMAT: specify [hero image / square ad / vertical story / marketplace thumbnail].
PHASE 4 / SIGNATURE: add [brand elements] — grain, shadow, overlay texture, packaging motif,
graphic frame.

Goal: swapping the product under the same brand keeps the visual world identical, while the ad
still has one clear protagonist and commercial polish.
```

### Brand persona comic

```
From the uploaded [logo / brand visual], generate a 4:5 comic infographic titled
"What This Brand Feels Like".
Goal: turn the brand into a perceivable character and explain how it speaks, acts, sells, answers
competitors and handles criticism.
Rule: every color, garment, posture, tone and graphic element derives from the logo and the brand
keywords.
Hero: one personified brand character whose clothing, expression and stance embody [brand character].
Around it: 6-8 comic panels, each with a short title, an action, and a speech or thought bubble.
Modules: voice tone, energy level, social behavior, communication style, DO / DON'T.
Style: comic plus editorial infographic — expressive but premium, short punchy text, layered.
Constraints: no generic marketing words, no empty regions, do not draw the persona as a random
character.
```

---

## Architecture & spaces

### Space render

```
Generate a [space type] render, functioning as [use].
Style: [modern minimal / industrial / warm contemporary]. Materials: [wood / stone / metal / glass].
Structure: [open plan / zoned]. Circulation: [main path].
Light: [daylight / artificial scheme], time [day / dusk / night].
Output: photorealistic architectural render.
```

**Pitfalls**

- Perspective distortion is the classic failure. "Eye-level perspective" suppresses it.
- Warm interior light against cool exterior light (blue-grey outside, amber inside) is the cheat
  code for a premium-looking space.

---

## Photography & realism

### Realistic photography

```
Subject: [person / object / street scene], set in [location].
Photographic style: [35mm / 85mm], [shallow / deep depth of field], [documentary / cinematic].
Light: [natural / neon night / backlit]. Mood: [emotion].
Detail: [skin texture / material / grain].
Output: photorealistic image.
```

**Pitfalls**

- Add flaws. AI people are too perfect and read as mannequins. Ask for skin pores, freckles, fine
  lines, subtle film grain — realism arrives immediately.
- Write imperfection concretely. "Rough brick, scattered ice, natural shadow, slight handheld tilt"
  outperforms the word "realistic" by a wide margin.

### Candid street moment

```
Generate a vertical phone documentary photo of [incident / everyday moment] at [street location].
Subject: [object / human action / trace on the scene], in a genuine material state —
[liquid spreading / ice scattered / paper creased / dust].
Environment: [ground material / wall / street elements], keeping natural mess and lived-in traces.
Light: [harsh noon / overcast diffuse / streetlight at night], shadows in a plausible direction,
optionally including [a person's shadow / sign shadow / tree shadow].
Lens: handheld phone viewpoint, slightly high or low angle, natural framing, like something caught
by chance.
Texture: raw unedited photo look, natural color, real texture, high detail.
Negative: no illustration, anime, CGI, studio light, over-cleanliness, over-composition, fake
liquid, floating objects, brand text, watermark or poster-design feel.
```

---

## Illustration & art

### Illustration style

```
Create a [subject] illustration, protagonist [character / subject].
Technique: [anime / watercolor / flat vector / painterly]. Line: [fine / bold].
Palette: [scheme]. Background: [simple / detailed scene].
Framing: [close / medium / wide], emphasizing [detail].
Output: high-quality illustration for a cover or social post.
```

**Pitfalls**

- Lock the brushwork. Without a stated technique ("impasto", "watercolor bleed") you get soulless
  default AI plastic.
- Be careful with master names. Naming an artist makes the model reproduce that artist's famous
  composition wholesale. Extract the trait instead — "swirling starry-night brushwork" rather than
  the name.

---

## Characters & people

### Character design sheet

```
Character sheet for [character]. Identity anchors: [face shape, eyes, brows, nose, hairstyle].
Outfit: [garment + material]. Proportions: [head-to-body ratio].
Panels: [N] poses, numbered, in a [rows x columns] grid, consistent scale.
Same character, same outfit, same proportions across every panel.
Style: [technique]. Background: neutral, non-competing.
```

**Pitfalls**

- Decompose the face. "A beautiful girl" means nothing to the model. "Almond eyes, high nose
  bridge, natural untamed brows" means something.
- Name the garment material — silk, technical windproof fabric — and the character gains dimension.
- Lock the grid. State panel count, numbering and per-cell structure or the steps compress into one
  cluttered diagram.
- Put character consistency *before* the action list. The longer the sequence, the more the face
  and clothes drift.

### 3D collectible toy

```
Turn [character / person from reference] into a premium collectible figure.
Preserve identity anchors from the reference: face shape, hairstyle, signature outfit details.
Proportions: [chibi / realistic], head-to-body [ratio].
Material: [matte vinyl / glossy resin / flocked], base [description], packaging [blister / window box].
Lighting: soft studio, real shadow, collectible display scale.
Constraints: keep packaging text minimal and accurate; no generic doll body without identity detail.
```

**Pitfalls**

- Preserve identity anchors before describing the stylization. Lock face, hair and costume markers
  first, then head ratio and material, or it becomes a different person.

### Identity-preserving persona transformation

A real, specific person from a reference photo, restyled into a theme — a decade, a profession, a
fictional role — while staying recognizable as *that* person, not a generic member of the theme.
This is a distinct problem from the two templates above: `Character design sheet` designs a new
character from scratch, `3D collectible toy` stylizes toward a material and a proportion system.
Here the reference photo is a hard constraint the whole rest of the prompt has to work around, and
the single most common failure is a prompt that describes the transformation in far more depth than
it protects the identity — producing something that reads as "a convincing [theme] person who
resembles the subject" rather than "this exact subject as a [theme] person".

```
Using the attached reference photo, transform the same person into [theme/role].

Identity — the highest-priority constraint: the subject must remain unmistakably the same real
person from the reference. Preserve his/her facial geometry and the relationship between the
features — [2-3 anchors specific to this face, not a checklist], natural skin tone, [facial hair/
distinguishing marks], apparent age, and body proportions ([build], shoulder width, torso-to-leg
relationship — not height, which a single photo cannot establish). The transformation changes
styling, clothing, hair and environment, not the person underneath.

Transformation: reimagine this person as [role/theme], described by function or era rather than by
a specific well-known archetype — [see the pitfall on borrowed archetypes below].

Wardrobe: [fully specified outfit, one option per garment — no alternatives].

Hair and eyewear: transform the *existing* [hairstyle/eyewear] into [period/theme version] while
preserving the underlying hairline, forehead proportions and head shape — do not replace them with
a generic version of the trope. [If eyewear must darken]: keep the eye area partially visible rather
than fully obscured.

Pose and framing: [specific pose], [specific framing — e.g. three-quarter to full body].

Environment/style: [scene]. [Photographic treatment].

Constraints: preserve the subject's identity above every stylistic choice — do not reshape the
face, change apparent age, alter body proportions, or produce a different person. One person only.
No duplicated text. [Plus any exclusions specific to the theme.]
```

**Pitfalls**

- State identity preservation as the first, highest-priority instruction — not one item inside a
  long list of wardrobe and scene detail. Position in the prompt reads as priority to the model.
  Burying "same person" under twenty lines of costume description is asking for exactly what it
  produces: a costume that succeeded and an identity that partially didn't.
- Say what changes, not just what doesn't. "The transformation changes styling and environment, not
  the person underneath" does more than a list of preserved features, because it tells the model
  *where* to spend its freedom instead of only where not to.
- Hair and eyewear are identity anchors, not accessories, whenever the theme wants to change them
  dramatically. "Give him voluminous 1980s hair" replaces the skull's silhouette wholesale; "transform
  his existing hairstyle into a period version, preserving the hairline and head shape" keeps the
  transformation from overwriting the anchor it's built on. The same logic applies to sunglasses or
  anything else that would otherwise cover a feature the reference photo needs to keep legible.
- Avoid naming a specific, iconic archetype as the transformation's descriptor — "hair-metal fashion
  icon", "film noir detective" — when the reference photo's identity has to win. A well-known
  archetype carries its own strong, prototypical visual template, and it competes with the reference
  photo for control of the face and body. Describe the function or era instead ("a television host
  on a 1980s music program") and let the wardrobe section carry the aesthetic.
- Resolve every alternative. "A or B" phrased as normal prose is the same failure as leaving a
  `[bracket]` placeholder — an unresolved decision the model has to make for you. Pick one option per
  garment, one color, one pose; see "No unresolved alternatives" in `gpt-image.md`.
- Height cannot be verified from a single photo and isn't something the model can preserve
  literally — use shoulder width, build and proportion instead.

---

## Scenes & storytelling

### Narrative scene

```
Scene: [who] at [where], [when]. Event: [what is happening right now].
Conflict/tension: [what is at stake]. Emotion: [feeling on the faces].
Camera: [low angle / Dutch angle / over the shoulder], [framing].
Style: [technique]. Details that support the story: [elements].
```

**Pitfalls**

- Insist on a verb. Narrative images collapse into landscape postcards without an event —
  "is collapsing", "has just lit the torch", "is turning at the sound".
- Use camera grammar for drama. Low angle for power, Dutch angle for unease.
- No generic fantasy backgrounds; keep narrative cues visible inside the frame.

### Real place, transformed

A named, real location shown after a stated change — decades of abandonment, a disaster, a climate
shift, an alternate timeline. The defining difficulty is different from a generic narrative scene:
the harder you push the transformation, the more the model needs to be told what makes this place
*this place*, or it drifts into "generic city" the moment the recognizable surface is altered.

```
[Real, named place], approximately [duration] after [stated event/change].

[If the scene layers three or more competing subjects — place, nature, people, wildlife]: the
composition should read in this order: [1. the place itself, 2. the transformation/reclamation,
3. the figures, 4. incidental wildlife] — ranked so the model knows what should dominate and what
should merely be present.

Identity anchors — the place must remain recognizable through: [3-5 unmistakable features:
signature geometry, skyline silhouette, named structures, characteristic materials or proportions].
State explicitly: must remain immediately recognizable as [place] despite the transformation.

Transformation: [what changed and why], reflecting [duration] of [process — decay, growth, climate,
disuse], not a fresh event. Damage/change looks cumulative and old: [concrete old-not-fresh markers
— biological growth over water stains, weathering rather than fresh breakage, sediment accumulated
in low areas].

Regional consistency: [flora/fauna/materials/climate details] appropriate to [the real region] —
not generic or exotic dressing. Give each named element its own placement or action rather than
listing them flat, so the scene reads as an ecosystem, not an inventory: not "deer, a fox, herons"
but "deer grazing in the middle distance", "a fox moving through debris farther back", "herons near
a shallow pool".

[If the transformation implies a persistent state that would not obviously last on its own —
permanent flooding, arrested climate, a structure that should have collapsed but hasn't]: name the
mechanism in one clause, not just the state — [why it lasted, e.g. failed drainage, subsidence,
blocked outflow].

[If figures appear]: exactly [number] figures, incidental to the scene, occupying a small portion
of the frame so the environment reads first. [Body language / activity], not posed for the camera.
Describe their wear through clothing, grime and posture rather than the body itself, unless genuine
emaciation is specifically the point — see the pitfall below.

The scene must feel observed rather than designed; nothing should appear deliberately arranged for
dramatic effect.

Style: [photographic/technique]. Avoid teal-and-orange grading, excessive HDR, lens flares, glowing
highlights, and any game-engine or matte-painting sheen.

[If the transformation involves destruction rather than only decay/growth]: the destruction is
extensive but visually mundane and believable — avoid spectacular collapse, giant rubble piles,
explosions, fire, or exaggerated apocalyptic imagery.

Constraints: [...]. [If a figure count was stated]: exactly [number] people, no additional human
figures anywhere in the frame, no crowd.
```

**Pitfalls**

- Anchors scale with alteration, not against it. The more damage, overgrowth or time you ask for,
  the *more* identity anchors the prompt needs, not fewer — a heavily ruined place with no anchors
  reads as any ruined place.
- Verify each anchor is actually true to the named place, not borrowed from a different, similarly
  famous landmark. A wrong anchor is worse than no anchor: it pulls the render toward *that* place's
  real appearance instead of the one you asked for. ("Flatiron-style" tower as a Times Square anchor
  risks summoning the actual Flatiron Building, which is a different building in a different
  neighborhood — say what is structurally true of the named place, not the nearest famous analogue.)
- Match environmental dressing to the real region. Vegetation, weather and light that would suit a
  different climate break credibility even when individually beautiful.
- Give each environmental detail a placement or action, not a flat list — this applies best to
  static texture (vegetation, materials, weathering), which can be numerous because it reads as
  ambient backdrop, not competing subjects. Live animals are a different case: each one is a
  discrete subject the model has to render individually and place believably in the same continuous
  frame, so they compete for attention in a way plant species don't. Cap explicitly rendered animals
  at 2-3; let the rest of the wildlife be implied through evidence — tracks, feathers, a disturbed
  patch of reeds, a distant call — rather than another fully rendered creature.
- State the duration into the damage itself. "Twenty years later" with no further instruction
  defaults to depicting the event, not its aftermath — describe cumulative, weathered change
  explicitly rather than trusting the number alone.
- If the persistent state itself is physically surprising — water that never drained, a structure
  standing when it should have fallen — name why it lasted, not only that it did. See "Fighting the
  synthetic look" in `gpt-image.md`; describing the result without its cause reads as an
  inconsistency the model has to paper over, not a fact it can render. But name it once, as a
  settled fact — don't also describe the same element as still in the middle of changing. "Stayed
  flooded because drainage failed" and "floodwater that is still slowly receding" are two different
  claims about the same water; pick the one that's true now.
- Describe only what the camera would actually see. Naming the surrounding boroughs, districts or
  administrative geography of a real place is encyclopedia prose, not a visual instruction — it
  competes with the identity anchors for the model's attention without giving it anything to render.
- Keep the scale of any destruction restrained and mundane unless spectacle is genuinely the goal —
  see "Keep destruction mundane, not spectacular" in `gpt-image.md`. Old damage and giant collapsed
  rubble are not the same thing; a scene can nail the former and still look like a disaster-movie
  poster without this constraint.
- If people appear, make them incidental discoveries, not the photo's subject. Small in the frame,
  mid-activity, unposed — see "Fighting the synthetic look" in `gpt-image.md`.
- A stated headcount needs to be repeated as an exclusion in the constraints block, not only stated
  once where the figures are introduced — "exactly two, no additional people, no crowd" close to the
  other exclusions is what actually holds the count.
- Reach for "gaunt" and other starvation markers only when genuine famine is the intent. For a
  general survivor scene, wear communicated through ragged clothing, grime, posture and caution
  reads as hardened rather than starved — the words you want most of the time are "weathered" and
  "worn", not "gaunt" or "emaciated".

---

## Historical & period

### Period piece

```
Generate a [period / dynasty / decade] scene depicting [subject].
Period markers: [clothing system], [architecture], [objects], [materials].
Format: [scroll / album page / poster / photographic].
Cultural mood: [restraint / opulence / austerity].
Constraint: no modern elements. No anachronistic props, garments, or technology.
```

**Pitfalls**

- Name the exact period. Left vague, the model mixes eras — a kimono and a Qing fan inside a Tang
  palace, or a Victorian dress at a Roman banquet.
- Always add the explicit "no modern elements" line, or a Starbucks cup appears in the hands of
  your period figure.

---

## Documents & publishing

### Publication layout

```
Generate a [white paper / manual / encyclopedic plate / report page] layout.
Page: [size], [N] columns, margins [description].
Structure: [title block / table of contents / figure system / caption system].
Typographic hierarchy: display [size/weight], body [size], captions [size].
Headline text, exact: "[headline]".
Body copy: simulated text blocks, not real sentences.
Style: [publisher / technical manual / editorial].
Constraint: no tiny dense text; charts and captions aligned to the page grid.
```

**Pitfalls**

- Structure beats style. Column count and margins matter far more than adjectives.
- Give up on full body copy. Do not expect a typo-free page of running text. Ask for "simulated text
  blocks" for the body and hard-code only the headline and key labels. This is the single most
  useful trick in this entire library.
- For a multi-page system, request an additional spread overview to verify that cover, interior,
  case page and contact page hold together.

---

## Other

### Concept product breakdown

```
Task and purpose: [state what this image is for before any visual detail].
Artifact: [exploded diagram / R&D board / technical breakdown] of [object].
Components: [list], with [callout style] labels and [material logic].
Presentation format: [board / plate / single render].
Constraint: short labels, visible component relationships, controlled technical style.
```

**Pitfalls**

- State the goal and the use first. Giving the model global context up front, before visual detail,
  measurably improves coherence on unusual tasks.
- For exploratory work, ask for a main version plus one alternative in the same generation so you
  can pick.

---

## Universal pitfalls

These recur across every category and are worth checking before any prompt ships.

- **Never leave text unquoted.** Every string that must appear goes in quotes, with "exactly once,
  verbatim, no extra characters".
- **Never leave the ratio implicit.** The model defaults to shapes that may not match the
  destination.
- **Name the medium.** Photo, 3D render, watercolor, vector, ink. Without it you get the default
  house style.
- **Exclusions are cheap and effective.** No watermark, no extra text, no logos, no added elements.
- **One change at a time when iterating.** Rewriting the whole prompt to fix one detail loses
  everything that was already right.
