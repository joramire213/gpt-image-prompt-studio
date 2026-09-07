# Worked Examples — Real Prompts That Produced Good Images

Seven prompts mined from the source project's corpus of 541 documented GPT Image 2 cases, each one
attributed to the practitioner who published it. They are here because a finished prompt teaches
more than a skeleton does: you can see how much detail is actually enough, where constraints go,
and how differently a poster and a style-lock are written.

Read them for **shape**, not content. Each is annotated with the technique it demonstrates.

## Contents

1. [Sectioned prose — typography travel poster](#1-sectioned-prose--typography-travel-poster)
2. [Use-case header + granular prose — cinematic portrait](#2-use-case-header--granular-prose--cinematic-portrait)
3. [Numbered panels — dense editorial infographic](#3-numbered-panels--dense-editorial-infographic)
4. [Compact prose — brand campaign poster](#4-compact-prose--brand-campaign-poster)
5. [JSON spec — product infographic](#5-json-spec--product-infographic)
6. [JSON style lock — illustration technique](#6-json-style-lock--illustration-technique)
7. [Dense realism paragraph — fake phone photo of a screen](#7-dense-realism-paragraph--fake-phone-photo-of-a-screen)

## Choosing a shape

The corpus shows four shapes in real use. Pick deliberately.

| Shape | Use it when | Examples |
|---|---|---|
| Flowing prose | Photographic and narrative work, where mood and continuity matter | 2, 4, 7 |
| Sectioned prose (ALL-CAPS headers) | Posters and complex compositions with several distinct zones | 1 |
| Numbered panels | Infographics, reports, dashboards — anything that is a grid of specified cells | 3 |
| JSON | Style locks and spec-heavy assets where every attribute must be pinned and reused across renders | 5, 6 |

OpenAI's guide confirms all four work. The failure is not picking one — an unstructured pile of
adjectives is what produces mush. JSON in particular is underrated for **reproducibility**: when the
user will generate several images that must share a look, a JSON style block is far easier to hold
constant than a paragraph.

---

## 1. Sectioned prose — typography travel poster

*Source: @iamaiistudio (case 431)*

**Demonstrates:** ALL-CAPS section headers as scaffolding; ratio declared in the first line; an
explicit anti-gibberish clause before anything else; and a conditional palette rule that lets one
prompt adapt to different inputs.

> Ultra-high-resolution typography travel poster themed around [CITY NAME]. 16:9 poster ratio.
>
> IMPORTANT: every visible word on the poster must be in English, perfectly spelled, professionally
> typeset. No distorted letters, no random symbols, no broken text, no AI gibberish.
>
> CORE COMPOSITION:
> Place the giant English word "[CITY_NAME]" front and center. Each letter is a tall, bold,
> elongated sans-serif form, and each one frames a different illustrated scene from the city, like a
> row of gallery windows. Spread landmarks, streets, transport, nature, culture, and architecture
> across the letters so the scenes flow from one letter into the next as a single connected urban
> panorama.
>
> TOP HORIZONTAL STRIP:
> Across the top, a thin panoramic band: city skyline silhouettes, cars, trams or trains, boats
> where it fits, birds, clouds, sun. Keep it minimalist, elegant, rhythmically balanced.
>
> STYLE: mid-century modern editorial poster, Swiss graphic design, minimal vector illustration,
> architectural infographic feel, travel typography poster, flat geometric illustration, ultra-clean
> composition, premium magazine design, screen-print poster vibe, retro-futuristic travel branding.
>
> ILLUSTRATION:
> Flat vector shapes only. No realism, no gradients, no noise. Clean geometric shadows, simplified
> architectural forms, a mix of map-like top-down with side-view cityscape. Subtle line-art details,
> crisp vector edges, strong negative space, harmonious rhythm between the letters.
>
> TYPOGRAPHY:
> Giant bold sans-serif, letters fill most of the canvas height. Pixel-perfect alignment. Each letter
> acts as its own framed illustration panel. Editorial spacing. Print-ready and geometrically clean.
>
> COLOR PALETTE:
> Pull a cohesive palette inspired by [CITY_NAME]:
> coastal city -> aqua, sand, coral, muted teal
> desert city -> terracotta, beige, warm cream
> cyber city -> mint, navy, steel blue
> historic European city -> dusty rose, olive green, parchment
> Muted pastels, soft vintage travel-poster colors, low saturation. Max 4 to 6 colors.
>
> COMPOSITION:
> Centered typography on a white or soft ivory background. Lots of breathing room. The top panoramic
> strip balances the heavy typography below. Asymmetrical but visually balanced.
>
> MOOD: premium, intellectual, calm, design-forward, travel-editorial.
>
> QUALITY: 8K ultra-detailed, print-ready, razor-sharp vector edges, flawless typography, zero
> distorted text, zero random characters, zero spelling errors, zero AI artifacts.

---

## 2. Use-case header + granular prose — cinematic portrait

*Source: @BubbleBrain (case 399)*

**Demonstrates:** stating the use case and target size before any visual detail — the "global
context first" principle; wardrobe and pose described down to a half-tied lace; and a film-stock
reference doing the work of ten adjectives.

> Use case: photorealistic-natural
> Asset type: cinematic portrait image, final target size 1216x1536 portrait
>
> Create a photorealistic image of a fictional adult Korean female idol in her mid-20s, not
> resembling any real celebrity. Maintain a Japanese negative film look: soft overexposure, faded
> neutrals, low contrast, subtle grain, and imperfect snapshot framing.
>
> Scene/backdrop: the back stairwell of a small record label building, with moving boxes, scuffed
> concrete steps, a gray metal handrail, and a pale security light. The atmosphere should feel quiet,
> slightly intimate, and workaday, as if caught in a private in-between moment after practice and
> during moving day.
>
> Wardrobe/props: a slightly cropped black blazer worn casually and slightly open, over a fitted
> heather-gray ribbed tee, loose khaki cargo pants sitting naturally on the waist, a thin silver
> chain necklace, a roll of black gaffer tape placed beside her, and one sneaker lace still
> half-tied.
>
> Composition/framing: tall portrait. The subject is seated on the stairs with one knee slightly
> raised and one leg relaxed lower on the step, leaning back lightly with one hand braced behind her
> on the stair. The other hand is near her half-tied sneaker, as if she has just paused while tying
> it. Her head is tilted up toward the camera with a calm, self-possessed expression. The pose should
> feel candid, natural rather than staged.
>
> Lighting/mood: flat stairwell light, understated backstage realism, soft grain, muted tones, gentle
> highlight bloom, quiet intimate mood. Photorealistic, restrained, cinematic.

Note the closing `--2:3` in the original. That is a Midjourney-style flag and it is **not** a
GPT Image 2 parameter — state the ratio in words instead.

---

## 3. Numbered panels — dense editorial infographic

*Source: @meng_dagg695 (case 364)*

**Demonstrates:** the only reliable way to control a dense multi-cell layout — enumerate the panels
and specify each one; a global render-spec block at the end that binds all panels to one look; and
typography discipline (exactly one serif display plus one sans body).

> LUXURY PERSONAL COLOR PROFILE — EDITORIAL LAYOUT
>
> Studio portrait of subject as anchor — skin retouched to luminous perfection, preserved natural
> structure, realistic pore texture, soft directional key lighting, no facial alteration. Background:
> warm ecru parchment with subtle linen grain. Layout reads like a beauty supplement printed on
> heavyweight matte stock. Structured editorial grid, 3-column asymmetric, wide negative space, serif
> condensed display headers, all labels in spaced uppercase tracking, cohesive warm ivory/sand/ecru
> background system throughout all panels, flat elegant surfaces, no drop shadows.
>
> PANELS:
> ① UNDERTONE DIAGNOSIS — Tonal spectrum bar from cool ash to warm amber, precision needle marker on
> the subject's reading. Labels: Cool / Neutral-Cool / Neutral / Neutral-Warm / Warm.
> ② SEASONAL COLOR PALETTE — 10-12 fabric-textured swatches. Each labeled with a color name and HEX.
> Grouped: Power Colors / Softest Options / Harmonizing Neutrals.
> ③ COLORS TO AVOID — Desaturated row of clashing tones with fine editorial strikethrough.
> ④ MAKEUP CARTOGRAPHY — Eyeshadow gradient swatches / blush tones / lip spectrum / highlighter
> finishes labeled: champagne, rose gold, pearlescent ivory.
> ⑤ HAIR COLOR SPECTRUM — Curved gradient strip: base, dimension, highlight, contrast tones.
> ⑥ JEWELRY & METAL GUIDE — Flat-lay render: yellow gold, rose gold, oxidized silver, platinum.
> ⑦ YOU IN YOUR PALETTE — 3-4 lookbook frames in palette-correct outfits.
> ⑧ CAPSULE WARDROBE GRID — Outfit flatlay with coordinating lines showing interchangeability.
> ⑨ PRINTS & PATTERNS — 4 fabric print thumbnails, one-line styling note per print.
> ⑩ STYLE ARCHETYPE — Single typographic panel. Style identity title set large. Three defining words.
>
> RENDER SPECS: Ultra-photorealistic, editorial magazine print quality, warm neutral color grading,
> soft diffused studio lighting consistent across all panels, one serif display font + one fine
> sans-serif body font, no gradients, flat matte surfaces only.

---

## 4. Compact prose — brand campaign poster

*Source: @Daniel_adsss (case 344)*

**Demonstrates:** that length is not the point. 556 characters, and it still names the subject, the
setting, the light, the typography, the exact copy, the palette and two exclusions. Reach for this
shape when the concept is simple and clear.

> Create a premium, highly realistic 1:1 campaign poster for NOIR, a modern streetwear brand. Show
> one hero oversized hoodie as the main focus against a gritty urban backdrop with wet concrete
> floors, dramatic low lighting, subtle smoke in the air and a raw street energy. Add bold minimal
> typography with the brand name NOIR and a short campaign headline "Wear the Dark." Make it feel
> like a real high-end streetwear editorial, sharp detail, realistic fabric textures, modern and
> edgy, deep black tones with subtle grey accents, no clutter, no collage.

---

## 5. JSON spec — product infographic

*Source: @AmberPromptai (case 157), simplified*

**Demonstrates:** JSON as the natural fit when the asset is structurally a set of sections with
repeating shapes. Every headline is a literal string, every callout is enumerated. Trivially
re-runnable for a different product by swapping values.

```json
{
  "type": "e-commerce product infographic",
  "theme": "dark mode with orange accents",
  "product": {
    "brand": "MEAN WELL",
    "model": "ELG-100-24B",
    "description": "100W constant current LED driver, rectangular silver metal housing with black cables on both ends and a detailed specification label"
  },
  "layout": {
    "sections": [
      {
        "name": "Hero",
        "elements": [
          "Brand logo top left",
          "Headline: 'Stable Power For Outdoors'",
          "Subtext: wide input voltage, protected housing",
          "Large angled product shot",
          "Faded '100W' watermark in background"
        ]
      },
      {
        "name": "Feature highlights",
        "count": 3,
        "panels": [
          { "title": "Precision Build", "visual": "Close-up of the specification label" },
          { "title": "Secure Connection", "visual": "Close-up of the cable entry and mounting ear" },
          { "title": "Key Features", "visual": "Angled product shot with 3 callout lines pointing to text: '100~305VAC Input', 'Constant Current', 'IP67 / IP65 Housing'" }
        ]
      },
      {
        "name": "Applications",
        "count": 4,
        "panels": [
          { "title": "For Street Lighting", "visual": "Nighttime highway illuminated by streetlights" },
          { "title": "For Outdoor Projects", "visual": "Modern building exterior with architectural lighting" },
          { "title": "For Indoor Systems", "visual": "Modern commercial hallway with linear ceiling lights" },
          { "title": "For Dimming Control", "visual": "Control box with 4 labels: '0-10V', 'PWM', 'RESISTOR', 'DALI'" }
        ]
      },
      {
        "name": "Environmental protection",
        "elements": [
          "Product resting on a wet surface with water droplets and rain effect",
          "Headline: 'Protected Performance'",
          "Badge: '5-Year Warranty'"
        ]
      }
    ]
  }
}
```

---

## 6. JSON style lock — illustration technique

*Source: @Just_sharon7 (case 435)*

**Demonstrates:** the strongest use of JSON — pinning an aesthetic so precisely that it survives
across many renders. Note the granularity: depth planes are counted, the head-to-body ratio is a
number, the eyes and blush are separate fields. This is how you keep a series visually consistent.

```json
{
  "style": "layered paper-cut illustration, papercraft diorama, handcrafted aesthetic",
  "technique": {
    "layering": "multiple stacked paper layers with soft drop shadows between each layer",
    "depth": "5-7 visible depth planes from foreground to background",
    "edges": "smooth, rounded, slightly beveled paper-cut edges",
    "texture": "subtle paper grain and fibrous texture on all surfaces",
    "shadows": "soft, diffused inner shadows beneath each layer suggesting physical depth"
  },
  "character_design": {
    "proportions": "chibi / cute simplified - large round head, small body (1:1.5 ratio)",
    "face": {
      "eyes": "small dot eyes, glossy highlight",
      "cheeks": "soft circular rosy blush patches",
      "nose": "absent or minimal dot",
      "mouth": "simple small curve smile"
    },
    "limbs": "short, rounded, stubby limbs",
    "outline": "clean smooth silhouette, no sharp corners"
  },
  "color_palette": {
    "mood": "warm, cozy, pastel",
    "tones": ["soft cream", "dusty rose", "sage green", "warm peach", "sky blue", "honey yellow"],
    "saturation": "low-to-medium, muted and gentle",
    "background": "warm off-white or soft gradient"
  },
  "lighting": {
    "type": "soft ambient light from top-front",
    "highlights": "gentle white edge highlights on top layers",
    "shadows": "warm light tan/beige shadow tones beneath cut layers"
  },
  "overall_mood": "warm, whimsical, cozy, handmade, storybook",
  "render_quality": "ultra-detailed papercraft art, studio photography lighting, sharp focus on layer edges"
}
```

---

## 7. Dense realism paragraph — fake phone photo of a screen

*Source: @kaanakz (case 440)*

**Demonstrates:** how to fight the model's instinct toward polish. The entire craft here is the
inventory of imperfections — subpixel grid, moire, dust, fingerprints, handheld noise, perspective
skew — followed by a negative list that forbids every form of cleanliness. Also note that every
piece of on-screen copy is quoted.

> Create a raw smartphone photo of a laptop screen, not a screenshot. Aspect ratio 3:4, high-angle
> downward POV from someone standing over a desk at night. The laptop display fills most of the
> frame, with a narrow strip of black keyboard and trackpad visible at the bottom. Strong realism:
> visible RGB subpixel grid, subtle moire bands, small dust specks, faint fingerprints, uneven glass
> reflections, handheld phone noise, slight perspective skew, no studio polish. macOS dark mode.
> Background app: Apple Notes with a late-night study note titled "Design Critique" and short visible
> bullets: "layout", "lighting", "source links", "ship tomorrow". Foreground app: FaceTime live
> preview window floating lower-right, showing a fictional adult man in his 20s sitting at a
> cluttered desk, hoodie, tired but amused expression, warm desk lamp behind him. A second small
> Finder window with image thumbnails is partly visible behind it. Make it feel like an accidental
> real phone photo of a working laptop screen. No real-person likeness, no beauty filter, no perfect
> UI, no screenshot, no watermark, no cartoon, no 3D render.

---

## What the corpus teaches in aggregate

- **Every strong prompt names its exclusions.** Not one of these seven ends without saying what must
  not appear.
- **Every visible string is quoted.** Headlines, bullets, badges, labels.
- **The ratio is stated early**, usually in the first line.
- **Adjectives are cheap; nouns and numbers are expensive and worth it.** "5-7 depth planes",
  "max 4 to 6 colors", "one serif display font", "3-column asymmetric grid".
- **Realism comes from listing imperfections**, never from the word "realistic".
- Beware flags borrowed from other tools. `--2:3`, `--ar`, `--v` are Midjourney syntax and do
  nothing here.
