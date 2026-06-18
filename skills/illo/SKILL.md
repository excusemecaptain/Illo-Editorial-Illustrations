---
name: illo
description: >-
  Creates original editorial illustrations where a recurring mascot character
  performs the idea — one editorial scene by default, or a hand-built explainer
  diagram — in one of ten bundled print looks (riso, blueprint, woodcut, pixel,
  clay, manila, chalk, phosphor, enamel, gouache, felt). Powered by Venice AI
  image generation. Trigger: user says "illo", "make an illo", "editorial
  illustration", "mascot illustration", or explicitly invokes this skill.
  Adapted from tmchow/illo-skill (MIT). NEVER trigger on generic
  "illustrate / draw / make an image" requests.
version: 1.0.0
author: excusemecaptain (Agent Zero adaptation)
original_author: Trevin Chow
original_source: https://github.com/tmchow/illo-skill
license: MIT
---

# Illo — Editorial Illustrations for Agent Zero

Make original, distinctive editorial illustrations for written content. One
image explains one idea: a key judgment, a flow, a before/after, a trap, a
loop. A **recurring mascot (Blot)** performs the idea in every scene — the
subject, never decoration.

## Backend: Venice AI

This Agent Zero adaptation uses **Venice AI** (`VENICE_API_KEY` env var) as
the image backend. The `illo.py` script auto-detects it when no OpenRouter
key is configured. Default model: `flux-2-max`.

## Prerequisites (run once)

Verify the engine is ready:

```bash
SKILL_DIR="/a0/usr/plugins/illo/skills/illo"
python3 "$SKILL_DIR/scripts/illo.py" doctor
```

Exit 0 = ready. If it asks for backend selection, run:
```bash
python3 "$SKILL_DIR/scripts/illo.py" init --backend openrouter --no-key
```
(Venice resolves automatically via env var when no OpenRouter key is set.)

## Use Cases

| The user wants | Path |
|---|---|
| Illustrate an article / URL / paste | Steps 0–7: route source, shot list, one image per anchor |
| One image for a concept | Single image after up to ~3 quick questions |
| A sequence (before→after, process) | Mini-comic: 2–4 panels in one image |
| A traceable structure (pipeline, flow) | Explainer register: hand-built diagram |
| A different style | Read `references/styles/<name>.md` |
| A custom mascot | Read `references/character-builder.md` |

## Workflow

### 0. Preflight
```bash
SKILL_DIR="/a0/usr/plugins/illo/skills/illo"
python3 "$SKILL_DIR/scripts/illo.py" doctor
```

### 1. Read input and clarify (briefly)
- **URL/article**: lock thesis → pick coverage → shot list
- **Bare concept**: ask up to 3 quick questions, then build

### 2. Resolve character
Default: **Blot** (deadpan ink-drop, riso style). Read `references/character.md`.

### 3. Plan (shot list) for multi-image
Per image: placement, idea, register, staging, mascot action, palette, labels.
Read `references/composition.md`.

### 4. Resolve palette and style
Read `references/palettes.md`. Default: `ink-punch`. Style travels with character pack.
For non-riso styles read `references/styles/<name>.md`.

### 5. Generate

```bash
SKILL_DIR="/a0/usr/plugins/illo/skills/illo"
REF="$SKILL_DIR/assets/character-reference.webp"

# Write prompt to file first
cat > /tmp/shot-01.txt << 'PROMPT'
[Full prompt from references/prompt-recipe.md]
PROMPT

# Generate with Venice backend
python3 "$SKILL_DIR/scripts/illo.py" generate \
  --prompt-file /tmp/shot-01.txt \
  --ref "$REF" \
  --aspect 16:9 \
  --out "/a0/usr/workdir/assets/<slug>-illustrations/01-topic.png"
```

**Read `.path` from JSON output** — the engine names files by actual encoding.

### 6. QA
Read `references/quality-bar.md` and check every image. Re-roll when:
- Mascot is decorative / off-model
- Wrong aspect ratio (check `.width/.height` in JSON)
- Unwanted title bar
- Label text on colored fill
- Accent spread past limits

### 7. Deliver
Report: files, palette used, strongest vs optional, and placement map for sets.

## References (load on demand)

- `references/visual-style.md` — riso house default
- `references/styles/<name>.md` — other looks (blueprint, woodcut, pixel, etc.)
- `references/character.md` — Blot spec and custom pack format
- `references/character-builder.md` — build a custom mascot
- `references/composition.md` — registers, structure types, shot list format
- `references/palettes.md` — named presets and derive-from-color algorithm
- `references/prompt-recipe.md` — full generation prompt template
- `references/quality-bar.md` — post-generation checklist
- `references/backends.md` — backend resolution logic
- `references/models.md` — OpenRouter model lineup (OpenRouter backend only)

## Venice Models

| Model | Notes |
|---|---|
| `flux-2-max` | Default; best quality, 16:9 support |
| `flux-dev` | Faster, good for drafts |
| `stable-diffusion-3-5` | Good style adherence |

Pass via `--model <id>` to override. Venice does not support reference image
conditioning the same way OpenRouter models do — the mascot spec is embedded
in the prompt text using the character description from `references/character.md`.
