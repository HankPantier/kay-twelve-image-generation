# Kay-Twelve Visual Style Guide — Design Spec

**Date:** 2026-04-09  
**Status:** Approved  
**Deliverables:** `ImageGeneration/docs/kay-twelve-visual-style-guide.md` + `.pdf`

---

## Context

Kay-Twelve needs a reusable visual style guide that governs all social media and content imagery. The guide serves two roles simultaneously:

1. **AI-executable instructions** — Claude Code reads this document at runtime to make generation decisions (shot type selection, Nano Banana schema defaults, platform batching, workflow routing)
2. **Human reference** — exported as PDF for photographers, agency partners, and the Kay-Twelve team

The guide covers two image creation workflows: full AI generation (Nano Banana from scratch) and inpainting (adding students to real environment photos). Generated photos feed into a two-stage social card pipeline: Nano Banana produces the photo, Pencil MCP composes the branded card using Figma-defined templates.

---

## Architecture

### Two-layer document structure

**Layer 1 — Photography Standards** (sections 1–7)  
Defines what on-brand looks like. Used by Claude Code for generation decisions. Also human-readable for photographers.

**Layer 2 — Generation Workflow** (sections 8–12)  
Executable instructions for Claude Code. References Layer 1 at every decision point.

### End-to-end workflow (Option C hybrid)

```
User brief
    ↓
Claude Code reads style guide → identifies archetype
    ↓
Nano Banana schemas generated (3 per full run: 1:1, 4:5, 16:9)
    ↓
Photos returned from Nano Banana
    ↓
Pencil reads Figma template definitions
    ↓
Branded social cards composed per platform
    ↓
Output: raw photos + branded cards, organized by platform
```

Inpainting variant: same loop, but Step 3 includes `input_image` pointing to a real environment photo.

---

## Section-by-section spec

### Section 1 — Purpose & scope

Short (half page). States:
- This document is AI-first — Claude Code reads it to make generation decisions
- It covers photography standards AND generation workflow
- Platforms in scope: LinkedIn, Instagram, Facebook, HubSpot newsletter, blog, website
- Links to `examples/` folder for visual reference

### Section 2 — Brand visual identity (reference only)

For use in social card overlays — not photography direction.

| Role | Color | Hex |
|---|---|---|
| Primary | Pelorous Blue | `#208ca9` |
| Secondary | Yellow | `#ffb41f` |
| Secondary | Shady Lady Gray | `#9e9ca4` |
| Secondary | Pink | `#ea5581` |
| Secondary | Chelsea Cucumber Green | `#88a95b` |

| Font | Usage |
|---|---|
| Edo | Headlines / display |
| Source Sans Pro | Subheadings / body |
| Helvetica | Body / general use |

### Section 3 — Photography philosophy

Narrative section capturing the Kay-Twelve visual story. Key principles:

- **Human-first:** Students are always the emotional center. Spaces tell stories through people, not furniture catalogs.
- **Natural light:** Never flash. Light should feel warm, even, and real — not studio-staged.
- **Clean but lived-in:** Spaces feel usable and real. Slight imperfections add authenticity. Never sterile.
- **Storytelling over product shots:** Every image should answer "what is learning like here?" — not "what furniture is this?"
- **Two types of image serve different purposes:** Wide shots establish context; close shots capture moments. Both are needed.

Sources: confirmed by user against approved example reasoning. Grounded in EXIF analysis (no flash in any approved photo; all ambient/natural light).

### Section 4 — Good vs. bad: annotated examples

Each example listed with:
- Image path (relative to `ImageGeneration/`)
- One-line classification label
- 2–3 bullet rules derived from the reasoning

**Good examples:**

| File | Label | Rules |
|---|---|---|
| `examples/photos/example-1.jpg` | Innovative open space, students present | Wide sightlines; natural light from windows; students in background, not posed |
| `examples/photos/example-2.jpg` | Student interacting creatively with furniture | Subject centered in furniture; expression engaged not performative; minor distractors (trash can) acceptable if croppable |
| `examples/photos/example-3.jpg` | Fresh modern environment, minimal clutter | Non-traditional layout; color and material choices visible; no visual noise |
| `examples/photos/example-4.jpg` | Action shot, high-res, considered framing | Crisp, professional resolution; authentic interaction; lighting and framing deliberate |
| `examples/photos/example-5.jpg` | Collaboration, energy and purpose | Students facing activity not camera; texture and spatial design visible; not posed |

**Bad examples:**

| File | Label | Rules violated |
|---|---|---|
| `examples/bad/ex-1.jpg` | Grainy, cluttered background | Resolution too low; wall decor creates visual noise; furniture not the hero |
| `examples/bad/ex-2.jpg` | No students, cluttered, not a wide shot | No human presence; cluttered shelves dominate; composition too tight |
| `examples/bad/ex-3.JPG` | Distracting elements | Water bottles visible; student with shirt over head; too much happening |
| `examples/bad/ex-4.jpg` | No students, worn space, clutter | No human presence; tape on floor; overflowing storage; doesn't read as "new space" |

**Environment folder (no students):**  
`examples/environment/` — 5 clean professional shots of real spaces. Use as `input_image` for inpainting workflow. Do not use as-is for social without student insertion.

### Section 5 — Shot archetypes

Three named archetypes. Claude Code identifies the correct one from the user brief before generating any schema.

---

#### The Space

Wide establishing shot. Shows full learning environment — architecture, furniture layout, sightlines, natural light. Students may be present but are secondary to the space. No students required.

**When to use:** Project reveals, website hero images, LinkedIn article covers, newsletter headers.

**Composition rules:**
- Shoot from corner or doorway to maximize sightlines
- No foreground clutter
- Subject centered with generous frame on all sides (safe zone compliance)
- Natural light source visible or clearly implied
- Students optional — if included, 1–2 max, positioned in mid-ground

**Nano Banana defaults:**
```json
{
  "meta": { "aspect_ratio": "{{PLATFORM_RATIO}}", "quality": "ultra_photorealistic" },
  "technical": {
    "camera_model": "Canon EOS R5",
    "lens": "24mm",
    "aperture": "f/5.6",
    "iso": "800",
    "film_stock": "Kodak Portra 400"
  },
  "composition": {
    "framing": "wide_shot",
    "angle": "eye_level",
    "focus_point": "whole_scene"
  },
  "scene": {
    "lighting": { "type": "natural_sunlight", "direction": "front_lit" }
  }
}
```

---

#### The Moment

Close action shot. One student engaged with the space — reading, writing, thinking, interacting with furniture in a creative or unexpected way. Authentic, never posed. Background softly blurred.

**When to use:** Instagram primary feed posts, storytelling moments, blog in-article images, email headers.

**Composition rules:**
- Subject centered, generous head room above
- Background softly blurred (bokeh) — space visible but not distracting
- Expression: engaged, natural — never smiling at camera
- At least one piece of furniture or environment element visible
- Portrait orientation preferred (4:5) — plan crop accordingly

**Nano Banana defaults:**
```json
{
  "meta": { "aspect_ratio": "{{PLATFORM_RATIO}}", "quality": "ultra_photorealistic" },
  "technical": {
    "camera_model": "Nikon Z 8",
    "lens": "85mm",
    "aperture": "f/1.4",
    "iso": "400",
    "film_stock": "Kodak Portra 400"
  },
  "composition": {
    "framing": "medium_shot",
    "angle": "eye_level",
    "focus_point": "face"
  },
  "scene": {
    "lighting": { "type": "natural_sunlight", "direction": "side_lit" }
  }
}
```

---

#### The Collaboration

Group shot showing 2–4 students working together — discussion, problem-solving, hands-on activity. Space is equally important as students. Energy and purpose over posed arrangement.

**When to use:** LinkedIn feed, Facebook posts, newsletter features, blog headers, project case studies.

**Composition rules:**
- 2–4 students max — avoid crowd feel
- Students facing each other, not the camera
- An activity visible — something in their hands or on the work surface
- Space tells the story equally — furniture, materials, and layout clearly visible
- Subject group centered for multi-ratio crop safety

**Nano Banana defaults:**
```json
{
  "meta": { "aspect_ratio": "{{PLATFORM_RATIO}}", "quality": "ultra_photorealistic" },
  "technical": {
    "camera_model": "Nikon Z 8",
    "lens": "50mm",
    "aperture": "f/2.8",
    "iso": "400",
    "film_stock": "Kodak Portra 400"
  },
  "composition": {
    "framing": "full_body",
    "angle": "eye_level",
    "focus_point": "whole_scene"
  },
  "scene": {
    "lighting": { "type": "natural_sunlight", "direction": "front_lit" }
  }
}
```

### Section 6 — Composition & safe zone rules

Because every image must work across multiple aspect ratios from a single generation, composition must be center-weighted with safe zones on all sides.

**The multi-ratio problem:**
- 16:9 is very wide (1200×630)
- 4:5 is taller than square (1080×1350)
- The same image must crop cleanly to both

**Safe zone requirement:**
- Primary subject must be centered horizontally
- Minimum 15% clear space above subject's head
- Minimum 10% clear space below subject's feet
- No critical content within 10% of any edge
- No text or logos in subject area (reserved for card composition layer)

**What this means in Nano Banana terms:**
- Always specify `position: "center"` for primary subjects
- Use `framing` values that leave room: prefer `medium_shot` over `close_up`, `wide_shot` over `medium_shot` for multi-subject scenes
- Set `focus_point: "whole_scene"` for The Space and The Collaboration archetypes

### Section 7 — Universal negative prompts library

Applied to every Nano Banana run regardless of archetype. Include in `advanced.negative_prompt` array:

```json
[
  "clutter", "storage overflowing", "tape on floor", "water bottles",
  "trash can", "fan", "paper piles", "boxes", "worn furniture",
  "posed smile at camera", "looking at camera", "smiling at camera",
  "low quality", "grainy", "blurry", "blur", "noise", "grain",
  "watermark", "text", "logo", "bad hands", "extra fingers", "mutated",
  "flash photography", "harsh lighting", "studio lighting", "artificial light",
  "catalog style", "sterile", "empty showroom", "product photography",
  "cropped", "worst quality", "distorted"
]
```

---

### Section 8 — Platform & aspect ratio guide

**Standard full run — 3 files covers all platforms:**

| Ratio | Nano Banana value | Covers |
|---|---|---|
| 1:1 | `"1:1"` | LinkedIn feed, Facebook feed, Instagram square |
| 4:5 | `"4:5"` | Instagram primary feed, Facebook feed (max reach) |
| 16:9 | `"16:9"` | Blog featured image, HubSpot newsletter header, LinkedIn article |

**On-demand only:**

| Ratio | Nano Banana value | Covers |
|---|---|---|
| 9:16 | `"9:16"` | Instagram Stories, Reels (generate only when explicitly requested) |

**Output file naming convention:**
```
[brief-slug]_[archetype]_[ratio].jpg
Example: library-collab_collaboration_4x5.jpg
```

**When a user requests specific platforms**, map them:
- "LinkedIn" → 1:1 + 16:9
- "Instagram" → 4:5 + 1:1
- "Facebook" → 1:1 + 4:5
- "newsletter" or "blog" → 16:9
- "Stories" or "Reels" → 9:16
- De-duplicate: if LinkedIn + blog both requested, generate 16:9 once

### Section 9 — Nano Banana schema defaults (per archetype)

Full drop-in schema templates for each archetype. Replace `{{PLATFORM_RATIO}}` with the target ratio. Replace `{{SUBJECT_DESCRIPTION}}` and `{{SCENE_DESCRIPTION}}` with brief-specific content.

Schemas include:
- All confirmed `technical` settings (camera, lens, aperture, ISO, film stock)
- All confirmed `composition` settings (framing, angle, focus point)
- All confirmed `scene.lighting` settings
- Full `advanced.negative_prompt` array from Section 7
- `advanced.hdr_mode: true`
- `advanced.magic_prompt_enhancer: false` (disabled — style guide is the enhancer)

Note: `magic_prompt_enhancer` is disabled because the style guide's rules already define the aesthetic. Allowing AI to expand the prompt risks drifting from established style.

### Section 10 — Workflow A: full AI generation

Step-by-step instructions for Claude Code:

1. Read user brief
2. Identify platforms requested → map to required ratios (Section 8)
3. Identify shot archetype from brief content (The Space / The Moment / The Collaboration)
4. Load base schema for identified archetype (Section 9)
5. Fill `subject` array from brief — describe student(s), age, clothing, pose, expression
6. Fill `scene.location` from brief — describe the space type (library, classroom, etc.)
7. Fill `scene.background_elements` with space-specific details
8. Clone schema for each required ratio, updating only `meta.aspect_ratio`
9. Submit all schemas to Nano Banana
10. Save returned photos using naming convention (Section 8)
11. Proceed to social card composition (Section 12) if social platforms requested

**Example brief → archetype mapping:**
- "two students working together at a flexible table" → The Collaboration
- "a student reading in the library corner" → The Moment
- "our new open learning commons" → The Space

### Section 11 — Workflow B: inpainting (student insertion)

**When to use:**
- A real Kay-Twelve environment photo exists (from `examples/environment/` or client-provided)
- The space is correct but lacks student presence
- The goal is to show the space as built, not a generic AI environment

**Schema difference from Workflow A:**

> **Implementation note:** Nano Banana's `input_image` is subject-scoped in the schema. For true inpainting (placing students *into* an existing environment photo), the environment image functions as the scene base, not a subject reference. The exact schema syntax for scene-level inpainting must be verified against Nano Banana's actual API at implementation time. The intent is:
> - Environment photo = scene/background reference
> - Generated student(s) = composited into that scene
>
> If Nano Banana doesn't support scene-level `input_image`, the fallback is `style_transfer` on the environment image to maintain the space's look and feel while generating the full scene from scratch.

**Intended schema structure (verify at implementation):**
```json
{
  "subject": [{
    "type": "person",
    "description": "{{STUDENT_DESCRIPTION}}",
    "pose": "{{POSE}}",
    "expression": "engaged",
    "position": "center"
  }],
  "scene": {
    "location": "{{DERIVED FROM ENVIRONMENT PHOTO}}",
    "input_image": {
      "path": "examples/environment/environment-1.jpg",
      "usage_type": "style_transfer",
      "strength": 0.75
    }
  }
}
```

**Decision rules — inpainting vs. full generation:**
- Real client project photo available AND space is the story → **inpainting**
- Generic content (blog illustration, social post not tied to a specific project) → **full generation**
- Client photo is cluttered or violates bad-example rules → **full generation** (do not inpaint into a bad-example space)

**Strength setting guide:**
- `0.85–1.0`: Tight to reference photo — student closely matches reference pose/clothing
- `0.6–0.75`: Loose — student feels natural in space, less tied to reference
- Default: `0.75`

### Section 12 — Social card composition (Figma + Pencil)

**Overview:** Photos from Sections 10–11 are raw images. Social cards add brand elements (logo, color bar, text zone) via a two-tool pipeline.

**Figma template spec (one-time setup):**

Three master templates required, one per standard ratio:
- `kt-social-card-1x1.fig`
- `kt-social-card-4x5.fig`
- `kt-social-card-16x9.fig`

Each template contains these zones:
| Zone | Position | Notes |
|---|---|---|
| Photo layer | Full bleed, bottom | Raw Nano Banana photo fills entire card |
| Color bar | Bottom 12% | Pelorous Blue `#208ca9`, semi-transparent overlay |
| Logo | Bottom-left, within color bar | Kay-Twelve wordmark, white |
| Text zone | Bottom-right, within color bar | Optional caption text, Source Sans Pro, white |
| Safe frame | Inset 5% all sides | No critical content outside this frame |

**Pencil generation instructions:**

When composing a social card batch:
1. Read Figma template for target ratio
2. Insert Nano Banana photo into photo layer
3. Apply color bar overlay at 80% opacity
4. Place logo at defined coordinates
5. If caption text provided: populate text zone
6. Export at platform-specified dimensions
7. Output naming: `[brief-slug]_card_[ratio].png`

**Note:** Figma template design (visual polish, exact proportions, logo lockup) is a follow-on deliverable. This section defines the spec those templates must satisfy.

---

## Deliverables

| File | Format | Purpose |
|---|---|---|
| `ImageGeneration/docs/kay-twelve-visual-style-guide.md` | Markdown | AI-executable primary doc, read by Claude Code at runtime |
| `ImageGeneration/docs/kay-twelve-visual-style-guide.pdf` | PDF | Human-shareable export for team, photographers, agency partners |

PDF generated from Markdown via pandoc or equivalent. Style guide `.md` is the source of truth — PDF is always derived from it.

---

## Follow-on work (out of scope for this deliverable)

- Figma social card template design (3 templates: 1:1, 4:5, 16:9)
- Pencil `.pen` file for card generation automation
- 9:16 Stories/Reels template
- Alt-text and caption style guidance
