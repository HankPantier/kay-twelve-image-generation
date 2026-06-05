# Visual Content System — Session Summary
**Date:** 2026-04-09

This document captures everything decided in the brainstorm session that produced the Kay-Twelve Visual Content System design.

---

## What we built

A complete design spec for Kay-Twelve's AI-driven image generation and social media content workflow. The system takes a plain-language brief and produces platform-ready photos and branded social cards across LinkedIn, Instagram, Facebook, the newsletter, and the blog.

---

## Confirmed decisions

### Audience & purpose
- The visual style guide is **AI-first** — Claude Code reads it at runtime to make generation decisions
- PDF export is human-readable (for photographers, team, agency partners)
- Both live in `ImageGeneration/docs/`

### Shot archetypes (3 types)
| Archetype | Description | Best for |
|---|---|---|
| **The Space** | Wide establishing shot, full environment | Project reveals, website hero, LinkedIn articles |
| **The Moment** | Close student action shot, one student, bokeh | Instagram feed, storytelling, blog in-article |
| **The Collaboration** | 2–4 students working together | LinkedIn, Facebook, newsletter, blog headers |

### Camera defaults (from EXIF analysis)
| Archetype | Camera | Lens | Aperture | ISO |
|---|---|---|---|---|
| The Space | Canon EOS R5 | 24mm | f/5.6 | 800 |
| The Moment | Nikon Z 8 | 85mm | f/1.4 | 400 |
| The Collaboration | Nikon Z 8 | 50mm | f/2.8 | 400 |

All archetypes: `film_stock: Kodak Portra 400` · `lighting: natural_sunlight` · `flash: never` · `magic_prompt_enhancer: false`

### Platform & aspect ratio mapping
| Ratio | Covers |
|---|---|
| **1:1** | LinkedIn feed, Facebook feed, Instagram square |
| **4:5** | Instagram primary feed (max reach), Facebook |
| **16:9** | Blog featured image, HubSpot newsletter header, LinkedIn article |
| **9:16** | Instagram Stories / Reels — on demand only |

Standard run = **3 files** (1:1 + 4:5 + 16:9). 9:16 generated only when explicitly requested.

### Workflows
1. **Full AI generation** — Nano Banana from scratch; Claude Code reads brief, identifies archetype, generates 3 schemas, submits to Nano Banana
2. **Inpainting** — Add students to real environment photos in `examples/environment/`; use when a real client space photo exists but lacks human presence

### Social card layer (Option C hybrid)
- **Figma** — Design master card templates once (1:1, 4:5, 16:9). Defines photo zone, logo, color bar, text zone.
- **Pencil MCP** — Automated card composition; reads Figma templates, drops in photo, applies branding, exports per platform
- Card zones: full-bleed photo · Pelorous Blue (#208ca9) color bar (bottom 12%) · Kay-Twelve logo (bottom-left) · optional caption text (bottom-right)

---

## Document structure (12 sections)

### Layer 1 — Photography Standards
1. Purpose & scope
2. Brand visual identity (colors + fonts for overlays)
3. Photography philosophy
4. Good vs. bad: annotated examples
5. Shot archetypes (The Space · The Moment · The Collaboration)
6. Composition & safe zone rules
7. Universal negative prompts library

### Layer 2 — Generation Workflow
8. Platform & aspect ratio guide
9. Nano Banana schema defaults (per archetype)
10. Workflow A: full AI generation
11. Workflow B: inpainting (student insertion)
12. Social card composition (Figma + Pencil)

---

## Files created this session

| File | Purpose |
|---|---|
| `docs/superpowers/specs/2026-04-09-kay-twelve-visual-style-guide-design.md` | Full approved design spec |
| `docs/kay-twelve-visual-content-system-overview.pdf` | Client-shareable 6-page overview |
| `docs/kay-twelve-visual-content-system-overview.html` | HTML source for PDF |
| `docs/session-summary-2026-04-09.md` | This document |

---

## What comes next

1. **Implementation plan** — `writing-plans` skill to create a step-by-step plan for writing the style guide
2. **Visual style guide** — the actual `kay-twelve-visual-style-guide.md` and `.pdf`
3. **Figma card templates** (follow-on) — 3 master templates for social card design
4. **Pencil automation** (follow-on) — `.pen` file for card composition workflow

---

## Open questions / implementation notes

- **Inpainting schema syntax** — Nano Banana's `input_image` is subject-scoped in the schema. Exact syntax for scene-level inpainting (environment photo as scene base) needs verification against Nano Banana's actual API at implementation time. Fallback: `style_transfer` usage type.
- **Figma template design** — Visual polish, exact proportions, and logo lockup are a separate design deliverable. The spec defines what the templates must contain, not how they look.
