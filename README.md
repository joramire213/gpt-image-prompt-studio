# GPT Image 2 Style Library

An agent skill that turns a rough image idea into a production-ready prompt for **GPT Image 2**,
OpenAI's current image model. Give it a one-line description of the image you want — a poster, an
infographic, a product shot, a persona transformation from a reference photo — and it hands back a
finished, copy-ready prompt. **It never generates the image itself, only the prompt text.**

Built on:

- **28 industrial prompt templates**, each with a skeleton and a documented list of failure modes
  ("pitfalls"), covering posters, UI screens, brand identity, architecture, photography,
  illustration, characters, historical scenes, documents, and more.
- **The model's real capabilities**, sourced from OpenAI's own prompting guide: valid sizes and
  aspect ratios, quality levels, how to make text render correctly, how to phrase exclusions,
  documented weaknesses, and techniques for fighting the "AI-generated" look.
- **7 worked examples** mined from a corpus of 541 real, published GPT Image 2 prompts, each
  annotated with the technique it demonstrates.

## This is a fork

This skill started as [`freestylefly/awesome-gpt-image-2`](https://github.com/freestylefly/awesome-gpt-image-2)'s
`gpt-image-2-style-library` skill (26 templates, Chinese/English bilingual, no model-capability
reference). It has since diverged substantially:

- Adapted for Spanish as the input language (final prompts remain in English, which is what the
  model handles best).
- Removed all Chinese-language content.
- Added `references/gpt-image-2.md` — the model-capabilities file described above, which the
  original didn't have.
- Added `references/examples.md` — 7 real prompts mined from the source project's own case corpus.
- Added 2 new templates the original 26 didn't cover: **Real place, transformed** (a real, named
  location shown after a stated change — decades of abandonment, disaster, an alternate timeline —
  while staying recognizable) and **Identity-preserving persona transformation** (restyling a real
  person from a reference photo into a theme while keeping them recognizable).
- Rewrote the workflow around an explicit ask-vs-ship calibration, a sizing/validation procedure
  against the model's actual size constraints, and a pre-delivery verification checklist.
- Removed `references/style-library.md` (the original catalog index), superseded by the above.

See `LICENSE` for the original MIT license and attribution.

## Install

**Via the [skills CLI](https://skills.sh):**

```bash
npx skills add joramire213/gpt-image-2-style-library -g -y
```

**Manually**, if the CLI doesn't pick it up automatically: copy `SKILL.md`, `references/`, `agents/`
and `assets/` from this repo into your agent's skills folder — for Claude Code, that's
`~/.claude/skills/gpt-image-2-style-library/`.

**Via the bundled installer**, from a local clone:

```bash
node bin/install.mjs install all        # installs for Claude Code, Codex, and any agent reading ~/.agents/skills
node bin/install.mjs install claude-code # just Claude Code
```

## Use

Describe the image you want, in Spanish or English — "hazme un prompt para un póster de...",
"necesito un prompt de imagen para...", "give me a prompt for a product shot of...". The skill
classifies the request against its template library, asks only for what it genuinely can't infer,
and returns the finished prompt in English, ready to paste into GPT Image 2.

## No warranty

This is a personal project, shared as-is. See `LICENSE`.
