# Kay-Twelve Visual Style Guide

**Version:** 1.1  
**Last updated:** 2026-05-06  
**Purpose:** AI-executable photography standards and image generation instructions  

This document is the single source of truth for all Kay-Twelve visual content. It is read at runtime by the photo generation skill to make shot selection, schema construction, and workflow routing decisions. It also serves as a human-readable reference for photographers, agency partners, and the Kay-Twelve team.

---

## 1. Scope & Platforms

This guide governs all photography and AI-generated imagery for Kay-Twelve across these channels:

- LinkedIn (feed posts + article covers)
- Instagram (feed + Stories/Reels)
- Facebook (feed posts)
- HubSpot newsletter headers
- Blog featured images
- Website hero images

Every image produced under this guide must be usable across multiple platforms from a single generation run.

---

## 2. Brand Visual Identity (Overlay Reference)

These values are for social card overlays and branded compositions — not for photography direction. Photos should never contain brand colors as dominant elements.

### Colors

| Role | Name | Hex |
|---|---|---|
| Primary | Pelorous Blue | `#208ca9` |
| Secondary | Yellow | `#ffb41f` |
| Secondary | Shady Lady Gray | `#9e9ca4` |
| Secondary | Pink | `#ea5581` |
| Secondary | Chelsea Cucumber Green | `#88a95b` |

### Fonts

| Font | Usage |
|---|---|
| Edo | Headlines / display |
| Source Sans Pro | Subheadings / body |
| Helvetica | Body / general use |

---

## 3. Photography Philosophy

Every Kay-Twelve image must embody these principles:

**Human-first.** Students are always the emotional center. Spaces tell stories through people, not furniture catalogs. Even wide establishing shots should imply human activity.

**Natural light.** Never flash. Light should feel warm, even, and real — not studio-staged. All approved reference photos use ambient/natural light exclusively (confirmed via EXIF analysis).

**Clean but lived-in.** Spaces feel usable and real. Slight imperfections add authenticity. Never sterile, never cluttered.

**Storytelling over product shots.** Every image should answer "what is learning like here?" — not "what furniture is this?"

**Two types of image serve different purposes.** Wide shots establish context and show the space as a whole. Close shots capture individual moments of engagement. Both are needed across the content calendar.

---

## 4. Good vs. Bad — Annotated Examples

### Good Examples

| File | Label | Rules Demonstrated |
|---|---|---|
| `examples/photos/example-1.jpg` | Innovative open space, students present | Wide sightlines; natural light from windows; students in background, not posed |
| `examples/photos/example-2.jpg` | Student interacting creatively with furniture | Subject centered in furniture; expression engaged not performative; minor distractors acceptable if croppable |
| `examples/photos/example-3.jpg` | Fresh modern environment, minimal clutter | Non-traditional layout; color and material choices visible; no visual noise |
| `examples/photos/example-4.jpg` | Action shot, high-res, considered framing | Crisp professional resolution; authentic interaction; lighting and framing deliberate |
| `examples/photos/example-5.jpg` | Collaboration, energy and purpose | Students facing activity not camera; texture and spatial design visible; not posed |

### Bad Examples

| File | Label | Rules Violated |
|---|---|---|
| `examples/bad/ex-1.jpg` | Grainy, cluttered background | Resolution too low; wall decor creates visual noise; furniture not the hero |
| `examples/bad/ex-2.jpg` | No students, cluttered, not a wide shot | No human presence; cluttered shelves dominate; composition too tight |
| `examples/bad/ex-3.JPG` | Distracting elements | Water bottles visible; student with shirt over head; too much happening |
| `examples/bad/ex-4.jpg` | No students, worn space, clutter | No human presence; tape on floor; overflowing storage; doesn't read as "new space" |

### Environment Photos (Inpainting Base Images)

The `examples/environment/` folder contains 5 clean professional shots of real Kay-Twelve spaces. These are intended as `input_image` references for the inpainting workflow (Workflow B). Do not use as-is for social content without student insertion.

---

## 5. Shot Archetypes

Every image generated must map to one of three named archetypes. The photo generation skill identifies the correct archetype from the user's brief before constructing any schema.

---

### The Space

Wide establishing shot. Shows the full learning environment — architecture, furniture layout, sightlines, natural light. Students may be present but are secondary to the space itself.

**Best for:** Project reveals, website hero images, LinkedIn article covers, newsletter headers.

**Composition rules:**
- Shoot from corner or doorway perspective to maximize sightlines
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
    "focus_point": "whole_scene",
    "logo_safe_zone": {
      "region": "bottom_left",
      "width_percent": 25,
      "height_percent": 35,
      "instruction": "Keep all primary subjects, faces, and focal content clear of this region. The Kay-Twelve logo badge overlay is applied here in post-processing."
    }
  },
  "scene": {
    "lighting": { "type": "natural_sunlight", "direction": "front_lit" }
  }
}
```

---

### The Moment

Close action shot. One student engaged with the space — reading, writing, thinking, interacting with furniture in a creative or unexpected way. Authentic, never posed. Background softly blurred.

**Best for:** Instagram primary feed posts, storytelling moments, blog in-article images, email headers.

**Composition rules:**
- Subject centered, generous headroom above
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
    "focus_point": "face",
    "logo_safe_zone": {
      "region": "bottom_left",
      "width_percent": 25,
      "height_percent": 35,
      "instruction": "Keep all primary subjects, faces, and focal content clear of this region. The Kay-Twelve logo badge overlay is applied here in post-processing."
    }
  },
  "scene": {
    "lighting": { "type": "natural_sunlight", "direction": "side_lit" }
  }
}
```

---

### The Collaboration

Group shot showing 2–4 students working together — discussion, problem-solving, hands-on activity. Space is equally important as students. Energy and purpose over posed arrangement.

**Best for:** LinkedIn feed, Facebook posts, newsletter features, blog headers, project case studies.

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
    "focus_point": "whole_scene",
    "logo_safe_zone": {
      "region": "bottom_left",
      "width_percent": 25,
      "height_percent": 35,
      "instruction": "Keep all primary subjects, faces, and focal content clear of this region. The Kay-Twelve logo badge overlay is applied here in post-processing."
    }
  },
  "scene": {
    "lighting": { "type": "natural_sunlight", "direction": "front_lit" }
  }
}
```

---

## 6. Composition & Safe Zone Rules

Every image must work across multiple aspect ratios from a single generation. Composition must be center-weighted with safe zones on all sides.

**The multi-ratio challenge:**
- 16:9 is very wide (1200×630)
- 4:5 is taller than square (1080×1350)
- The same image must crop cleanly to both extremes

**Safe zone requirements:**
- Primary subject centered horizontally
- Minimum 15% clear space above subject's head
- Minimum 10% clear space below subject's feet
- No critical content within 10% of any edge
- No text or logos in subject area (reserved for social card composition)

**Nano Banana implications:**
- Always specify `position: "center"` for primary subjects
- Prefer `medium_shot` over `close_up`, `wide_shot` over `medium_shot` for multi-subject scenes
- Set `focus_point: "whole_scene"` for The Space and The Collaboration archetypes

### Logo Overlay Safe Zone

Every image — both fully generated (Workflow A) and inpainted (Workflow B) — must reserve the **bottom-left corner** for the Kay-Twelve logo badge, which is applied in a separate post-processing step.

**Protected region:** Bottom-left **25% of width × 35% of height**

This applies to all aspect ratios (1:1, 4:5, 16:9, 9:16). The badge is circular and positioned in the corner, so keep the entire rectangular region clear — not just the circle footprint.

**What must not appear in the protected region:**
- Faces or heads
- Hands engaged in activity
- Focal subjects or primary action
- Text, signage, or readable environmental content

**What is acceptable in the protected region:**
- Floor, background furniture, open space, architectural elements
- Out-of-focus background content

**Nano Banana schema implementation:** Add `logo_safe_zone` to the `composition` object (see Section 5 archetype defaults). Also include safe zone negative prompts from Section 7 in every schema.

---

## 7. Universal Negative Prompts

Applied to every Nano Banana generation regardless of archetype. Include in `advanced.negative_prompt`:

```json
[
  "clutter", "storage overflowing", "tape on floor", "water bottles",
  "trash can", "fan", "paper piles", "boxes", "worn furniture",
  "posed smile at camera", "looking at camera", "smiling at camera",
  "low quality", "grainy", "blurry", "blur", "noise", "grain",
  "watermark", "text", "logo", "bad hands", "extra fingers", "mutated",
  "flash photography", "harsh lighting", "studio lighting", "artificial light",
  "catalog style", "sterile", "empty showroom", "product photography",
  "cropped", "worst quality", "distorted",
  "primary subject in bottom left corner", "face in bottom left corner",
  "focal point in bottom left", "key action in bottom left quadrant",
  "student face in bottom left", "hands in bottom left corner"
]
```

> **Safe zone prompts note:** The last six entries above enforce the logo overlay safe zone (see Section 6). They are required on every schema, including inpainting runs.

---

## 8. Platform & Aspect Ratio Guide

### Standard Full Run — 3 Files

| Ratio | Nano Banana Value | Covers |
|---|---|---|
| 1:1 | `"1:1"` | LinkedIn feed, Facebook feed, Instagram square |
| 4:5 | `"4:5"` | Instagram primary feed (max reach), Facebook feed |
| 16:9 | `"16:9"` | Blog featured image, HubSpot newsletter header, LinkedIn article |

### On-Demand Only

| Ratio | Nano Banana Value | Covers |
|---|---|---|
| 9:16 | `"9:16"` | Instagram Stories, Reels (generate only when explicitly requested) |

### Platform-to-Ratio Mapping

When the user requests specific platforms, map as follows:
- **"LinkedIn"** → 1:1 + 16:9
- **"Instagram"** → 4:5 + 1:1
- **"Facebook"** → 1:1 + 4:5
- **"newsletter"** or **"blog"** → 16:9
- **"Stories"** or **"Reels"** → 9:16
- De-duplicate: if LinkedIn + blog both requested, generate 16:9 once

### Output File Naming Convention

```
[brief-slug]_[archetype]_[ratio].jpg
```

Examples:
- `library-collab_collaboration_4x5.jpg`
- `reading-nook_moment_1x1.jpg`
- `open-commons_space_16x9.jpg`

---

## 9. Nano Banana Schema Defaults

Full drop-in templates for each archetype live in Section 5. At generation time:

1. Select the archetype template from Section 5
2. Replace `{{PLATFORM_RATIO}}` with the target ratio from Section 8
3. Fill `subject` array from the user's brief (student descriptions, age, clothing, pose, expression)
4. Fill `scene.location` from the brief (library, classroom, maker space, etc.)
5. Fill `scene.background_elements` with space-specific details
6. Append the full negative prompt array from Section 7 to `advanced.negative_prompt` (includes safe zone prompts)
7. Set `advanced.hdr_mode: true`
8. Set `advanced.magic_prompt_enhancer: false` (the style guide IS the enhancer)
9. Confirm `composition.logo_safe_zone` is present (inherited from archetype template — do not remove)

---

## 10. Workflow A — Full AI Generation

Step-by-step instructions for the photo generation skill:

1. **Read the user's brief** — what scene, how many students, what space type, which platforms
2. **Map platforms to ratios** using Section 8. Default: full run (1:1 + 4:5 + 16:9)
3. **Identify shot archetype** from brief content:
   - Two+ students working together → **The Collaboration**
   - Single student in an action/moment → **The Moment**
   - Emphasis on the space/environment → **The Space**
4. **Load base schema** for the identified archetype (Section 5)
5. **Fill subject array** — describe each student: approximate age, clothing, pose, expression. Use `"engaged"` or `"neutral"` expressions only. Never `"smiling"` at camera.
6. **Fill scene fields** — `location`, `background_elements` derived from the brief
6a. **Verify logo safe zone** — confirm `composition.logo_safe_zone` is present from the archetype template and that no subject `position` is set to `"left"` as a solo primary subject (which risks placement in the bottom-left region)
7. **Submit one schema at a time** — Nano Banana produces better results with individual submissions. Do not batch multiple ratios in a single run.
8. **Save the returned photo** to the output directory using naming convention from Section 8
9. **For the next ratio:** duplicate the schema, change only `meta.aspect_ratio`, and submit again
10. **Repeat** until all required ratios are covered

**Archetype mapping examples:**
- "two students working together at a flexible table" → The Collaboration
- "a student reading in the library corner" → The Moment
- "our new open learning commons" → The Space
- "students brainstorming around a whiteboard" → The Collaboration
- "a quiet moment in the reading nook" → The Moment

---

## 11. Workflow B — Inpainting (Student Insertion)

### When to Use

- A real Kay-Twelve environment photo exists (from `examples/environment/` or client-provided)
- The space is correct but lacks student presence
- The goal is to show the space as built, not a generic AI environment

### Decision Rules

- Real client project photo available AND space is the story → **Inpainting**
- Generic content (blog illustration, social post not tied to a specific project) → **Full Generation (Workflow A)**
- Client photo is cluttered or violates bad-example rules → **Full Generation** (do not inpaint into a bad space)

### Schema Differences from Workflow A

The environment photo is provided as a scene-level reference. The student(s) are composited into the existing space.

**Schema structure:**

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
    "location": "{{DERIVED_FROM_ENVIRONMENT_PHOTO}}",
    "input_image": {
      "path": "{{ENVIRONMENT_PHOTO_PATH}}",
      "usage_type": "style_transfer",
      "strength": 0.75
    }
  }
}
```

> **Implementation note:** Nano Banana's `input_image` is subject-scoped in the schema. For scene-level inpainting, `style_transfer` is used as the usage type to maintain the space's look and feel. If Nano Banana adds explicit scene-level `input_image` support in the future, update this section accordingly.

### Strength Settings

| Range | Effect | Use When |
|---|---|---|
| 0.85–1.0 | Tight to reference — student closely matches environment style | Final client deliverable, specific space must be recognizable |
| 0.60–0.75 | Loose — student feels natural, less tied to exact reference | General content, blog illustrations |
| Default | **0.75** | Start here and adjust if needed |

### Inpainting Step-by-Step

1. **Confirm environment photo** — verify it exists and doesn't violate bad-example rules
2. **Identify archetype** — typically The Moment (single student) or The Collaboration (group)
3. **Load archetype schema** from Section 5
4. **Add `scene.input_image`** with path to environment photo, `usage_type: "style_transfer"`, and strength
5. **Fill subject array** — student description, pose, expression. For single-student inpaints, set `position: "center"` — never `"left"`, which risks placement in the logo safe zone
5a. **Verify logo safe zone** — confirm `composition.logo_safe_zone` is present and safe zone negative prompts (Section 7) are included in `advanced.negative_prompt`
6. **Derive `scene.location`** from the environment photo content (describe what the space is)
7. **Map platforms to ratios** — determine all needed ratios
8. **Submit one schema at a time** to Nano Banana (one ratio per submission) and save each result to output directory
9. **Repeat** for each remaining ratio, changing only `meta.aspect_ratio`
10. **Report results** with note that inpainting was used and which environment photo was the base

---

## 12. Social Card Composition (Figma + Pencil)

> This section is handled by the **separate social card composition skill** and is included here for reference only. The photo generation skill produces raw photos; the card skill consumes them.

### Card Structure

Each social card contains these zones:

| Zone | Position | Notes |
|---|---|---|
| Photo layer | Full bleed | Raw photo from generation fills entire card |
| Color bar | Bottom 12% | Pelorous Blue `#208ca9`, 80% opacity overlay |
| Logo | Bottom-left, within color bar | Kay-Twelve wordmark, white |
| Text zone | Bottom-right, within color bar | Optional caption, Source Sans Pro, white |
| Safe frame | Inset 5% all sides | No critical content outside this frame |

### Figma Templates Required

- `kt-social-card-1x1.fig`
- `kt-social-card-4x5.fig`
- `kt-social-card-16x9.fig`

### Card Output Naming

```
[brief-slug]_card_[ratio].png
```
