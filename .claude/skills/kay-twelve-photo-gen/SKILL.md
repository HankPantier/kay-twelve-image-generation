---
name: kay-twelve-photo-gen
description: Brainstorm, plan, and generate Nano Banana image schemas for Kay-Twelve photography. Use this skill whenever the user asks to brainstorm images, generate photos, create image concepts, write Nano Banana schemas, or produce visual content for Kay-Twelve blog posts, social media, newsletters, or the website. Triggers on "brainstorm images", "generate a photo", "create an image for", "write a schema", "what should the image be", "image ideas for this post", or any request to produce visual content. This skill enforces the Kay-Twelve visual style guide at every step — brainstorm concepts are constrained to the three defined archetypes before any schema is written.
---

# Kay-Twelve Photo Generation Skill

You are generating photography concepts and Nano Banana schemas for Kay-Twelve, a nationwide Learning Space Integrator. Every image you produce must represent Kay-Twelve's brand: warm, human-first, student-centered, modern learning environments shot in natural light.

---

## MANDATORY FIRST STEP — Read the Style Guide

Before brainstorming a single concept, read the full visual style guide:

```
docs/kay-twelve-visual-style-guide.md
```

Do not propose any concepts until the style guide has been read. It is the source of truth for everything in this skill.

---

## The Three Archetypes — The Only Options

There are exactly three approved image archetypes. Every concept must map to one of them.

| Archetype | Camera | Lens | Aperture | ISO | Angle |
|---|---|---|---|---|---|
| **The Space** | Canon EOS R5 | 24mm | f/5.6 | 800 | eye_level |
| **The Moment** | Nikon Z 8 | 85mm | f/1.4 | 400 | eye_level |
| **The Collaboration** | Nikon Z 8 | 50mm | f/2.8 | 400 | eye_level |

Camera model, lens, aperture, ISO, and angle are **fixed per archetype**. They are never adjusted. Any concept that cannot be executed within these constraints is off-brand and must not be proposed.

### Platform → Archetype Quick Routing

Use this table to select the right archetype before brainstorming. When a platform is requested, map it here first.

| Requested output | Archetype |
|---|---|
| 16:9 newsletter header, blog featured image, LinkedIn article cover | **The Space** |
| 1:1 LinkedIn feed, Facebook feed, Instagram square | **The Collaboration** |
| 4:5 Instagram primary feed, Facebook (max reach) | **The Moment** or **The Collaboration** |
| 9:16 Instagram Stories / Reels | **The Moment** |
| Website hero image | **The Space** |
| Project reveal, case study header | **The Space** |
| In-article image, blog body image | **The Moment** |

When a brief asks for multiple ratios, lead with the archetype best suited to the primary platform, then confirm it crops cleanly to the others.

**Locked values (all archetypes):**
- `film_stock`: Kodak Portra 400
- `angle`: eye_level — NEVER bird_eye_view, high_angle, overhead, drone_view, or any non-eye-level angle
- `lighting.type`: natural_sunlight only
- `lighting.direction`: front_lit (The Space, The Collaboration) · side_lit (The Moment)
- `magic_prompt_enhancer`: false
- `hdr_mode`: true

> **If colors come out flat or neutral:** Do NOT change the film stock — it is locked. The fix is always stronger color language in the prompt: name every furniture color explicitly in `scene.location` AND `scene.background_elements`, end the location string with a palette declaration ("The overall palette is bold and vibrant — teal, lime green, cobalt blue, yellow — not grey, not beige, not neutral"), and add color-specific negative prompts such as "grey metal furniture", "beige furniture", "neutral colored chairs", "muted colors", "desaturated". Overriding the film stock breaks archetype compliance and is not an approved fix.

---

## Brainstorm Rules

Before presenting any concepts:

1. Read the article or brief fully
2. Identify the emotional core — what is this content about at a human level?
3. Generate 3–5 concepts that express that core through Kay-Twelve spaces
4. Map every concept to an archetype before presenting it
5. Label every concept with its archetype

**Every concept must:**
- Show a modern, student-centered learning environment
- Have students present (The Space: 3–5 across zones for an active, lived-in feel — see diversity table below)
- Show spaces that look like Kay-Twelve spaces: natural light, flexible furniture, open sightlines

**Never propose:**
- Empty rooms with no students
- Object-only concepts (just furniture, just a whiteboard, etc.)
- Traditional rows of fixed desks — these represent what Kay-Twelve replaces
- Dark or dim institutional corridors, hallways, or spaces
- High-angle, overhead, or drone-style shots — eye_level only
- Spaces that read as institutional, dated, or low-quality
- Concepts requiring a lens not in the three archetypes (no 35mm, no 50mm for The Space, etc.)

**Brand gut-check before presenting:** Ask — "Does this image show what learning looks and feels like in a Kay-Twelve space?"

---

## Schema Requirements — Every Schema

When building a Nano Banana schema:

1. Start from the archetype template in **Section 5** of the style guide — never build from scratch
2. Replace `{{PLATFORM_RATIO}}`, `{{STUDENT_DESCRIPTION}}`, `{{SCENE_DESCRIPTION}}` with brief-specific content
3. Confirm every mandatory field is present before submitting

### Mandatory fields checklist

**Technical block — fixed per archetype:**
- `camera_model` — Canon EOS R5 (Space) or Nikon Z 8 (Moment/Collaboration)
- `lens` — 24mm / 85mm / 50mm
- `aperture` — f/5.6 / f/1.4 / f/2.8
- `iso` — 800 / 400 / 400
- `film_stock` — Kodak Portra 400

**Composition block — required on every schema:**
```json
"logo_safe_zone": {
  "region": "bottom_left",
  "width_percent": 25,
  "height_percent": 35,
  "instruction": "Keep all primary subjects, faces, and focal content clear of this region. The Kay-Twelve logo badge will be composited here in post-production."
}
```

**Advanced block — required on every schema:**
- `magic_prompt_enhancer`: false
- `hdr_mode`: true
- `steps`: 50
- `guidance_scale`: 7.5
- `negative_prompt`: full Section 7 list (see below)

### Full assembled negative prompt — copy this into every schema

This is the complete list combining the universal base, safe zone enforcement, color drift prevention, and stock photo prevention. Copy it in full every time — no assembly required.

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
  "student face in bottom left", "hands in bottom left corner",
  "grey metal furniture", "beige furniture", "neutral colored chairs",
  "brown wood tones", "muted colors", "desaturated", "washed out",
  "grey stools", "silver chairs", "traditional rows of desks",
  "chalkboard", "cluttered walls", "dim lighting",
  "stock photo", "staged", "overly posed",
  "drop ceiling", "acoustic ceiling tiles", "fluorescent light panels",
  "HVAC vent", "ceiling vent", "air conditioning vent",
  "plain vinyl floor", "linoleum floor", "bare concrete floor",
  "beige walls", "cream walls", "bare walls", "plain white walls",
  "bookcase", "bookshelf", "wooden door frame", "institutional door",
  "empty lounge chairs", "unoccupied seating zone"
]
```

> The first 35 entries are universal quality/content guards. Entries 36–41 enforce the logo safe zone. Entries 42–54 prevent color palette drift. Entries 55–57 prevent stock photo aesthetics. Entries 58–70 prevent institutional architecture defaults (ceiling tiles, vinyl floors, bare walls, bookshelves) — learned from real generation runs. All are required on every schema.

---

## Scene Architecture Defaults — The Space archetype

Gemini defaults to institutional school architecture (drop ceilings, vinyl floors, bare beige walls) unless explicitly overridden. For every Space schema, include all three of the following in `scene.location` AND `scene.background_elements`:

**Wall graphic (required):**
> "One full wall features a bold designed graphic — vertical multicolor stripes in teal, cobalt blue, lime green, and yellow running floor to ceiling — a signature architectural element, not wallpaper, not a thin stripe"

**Floor (required):**
> "Patterned carpet tile in navy blue and dark teal — NOT plain vinyl, NOT linoleum"

**Ceiling (required):**
> "Modern ceiling with clean recessed lighting — NOT drop ceiling acoustic tiles, NO visible HVAC vents, NO fluorescent panel strips"

Writing all three in both `scene.location` and `background_elements` is the single biggest factor in whether the result reads as a Kay-Twelve space or a generic school building.

---

## Subject and Scene Rules

**Expressions:**
- Neutral or engaged — student absorbed in work
- Never smiling at camera, looking at camera, or performing for the lens

**Subject positioning:**
- Never place a solo primary student at `position: "left"` — the bottom-left is the logo safe zone
- Solo subjects: `position: "center"` always
- Groups: `position: "center"` for the group as a whole

**Diversity formula — required per archetype:**

| Archetype | Student count | Minimum diversity requirement |
|---|---|---|
| The Space | 3–5 across visible zones | Place at least one student in each visible zone — collaboration table, lounge zone, standing work zone. At least two must be students of color (Black, Hispanic, or Asian). |
| The Moment | 1 | Rotate across Black, Hispanic, Asian, and white across the content calendar. Never default to white. |
| The Collaboration | 2–4 | Always include at least 2 different ethnicities. At least one student must be Black, Hispanic, or Asian. |

Always specify ethnicity and skin tone explicitly in the `description` field of every subject object. Without this, the model defaults to all-white subjects.

**Scene locations:**
- Modern K-12 learning environments only
- Library commons, flexible classrooms, maker spaces, open collaboration areas
- Never: hallways, cafeterias, traditional lecture halls, dark institutional spaces

**Lighting:**
- `natural_sunlight` only
- Never: `back_lit`, `under_lit`, `artificial_light`, `studio_light`, `flash`

---

## Workflow

### Step 1 — Brainstorm (always before any schema)
1. Read the article or brief
2. Read `docs/kay-twelve-visual-style-guide.md`
3. Generate 3–5 labeled concepts (archetype + concept description)
4. Present to user for selection

### Step 2 — Composition sketch + schema (present together in one pass)

Once the user selects a concept, present both in a single response:
1. **Composition sketch** — describe subject placement, zone layout, lighting direction, and how the logo safe zone is kept clear. This gives the user something to react to before the JSON.
2. **Schema** — immediately follow with the full Nano Banana JSON built from the archetype template.

Do not pause between the sketch and the schema to wait for approval — present them together. The user can request schema changes after seeing both.

### Step 3 — Build schema
- Load archetype template from Section 5 of style guide
- Fill subject, scene, and meta fields from brief
- Run mandatory fields checklist before finalizing

### Step 4 — Confirm compliance
Before submitting to Nano Banana, verify:
- [ ] `logo_safe_zone` present in composition block
- [ ] Full 35-entry negative prompt list present
- [ ] Correct camera/lens/aperture/ISO for archetype
- [ ] `angle: eye_level`
- [ ] `magic_prompt_enhancer: false`
- [ ] `hdr_mode: true`
- [ ] No primary subject in bottom-left position

### Step 5 — Submit and save
- Submit one schema per ratio (do not batch multiple ratios in one call)
- Save output: `[brief-slug]_[archetype]_[ratio].jpg`
  - Example: `ribbon-cutting-blog_space_16x9.jpg`

---

## Platform Ratios

| Ratio | Use for |
|---|---|
| 16:9 | Blog featured image, newsletter header, LinkedIn article |
| 1:1 | LinkedIn feed, Facebook feed, Instagram square |
| 4:5 | Instagram primary feed, Facebook (max reach) |
| 9:16 | Instagram Stories/Reels — on demand only |

Standard blog run = 16:9 only unless other platforms requested.

---

## Common Mistakes — Do Not Repeat

| Mistake | Correct behavior |
|---|---|
| Proposing overhead/drone concept | Angle is always eye_level — no overhead concepts |
| Using 35mm lens | Only 24mm (Space), 85mm (Moment), 50mm (Collaboration) |
| `back_lit` or `side_lit` on The Space | The Space is always `front_lit` |
| Omitting `logo_safe_zone` | Required on every single schema |
| Partial negative prompt list | Use the full 35-entry list every time |
| Solo student positioned at `left` | Use `center` — bottom-left is the logo zone |
| Empty room concept with no students | Students are always present |
| Traditional classroom rows | Kay-Twelve replaces traditional — never show rows |
| Brainstorming before reading style guide | Read the style guide first, always |
