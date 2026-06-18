# Illo — Editorial Illustrations for Agent Zero

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)

Original editorial illustrations where a recurring mascot character performs the idea — one editorial scene or a hand-built explainer diagram — in one of ten bundled print looks.

> **Adapted from [tmchow/illo-skill](https://github.com/tmchow/illo-skill)** by Trevin Chow (MIT License).  
> This Agent Zero plugin adds Venice AI as the image backend.

---

## What It Does

- 🎨 **10 visual styles**: riso (default), blueprint, woodcut, pixel, clay, manila, chalk, phosphor, enamel, gouache, felt
- 🐾 **Default mascot**: Blot — a deadpan ink-drop character that *performs* the idea, never decorates
- 📐 **Two registers**: editorial scene (one caught moment) or explainer diagram (flow, fan-out, timeline, loop)
- 🖼️ **Mini-comics**: 2–4 panel sequences for ideas that advance through stages
- 🎭 **Custom mascots**: build your own character pack
- 🌐 **Venice AI powered**: uses `flux-2-max` by default

## Trigger Phrases

Use one of these to activate the skill:
- `"make an illo"`
- `"create an editorial illustration"`
- `"use illo to illustrate this article"`
- `"illo this concept"`

> Do **not** trigger on generic "draw" or "make an image" requests.

## Requirements

- **Venice AI API key**: set `VENICE_API_KEY` environment variable
- **Python 3**: already available in Agent Zero

## Setup

1. Enable the plugin in Agent Zero Settings → Plugins → Illo
2. Ensure `VENICE_API_KEY` is set in your environment
3. Run preflight check:

```bash
SKILL_DIR="/a0/usr/plugins/illo/skills/illo"
python3 "$SKILL_DIR/scripts/illo.py" doctor
```

4. If it reports `backend: NEEDS CHOICE`:
```bash
python3 "$SKILL_DIR/scripts/illo.py" init --backend openrouter --no-key
```
*(Venice resolves automatically via `VENICE_API_KEY` env var)*

## Usage Examples

```
# Single concept
Make an illo of "you are the bottleneck"

# Article illustration set
Use illo to illustrate this article:
[paste article text]

# Specific style
Make an illo in blueprint style showing the pipeline failing

# Planning mode (no generation)
Use illo to plan a shot list for this piece — don't generate yet
```

## Visual Styles

| Style | Character |
|---|---|
| `riso` | Bold halftone grain, ink-layer offset, paper grain (default) |
| `blueprint` | White lines on blue paper, technical drawing feel |
| `woodcut` | High contrast, rough-cut lines |
| `pixel` | Retro pixel art |
| `clay` | Soft 3D clay look |
| `manila` | Warm manila paper, muted ink |
| `chalk` | Chalk on blackboard |
| `phosphor` | Glowing phosphor green on dark |
| `enamel` | Vintage enamel pin look |
| `gouache` | Flat gouache paint fills |
| `felt` | Stacked felt layers, cute but clean |

## Attribution

This is an adaptation of **Illo by Trevin Chow**.

- Original skill: **[tmchow/illo-skill](https://github.com/tmchow/illo-skill)**
- Original author: **Trevin Chow** ([tmchow](https://github.com/tmchow))
- Copyright © 2026 Trevin Chow
- Agent Zero adaptation: **excusemecaptain** — added Venice AI backend

Per the [NOTICE](./skills/illo/NOTICE) file: all redistributions and derivative works must retain the original copyright notice and credit **"Illo by Trevin Chow."**

## License

MIT — see [LICENSE](./LICENSE) and [NOTICE](./skills/illo/NOTICE)
