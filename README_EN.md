# Novel Rewriting Toolkit

> An AI-powered toolkit for rewriting web novels with original settings — driven by Claude Code.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[中文文档](README.md)

## What This Is

This toolkit takes a source novel and produces a completely original rewrite — new names, new locations, new power systems, new dialogue — while preserving the story's narrative structure and emotional rhythm.

It's a **three-stage automated pipeline** that runs inside Claude Code:

```
Source Novel → Stage 1·OUTLINE → Stage 2·RESTRUCTURE → Stage 3·WRITE → Original Rewrite
```

## Key Features

- **100% original output**: All proper nouns replaced, all dialogue rewritten, all descriptions recreated
- **Theme swapping**: Source novel can be adapted to any genre (urban, sci-fi, xianxia, post-apocalyptic, etc.)
- **Quality guardrails**: Built-in anti-AI-style detection, consistency checking, and copyright safety verification
- **Progress resilience**: Every step persists to disk. Interrupt and resume anytime without losing work.
- **Single-command operation**: Each stage starts with one instruction — Claude Code drives the entire workflow.

## How It Works

### Stage 1 — OUTLINE (Source Distillation)
Distills the source novel into structured archives while keeping original names:
- Splits chapters automatically
- Extracts world-building, factions, power systems, character profiles
- Creates per-chapter outlines with 8 dimensions (scene, event, conflict, combat, emotion, interaction, twist, target word count)
- Produces a name mapping of all source-specific terms

### Stage 2 — RESTRUCTURE (Outline Transformation)
Transforms the outline into an original work:
- **Step -1**: Four-pass analysis — story arcs, rhythm patterns, theme baseline calibration, forbidden term extraction
- **Step 0**: Theme conversion — generates new world settings, power systems, character identities, and a naming scheme constrained to common Chinese characters (3500常用字)
- **Step 1**: Rhythm-aware rewriting of each chapter outline with borrow-degree calibration

### Stage 3 — WRITE (Chapter Creation)
Writes the final novel chapter by chapter:
- Uses Claude Code with Opus-level models for quality
- Anti-AI-style rules enforced on every chapter
- Rolling 10-chapter quality audits
- Auto-generated chapter summaries and character state tracking
- Final output: ~2000-2300 words per chapter, ready for web novel platforms

## Repository Structure

```
├── README.md                      # This file
├── README_CN.md                   # Full Chinese documentation
├── RULES.md                       # Writing rules & prohibitions (enforced by AI)
├── WRITING_PARADIGM.md            # Writing paradigm & style guide
├── WRITE_SKILL.md                 # Chapter writing skill specification
├── SESSION_CONTROL.md             # Session lifecycle & resume logic
├── theme.md                       # Theme system design
├── MANUAL_OUTLINE.md              # Stage 1 manual (single-threaded)
├── MANUAL_OUTLINE_PARALLEL.md     # Stage 1 manual (parallel mode)
├── MANUAL_RESTRUCTURE.md          # Stage 2 manual
├── MANUAL_WRITE.md                # Stage 3 manual
├── polish_skill/                  # Quality polishing subsystem
│   ├── anti_ai_style_rules.md     # Anti-AI-style detection rules
│   └── system_prompt.md           # Polish agent system prompt
├── external_style_samples/        # Drop reference style samples here
└── themes/                        # Theme definition files
```

## Prerequisites

- **Claude Code** (with Claude Opus or Sonnet API access)
- **Python 3.8+** (for auxiliary merge scripts)
- A source novel in `.txt` format (UTF-8 or GBK)

## Quick Start

1. **Clone this repo** and open it in Claude Code:
   ```bash
   git clone https://github.com/sssst1118/novel-rewriting-toolkit.git
   cd novel-rewriting-toolkit
   code .
   ```

2. **Drop your source novel** into the project root (any filename, UTF-8/GBK/GB18030 all supported).

3. **(Optional)** Drop reference style samples into `external_style_samples/`.

4. **Launch Stage 1** by telling Claude Code:
   ```
   按 MANUAL_OUTLINE.md 执行
   ```
   For large novels (>500 chapters), use parallel mode:
   ```
   按 MANUAL_OUTLINE_PARALLEL.md 执行，段 ID = A，负责章号范围 0001-0500
   ```

5. The pipeline runs automatically. When Stage 2 Step -1·C completes, Claude Code will pause and ask: **"Which theme?"** — tell it the genre you want (e.g., "末世 post-apocalyptic", "都市异能 urban powers").

## Time Estimates (for a 1500-chapter source)

| Stage | Sessions | Wall-clock |
|-------|----------|------------|
| Stage 1 (single) | 10-15 | ~1-2 weeks |
| Stage 1 (2-way parallel) | Same but split | ~3-5 days |
| Stage 2 | 37-51 | ~1 week |
| Stage 3 | 100-150 | 1-2 months |
| **Total** | **~150-200** | **1.5-2.5 months** |

## Important Notes

- **Copyright safety**: The toolkit is designed to produce original works. It replaces all proper nouns, rewrites all dialogue, and transforms all descriptions. The output is meant to be an independent original creation.
- **This is NOT a one-click solution**: It's a power tool for authors who want to systematically transform narrative frameworks while maintaining quality control. You'll spend ~10-20 minutes per day launching resume sessions.
- **Model recommendation**: Opus 4.7+ strongly recommended for Stage 3 (style control). Sonnet works for Stages 1-2 at lower cost.

## License

MIT License — see [LICENSE](LICENSE) for details.

## Acknowledgments

Built on [Claude Code](https://claude.ai/code) and the [Anthropic API](https://anthropic.com/api).
