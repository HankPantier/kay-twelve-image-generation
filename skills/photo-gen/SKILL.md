# Kay-Twelve Photo Generation Skill — v3.0

**Version:** 3.0 | **Updated:** 2026-05-26
**Purpose:** Turn blog posts and social copy into on-brand Kay-Twelve image schemas for Nano Banana / Gemini generation. Claude handles all technical decisions silently — the user sees only concepts and JSON.

---

## How This Works

1. User shares post copy or topic
2. Claude brainstorms 2–3 scene concepts (one paragraph each) → user picks one
3. Claude runs the Auto-Selection Engine (Part 2) silently → builds and delivers the schema
4. User generates in Gemini
5. Claude updates the brief log

The user never sees palette names, ceiling types, or Brief Card approval prompts.

---

## Part 1: Post-to-Concepts (User-Facing Workflow)

### Step 1 — Read the post
Identify: the central idea, the emotional register (inspiring? practical? transformative?), the likely audience (educators, administrators, designers), and the platform the image is for.

### Step 2 — Generate 2–3 scene concepts
Each concept is a short paragraph covering:
- **What's happening** — the specific human moment in the frame
- **Who's in it** — student age group, rough count, energy
- **Where** — room type described in plain language (no RT codes)
- **Platform** — which output ratio/format this serves

Format each concept plainly, like a creative brief to a photographer. No palette names, no ceiling type codes, no schema language.

**Example concept format:**
> **Concept 1 — The Reggio Moment (16:9 blog header)**
> A flexible classroom mid-reconfiguration — a cluster of four high school students pulling cloud-shaped tables into a new group arrangement, mid-motion and absorbed in the task. Furniture is distinctly modern and non-institutional. The space has just been transformed by the students themselves. Wide shot showing the full classroom and the group in the middle distance.

### Step 3 — User picks a concept
User replies with their choice (or requests a tweak). Once a concept is confirmed, Claude proceeds directly to schema generation — no further questions asked.

---

### Platform → Archetype Routing

| Platform / Output | Archetype | Ratio |
|---|---|---|
| Blog header, newsletter, LinkedIn article cover | The Space | 16:9 |
| Website hero, project reveal, case study header | The Space | 16:9 |
| LinkedIn feed, Facebook feed, Instagram square | The Collaboration | 1:1 |
| Instagram primary feed, Facebook max-reach | The Moment or The Collaboration | 4:5 |
| Instagram Stories / Reels | The Moment | 9:16 |

---

## Part 2: Auto-Selection Engine (Internal — Run Silently)

After the user picks a concept, run this engine to lock all technical dimensions before writing the schema. Do not surface these choices to the user unless asked.

### Step 2A — Check the brief log
Read `briefs/brief-log.json`. Note the last 2 entries for: `room_type`, `palette`, `wall_treatment`, `floor`, `student_ages`, `ethnicities`. No dimension repeats its value from the last 2 entries.

### Step 2B — Lock Room Type
Match the concept's plain-language room description to the closest RT in Part 3C. Must differ from last 2 briefs.

### Step 2C — Lock Palette
Choose from A–F (Part 3D). Must differ from last 2 briefs. Palette A (Classic) is overused — avoid until B–F have each appeared at least once.

### Step 2D — Lock Floor
Choose from the valid options for this room type (Part 3I). Must differ from last 2 briefs.

### Step 2E — Lock Wall Treatment
Choose from Part 3G. Must differ from last brief. Stripe is maximum once every 3–4 briefs.

### Step 2F — Lock Ceiling
Choose from Part 3H based on room type and archetype:
- The Space (wide shot, open commons, library): Dramatic Geometric or Drum Pendants
- Flexible classroom / STEM lab: Standard Clean or Drum Pendants
- Maker / Project room: Open Structural
- Any: Drum Pendants are always safe

### Step 2G — Select Signature Furniture Piece
Choose at least one from Part 3B. For The Space archetype: include 2–3 distributed across zones. Vary which pieces appear across briefs.

### Step 2H — Lock Student Composition
- Match count to archetype (Part 3A)
- Match age to room type and concept
- Must differ from last 2 brief ethnicities mixes
- Always specify ethnicity + skin tone explicitly

### Step 2I — Record Decisions (internal only)
Mentally note: palette, room_type, wall_treatment, ceiling, floor, signature_piece, student_ages, ethnicities. These populate the schema and the brief log entry. Do not present as a card for approval.

---

## Part 3: Brand Library (Reference)

---

### Part 3A — Archetypes

Three archetypes. Camera specs are locked per archetype — never adjust them.

| Archetype | Camera | Lens | Aperture | ISO | Lighting |
|---|---|---|---|---|---|
| **The Space** | Canon EOS R5 | 24mm | f/5.6 | 800 | front_lit |
| **The Moment** | Nikon Z 8 | 85mm | f/1.4 | 400 | side_lit |
| **The Collaboration** | Nikon Z 8 | 50mm | f/2.8 | 400 | front_lit |

**Locked across all archetypes:**
- `film_stock`: Kodak Portra 400
- `angle`: eye_level — never overhead, drone, bird's eye, or high angle
- `magic_prompt_enhancer`: false
- `hdr_mode`: true
- `guidance_scale`: 7.5
- `steps`: 50

**Student count by archetype:**
| Archetype | Count Range |
|---|---|
| The Moment | 1 (single student only) |
| The Collaboration | 2–6 |
| The Space | 3–12 (crowd shots allowed — fill the space) |

---

### Part 3B — Signature Furniture Vocabulary

Every schema must include at least ONE of these pieces. They are what makes an image read as Kay-Twelve — not just the color palette. Reference the schema language in both `scene.location` and `scene.background_elements`.

---

**SP-1 — Circular Reading Pod**
A freestanding large circular tube-shaped reading nook — approximately 4 feet in diameter, about 5 feet tall. The exterior shell is one bold color; the interior lining is a contrasting color. A student can lie or sit fully inside the pod. This is the single most recognizable Kay-Twelve piece.

*Schema language:*
> `"A freestanding large circular tube reading pod — [DOMINANT COLOR] exterior shell with a [ACCENT COLOR] interior lining — a student sits or lies curled inside reading, fully enclosed in the circular nook"`

*Palette adaptation: match exterior to VISUAL DOMINANT color, interior to contrast accent.*

---

**SP-2 — Circular Portal Wall Partition**
A tall freestanding partition structure (approx. 7–8 feet tall, 10–12 feet wide) of warm maple or light birch wood panel, with multiple large circular portal openings cut through at varied heights. Each portal lined with upholstered acoustic felt in navy blue or sage green. Students sit inside the larger portals.

*Schema language:*
> `"A tall freestanding maple wood panel partition with multiple large circular portal cutouts at varied heights — each portal lined in upholstered navy blue or sage green acoustic felt — students visible inside the larger portals for focused individual reading"`

---

**SP-3 — Curved Serpentine Banquette Seating**
A large curved low seating element — crescent, arc, or S-curve — upholstered in a single bold palette color. Approximately 12–18 feet of continuous curved low seating. Students sit along the arc with a low table in the inner curve.

*Schema language:*
> `"A large [COLOR] curved serpentine low banquette — a bold crescent-shaped continuous upholstered seating arc with a low circular table nested in the inner curve"`

---

**SP-4 — Circular Banquette Table**
A round pedestal dining table surrounded by a continuous curved upholstered bench seat forming a horseshoe or three-quarter arc in a bold palette color. Used in cafeteria / dining commons.

*Schema language:*
> `"Round pedestal tables with continuous curved horseshoe bench seating upholstered in [COLOR] — the curved banquette wraps three-quarters around each round table"`

---

**SP-5 — Organic / Cloud-Shaped Collaborative Table**
Large collaborative tables with non-rectangular organic tops — amoeba, cloud, clover, or petal shapes. White laminate surface. Multiple chairs clustered around the organic perimeter.

*Schema language:*
> `"Large organic cloud-shaped or amoeba-shaped collaborative tables with white laminate tops — non-rectangular flowing forms allowing students to cluster around any point of the perimeter"`

---

**SP-6 — Hexagonal Modular Ottomans**
Upholstered ottomans in a regular hexagon shape, 16–18 inches across. Groups of 4–7 in a honeycomb arrangement. Can be pushed together or scattered. Colors: cobalt blue, lime green, sage/olive, yellow.

*Schema language:*
> `"A cluster of [COLOR] and [COLOR] hexagonal upholstered ottomans grouped in a loose honeycomb arrangement — modular upholstered hexagons in two contrasting colors"`

---

**SP-7 — Tiered Amphitheater Step Seating**
Built-in or modular tiered risers — two to four steps rising from floor level, each step generous depth (18–24 inches) for comfortable sitting. Steps upholstered in bold palette colors or made of maple/birch wood.

*Schema language:*
> `"Built-in tiered step seating — three maple wood risers stepping up from floor level, used as informal amphitheater seating — students seated across multiple steps"`

*Or for upholstered version:*
> `"Modular stepped tiered seating in [COLOR] and [COLOR] upholstered risers — students seated across three tiers of bold-colored step seating"`

---

**SP-8 — Cylindrical Wobble Stools**
Chunky cylindrical stools with a slightly convex/dome base that allows gentle rocking movement. Upholstered fabric on top and around the cylinder. Typically gray-blue, sage green, or navy. Used at round and organic tables, especially with younger students.

*Schema language:*
> `"Chunky cylindrical wobble stools with dome-shaped rocking bases — [COLOR] upholstered fabric-wrapped cylinders that allow gentle movement"`

---

### Part 3C — Room Type Library

Eight approved room types. Must differ from last 2 briefs.

---

**RT-1: Open Learning Commons**
Natural student count: 5–12 (Space), 2–4 (Collaboration), 1 (Moment)

*Scene location template:*
> "Large open K-12 learning commons with multiple distinct zones visible simultaneously — a standing-work zone with standing-height tables in the foreground, a lounge zone with a large curved serpentine banquette or cluster of hexagonal ottomans in the mid-ground, and a focused-work zone with organic cloud-shaped tables in the background. Wide open sightlines across all three zones. Generous ceiling height. Large windows on one full wall flooding the space with natural light. Students are distributed across all visible zones in active use."

---

**RT-2: Flexible Classroom**
Natural student count: 4–8 (Space/Collaboration), 1 (Moment)

*Scene location template:*
> "Modern K-12 flexible classroom — a traditional room footprint completely transformed. Furniture is grouped in clusters rather than rows: hexagonal ottomans and organic-shaped tables pushed together for group work, one cluster with cylindrical wobble stools at a round table. Large windows along one wall. Evidence of recent furniture reconfiguration — chairs mid-movement, tables angled — a space in active, adaptable use. The teacher zone is at the side, not the front."

---

**RT-3: Maker / Project Room**
Natural student count: 3–8

*Scene location template:*
> "Modern K-12 maker space and project room. Standing-height worktables with generous surfaces are the dominant furniture element. A project-in-progress is visible — a physical prototype, a large-format layout, assembled components, or design materials spread across the table surface. Students are in mid-build or mid-design activity. Pegboard or wall-mounted tool organization visible on one wall. The space feels purposeful, slightly raw, and built for making."

---

**RT-4: Library / Media Center**
Natural student count: 2–8

*Scene location template:*
> "Modern K-12 library and media center — not a traditional library. A circular reading pod nook in one zone where a student sits inside a large circular tube pod reading independently. Research tables with mobile seating in another zone. Warm natural light from a bank of windows. The space is warm, inviting, and purposeful — no traditional bookshelves dominating. Display panels, soft seating, and open sightlines replace the traditional stacks."

---

**RT-5: Early Childhood / PreK–2**
Natural student count: 4–10

*Scene location template:*
> "Bright, modern early childhood learning space for PreK through 2nd grade. All furniture is child-scaled — low round tables at seated-child height, cylindrical wobble stools and floor cushions, activity zones organized around the perimeter with open floor space in the center. A large area rug defines the main gathering space. One zone has a floor-level building activity. Another has a low table with art materials. Low wave-form rocking chairs or ramp seating visible at one zone."

---

**RT-6: Breakout / Small Group Pod**
Natural student count: 2–4

*Scene location template:*
> "Small K-12 breakout collaboration pod — a semi-enclosed space separated from the main commons by a partial glass wall or acoustic partition. The room holds 4–6 students maximum. Mobile seating is pulled into a tight working circle. A writable surface is on one wall. The space feels intentionally intimate and acoustically distinct from the larger open space visible through the glass beyond."

---

**RT-7: STEM / Science Lab**
Natural student count: 3–8

*Scene location template:*
> "Modern K-12 STEM learning lab with flexible furniture integrated alongside dedicated lab infrastructure. Standing-height lab benches with generous surfaces. A current experiment or investigation is underway — materials, observation tools, and recording sheets visible on the bench surface. Mobile seating pulled up to the benches. The space combines the rigor of a traditional science lab with the flexibility of a learning commons. Natural light from one side."

---

**RT-8: Cafeteria / Dining Commons**
Natural student count: 6–15

*Scene location template:*
> "Large K-12 cafeteria commons that doubles as a learning and social space. Circular banquette tables — round pedestal tables with continuous curved horseshoe bench seating upholstered in bold palette colors — are distributed across the floor. Hexagonal ottomans cluster near one wall as informal seating. The space is energetic, social, and visually busy in a positive way. Large open floor plan, high ceilings, natural light from clerestory or large windows."

---

### Part 3D — Color Palette Library

Six palettes. Must differ from last 2 briefs. Use only the designated palette's colors — do not introduce colors from other palettes. Each palette has a VISUAL DOMINANT — the color that must fill most seating surfaces.

---

**Palette A — Classic Kay-Twelve**
⚠️ OVERUSED — avoid until Palettes B–F have each appeared at least once.

**VISUAL DOMINANT: Teal + lime green.** Teal and lime green fill the majority of seating surfaces. The room reads green before any other color registers. Cobalt blue appears on accent chairs only (2–3 pieces max) — NOT the dominant color.

| Element | Color |
|---|---|
| Upholstered seating | Teal stools, lime green ottomans and lounge chairs |
| Tables | Bright yellow laminate tops, white metal frames |
| Accent chairs | Cobalt blue on casters — 2–3 max |
| Floor cushions | Lime green and teal |

*Acoustic panel colors: teal + lime green*

Palette-specific negatives:
```
"coral furniture", "orange furniture", "raspberry furniture", "magenta furniture",
"purple furniture", "violet furniture", "forest green furniture", "olive furniture"
```

---

**Palette B — Cobalt Dominant**

**VISUAL DOMINANT: Cobalt blue everywhere.** Cobalt blue fills every upholstered seating surface — stools, chairs, ottomans, banquettes. Yellow appears ONLY as floor cushions and small accent ottomans. The room reads unmistakably, saturatedly blue. No teal. No green. White tables only.

| Element | Color |
|---|---|
| Upholstered seating | Cobalt blue on every seating surface |
| Tables | White laminate tops, white metal frames — NO yellow table tops |
| Accent items | Bright yellow floor cushions and small ottomans only |

*Acoustic panel colors: cobalt blue + bright yellow*

Palette-specific negatives:
```
"teal furniture", "lime green furniture", "coral furniture", "orange furniture",
"raspberry furniture", "forest green furniture", "purple furniture",
"yellow table tops", "yellow laminate tables"
```

---

**Palette C — Warm Coral**

**VISUAL DOMINANT: Coral / orange-red.** Coral and orange-red fills the majority of upholstered seating. The room reads warm and energetic. Cobalt blue appears on 2–3 accent pieces MAX — the space must NOT read as a blue room.

| Element | Color |
|---|---|
| Upholstered seating | Coral and orange-red — dominant throughout |
| Tables | White laminate tops, white metal frames |
| Accent chairs | Cobalt blue — 2–3 max |
| Floor cushions | Cobalt blue and yellow — small accent only |

*Acoustic panel colors: coral + cobalt blue*

Palette-specific negatives:
```
"teal furniture", "teal chairs", "lime green furniture", "lime green chairs",
"raspberry furniture", "purple furniture", "forest green furniture",
"predominantly blue room", "mostly blue furniture"
```

---

**Palette D — Raspberry**

**VISUAL DOMINANT: Raspberry / magenta.** Raspberry and magenta fills the majority of upholstered seating. Bright yellow table tops make the second visual hit. Cobalt blue is a minor accent (2–3 chairs max) — the space must NOT read blue.

| Element | Color |
|---|---|
| Upholstered seating | Raspberry and magenta — dominant throughout |
| Tables | Bright yellow laminate tops, white metal frames |
| Accent chairs | Cobalt blue — 2–3 max |
| Floor cushions | Cobalt blue accents — small pops only |

*Acoustic panel colors: raspberry + bright yellow*

Palette-specific negatives:
```
"teal furniture", "teal chairs", "lime green furniture", "coral furniture",
"orange furniture", "forest green furniture", "purple furniture",
"predominantly blue room", "mostly blue furniture"
```

---

**Palette E — Purple + Yellow**

**VISUAL DOMINANT: Deep violet / purple.** Deep violet and purple fills the majority of upholstered seating. Bright yellow table tops make the second visual hit — a striking purple + yellow contrast. Cobalt blue is a minor accent (1–2 chairs max). Lime green cushions are tiny accent pops only.

| Element | Color |
|---|---|
| Upholstered seating | Deep violet and purple — dominant throughout |
| Tables | Bright yellow laminate tops, white metal frames |
| Floor cushions | Lime green — tiny accent pops only |
| Accent chairs | Cobalt blue — 1–2 max |

*Acoustic panel colors: deep violet + bright yellow*

Palette-specific negatives:
```
"teal furniture", "teal chairs", "coral furniture", "orange furniture",
"raspberry furniture", "magenta furniture", "forest green furniture",
"predominantly blue room", "mostly blue furniture"
```

---

**Palette F — Forest + Yellow**

**VISUAL DOMINANT: Warm olive / forest green.** Warm olive and forest green fills the majority of upholstered seating. The room reads earthy green. Bright yellow table tops make the second visual hit. Cobalt blue is strictly 1–2 accent chairs — the space must NOT read blue or teal.

| Element | Color |
|---|---|
| Upholstered seating | Warm olive and forest green — NOT teal, NOT blue-green — dominant throughout |
| Tables | Bright yellow laminate tops, white metal frames |
| Accent chairs | Cobalt blue — 1–2 max |
| Floor cushions | Bright yellow |

*Acoustic panel colors: forest green + bright yellow*

Palette-specific negatives:
```
"teal furniture", "teal chairs", "teal upholstery", "blue-green furniture",
"cyan furniture", "lime green furniture", "coral furniture", "raspberry furniture",
"purple furniture", "violet furniture",
"predominantly blue room", "mostly blue furniture"
```

---

### Part 3E — Students

**Always specify ethnicity AND skin tone explicitly in every subject description.** Without this, Gemini defaults to all-white subjects.

Ethnicity mixes to rotate through:
- Black + Hispanic + Asian
- White + Black + Hispanic
- Asian + Hispanic + White
- Black + White + Asian + Hispanic (4+ students)
- Black + Asian (2 students)
- Hispanic + White (2 students)
- Mixed elementary (4+ ethnicities, large group)

**Age ranges:**
| Label | Age | Furniture implications |
|---|---|---|
| Early childhood | 4–7 | Child-scaled furniture, floor activity |
| Elementary | 6–10 | Standard child furniture, table activity |
| Middle school | 11–13 | Teen-scale, varied energy |
| High school | 14–17 | Adult-scale, project-based work |
| Mixed | Span 2+ groups | Commons and large-group shots |

**Expression rules — always:**
- `"engaged"` or `"neutral"` only
- Never `"smiling at camera"`, `"looking at camera"`, or `"posed"`
- Students are absorbed in their activity, not aware of the photographer

**Position rules:**
- Solo primary subject: `position: "center"` — never `"left"` (bottom-left is logo safe zone)
- Groups: position group as a unit at `"center"`

---

### Part 3F — Activity Library

Do not repeat the same activity from the last 2 briefs.

| Activity | Best archetypes |
|---|---|
| Transitioning between zones (in motion) | Moment |
| Collaborative planning on large paper/surface | Collaboration |
| Building/assembling a physical prototype | Collaboration, Space |
| Independent writing or journaling | Moment |
| Small group discussion, gesturing | Collaboration |
| Presenting to peers | Collaboration, Space |
| Drawing or sketching in a sketchbook | Moment, Collaboration |
| Working with materials / experiment | Collaboration, Space |
| Reading independently inside a pod or nook | Moment |
| Peer tutoring (one explaining to another) | Collaboration |
| Brainstorming / sticky notes on surface | Collaboration |
| Floor activity (building, drawing, sorting) | Moment, Space (early childhood) |
| Measuring / recording data | Collaboration, Space |
| Art / mixed media creation | Moment, Collaboration |
| Large-group project spread across tables | Space, Collaboration |
| Using a writable wall or vertical surface | Collaboration |
| Eating and talking socially (cafeteria) | Space |
| Quiet parallel work in shared zone | Collaboration |

---

### Part 3G — Wall Treatment Library

Four options. Must differ from last brief. Stripe: maximum once every 3–4 briefs.

| Option | Schema language | Best room types |
|---|---|---|
| **stripe** | "One full wall — bold vertical multicolor stripes in teal, cobalt blue, lime green, and yellow, floor to ceiling — a signature architectural element, not wallpaper, not thin lines, not horizontal" | Open commons, flexible classrooms |
| **solid_feature** | "One full wall painted in deep solid cobalt blue — clean, bold, high-contrast, no pattern, no stripe" | Maker spaces, project rooms, breakout pods |
| **acoustic_panels** | "Wall covered in a flush-mounted grid of acoustic panels — [PALETTE COLOR 1] and [PALETTE COLOR 2] ONLY — modern sound mitigation elements in a regular repeating grid pattern — NOT a stripe pattern, NOT multicolor mixing, ONLY the two dominant colors from the active palette" | Classrooms, library, STEM labs |
| **graphic_panels** | "Large-format illustrated educational graphic panel on one wall — colorful, content-rich illustrated content, not a traditional bulletin board, not a chalkboard, not a whiteboard" | Library, early childhood, cafeteria |

**⚠️ Acoustic Panel Color Rule:** When using acoustic_panels, pick exactly TWO colors from the active palette and name them explicitly. The correct colors per palette are defined in Part 3D (listed as *Acoustic panel colors*). Never use the old "teal, lime green, cobalt blue, and yellow" mix — it bleeds into furniture colors and destroys palette identity.

---

### Part 3H — Ceiling Library

⚠️ **Always include a positive ceiling description.** Negative prompts alone cannot override Gemini's strong institutional classroom prior. See Part 6 for the Three-Field Repetition Rule.

**Ceiling A — Dramatic Geometric Acoustic**
*Use for: open commons hero shots, The Space archetype, flagship spaces*
> "Ceiling features a dramatic sculptural geometric acoustic system — dark charcoal angular folded panels creating a faceted, three-dimensional ceiling plane above the space — a striking architectural installation, NOT flat, NOT dropped tiles, NOT fluorescent panels"

**Ceiling B — Round Drum Pendant Lights**
*Use for: commons, library, social spaces, breakout areas — the most common Kay-Twelve ceiling*
> "Clean white ceiling with evenly spaced round drum pendant light fixtures — large circular fabric-wrapped pendant lights hanging at a uniform height, casting warm even light — a contemporary modern institutional ceiling with no visible fluorescent grid or ceiling tile grid lines"

**Ceiling C — Standard Clean Acoustic**
*Use for: classrooms, STEM labs, instructional spaces where ceiling should recede*
> "Clean white suspended acoustic ceiling with recessed LED lighting — a modern, well-maintained ceiling, NOT institutional yellow-beige ceiling tiles, NOT fluorescent tubes — clean, bright, and contemporary"

**Ceiling D — Open Structural**
*Use for: maker spaces and project rooms ONLY*
> "Open structural ceiling — exposed building systems and ductwork painted black above — the raw industrial ceiling of a true maker space, with pendant work lights hanging from the structure"

---

### Part 3I — Floor Library

⚠️ **Floor type must differ from the last 2 briefs.** Check `briefs/brief-log.json` before selecting. Always include floor description in both `scene.location` AND `scene.background_elements`.

Rooms marked *(fixed)* have only one appropriate floor — note the repetition in the log but do not force a variation that doesn't fit the space.

| Room Type | Floor Options |
|---|---|
| RT-1 Open Commons | navy_forest_carpet · light_wood_vinyl · polished_concrete |
| RT-2 Flexible Classroom | navy_forest_carpet · light_wood_vinyl · gray_carpet |
| RT-3 Maker / Project Room | polished_concrete *(fixed)* |
| RT-4 Library / Media Center | light_wood_vinyl · navy_forest_carpet |
| RT-5 Early Childhood | organic_rug *(fixed)* |
| RT-6 Breakout Pod | gray_carpet *(fixed)* |
| RT-7 STEM Lab | polished_concrete · light_wood_vinyl |
| RT-8 Cafeteria / Commons | polished_concrete · light_wood_vinyl |

**Floor descriptions:**

*navy_forest_carpet:*
> "Patterned carpet tile in navy blue and deep forest green — alternating geometric blocks in a grid pattern creating a rich two-color geometric floor — bold, not neutral, not plain"

*polished_concrete:*
> "Polished light gray concrete floor — seamless, smooth, slightly reflective — a clean contemporary floor appropriate for this type of learning space, NOT bare unfinished concrete, NOT stained"

*light_wood_vinyl:*
> "Light warm wood-look luxury vinyl plank flooring — pale oak or blonde wood tone, smooth, clean, contemporary — NOT dark wood, NOT laminate pattern, NOT parquet"

*organic_rug:*
> "A large circular or organic-shaped area rug with a soft neutral pattern — beige, warm gray, and cream tones — defining the main gathering zone of the early childhood space"

*gray_carpet:*
> "Medium gray heathered carpet tile — a textured monochromatic carpet in warm gray — NOT navy, NOT patterned, a quiet floor that lets the furniture color read"

---

## Part 4: Schema Assembly (Internal)

Run steps in order. Do not make new dimension choices during assembly — all choices were locked in Part 2.

### Step 4A — Load archetype template

```json
{
  "meta": {
    "aspect_ratio": "{{RATIO}}",
    "quality": "ultra_photorealistic",
    "notes": "Palette {{X}} — {{NAME}}. Room: {{ROOM_TYPE}}. Wall: {{WALL_TREATMENT}}. Ceiling: {{CEILING_TYPE}}. Floor: {{FLOOR_TYPE}}. Signature: {{SIGNATURE_PIECE}}."
  },
  "technical": {
    "camera_model": "{{PER ARCHETYPE}}",
    "lens": "{{PER ARCHETYPE}}",
    "aperture": "{{PER ARCHETYPE}}",
    "iso": "{{PER ARCHETYPE}}",
    "film_stock": "Kodak Portra 400"
  },
  "composition": {
    "framing": "{{PER ARCHETYPE}}",
    "angle": "eye_level",
    "focus_point": "{{PER ARCHETYPE}}",
    "logo_safe_zone": {
      "region": "bottom_left",
      "width_percent": 25,
      "height_percent": 35,
      "instruction": "Keep all primary subjects, faces, and focal content clear of this region. The Kay-Twelve logo badge overlay is applied here in post-processing."
    }
  }
}
```

**Archetype quick-reference:**

The Space:
```json
"technical": { "camera_model": "Canon EOS R5", "lens": "24mm", "aperture": "f/5.6", "iso": "800", "film_stock": "Kodak Portra 400" },
"composition": { "framing": "wide_shot", "angle": "eye_level", "focus_point": "whole_scene" }
```

The Moment:
```json
"technical": { "camera_model": "Nikon Z 8", "lens": "85mm", "aperture": "f/1.4", "iso": "400", "film_stock": "Kodak Portra 400" },
"composition": { "framing": "medium_shot", "angle": "eye_level", "focus_point": "face" }
```

The Collaboration:
```json
"technical": { "camera_model": "Nikon Z 8", "lens": "50mm", "aperture": "f/2.8", "iso": "400", "film_stock": "Kodak Portra 400" },
"composition": { "framing": "full_body", "angle": "eye_level", "focus_point": "whole_scene" }
```

---

### Step 4B — Write subject array
- One object per student
- Include: type, description (ethnicity + skin tone + clothing + activity), pose, expression, position
- Ethnicity and skin tone explicit in every description — no exceptions
- Expression: "engaged" or "neutral" only

### Step 4C — Write scene.location
Combine in this order:
1. Room type template from Part 3C (adapt to the specific brief)
2. Signature furniture piece from Part 3B (name and describe explicitly)
3. Palette furniture colors named explicitly from Part 3D (VISUAL DOMINANT named first)
4. Wall treatment description from Part 3G (with palette-correct acoustic panel colors if applicable)
5. Floor description from Part 3I (room-type matched)
6. Ceiling description from Part 3H
7. Close with explicit palette declaration: *"The overall palette is [VISUAL DOMINANT COLOR] with [ACCENT COLOR] and [ACCENT COLOR] — not grey, not beige, not neutral."*

### Step 4D — Write scene.background_elements
Repeat each of these as a dedicated array entry — apply Three-Field Repetition Rule from Part 6:
- Signature furniture piece (second reference)
- Palette furniture colors
- Wall treatment (with negations)
- Ceiling (with negations)
- Floor (with negations)

Repetition across both scene fields is the single most effective technique for getting Gemini to honor architectural and color choices.

### Step 4E — Write scene.lighting
```json
"lighting": { "type": "natural_sunlight", "direction": "{{per archetype}}" }
```
Space + Collaboration: `front_lit` | Moment: `side_lit`

### Step 4F — Write style_modifiers
```json
"style_modifiers": {
  "mood": "{{brief-specific mood}}",
  "artist_reference": ["Iwan Baan", "architectural editorial photography"],
  "aesthetic": "Kodak Portra 400 film grain, warm tones, authentic candid documentary — not stock photography"
}
```

### Step 4G — Append negative prompt block (Part 5)
### Step 4H — Verify against checklist (Part 7)

---

## Part 5: Negative Prompt Boilerplate (Append Automatically)

Build the negative prompt by combining ALL categories below. Never omit a category.

### Category 1 — Quality guards
```
"low quality", "grainy", "blurry", "blur", "noise", "grain",
"watermark", "text", "logo", "bad hands", "extra fingers", "mutated",
"cropped", "worst quality", "distorted"
```

### Category 2 — Lighting guards
```
"flash photography", "harsh lighting", "studio lighting", "artificial light",
"back_lit", "under_lit", "overexposed", "underexposed"
```

### Category 3 — Stock photo prevention
```
"stock photo", "staged", "overly posed", "catalog style", "sterile",
"empty showroom", "product photography",
"posed smile at camera", "looking at camera", "smiling at camera"
```

### Category 4 — Logo safe zone enforcement
```
"primary subject in bottom left corner", "face in bottom left corner",
"focal point in bottom left", "key action in bottom left quadrant",
"student face in bottom left", "hands in bottom left corner"
```

### Category 5 — Architecture guards (always)
```
"drop ceiling", "acoustic ceiling tiles", "fluorescent light panels", "fluorescent tubes",
"grid ceiling", "suspended tile ceiling", "ceiling tile grid", "T-bar ceiling grid",
"2x2 ceiling tiles", "2x4 ceiling tiles", "recessed fluorescent lighting",
"beige ceiling tiles", "yellowed ceiling panels", "institutional ceiling",
"drop tile ceiling", "ceiling with tile grid visible",
"HVAC vent visible",
"beige walls", "cream walls", "bare walls", "plain white walls without any design element",
"bookcase", "bookshelf", "wooden door frame", "institutional door",
"traditional classroom", "rows of desks", "all rectangular tables",
"colorful hanging baffles", "dangling colored panels", "teal ceiling panels"
```

*Note: add "exposed ductwork" to Category 5 for all room types EXCEPT RT-3 Maker / Project Room, where open structural ceiling is correct.*

### Category 6 — Furniture quality guards
```
"grey metal furniture", "beige furniture", "neutral colored chairs",
"brown wood tones", "muted colors", "desaturated", "washed out",
"grey stools", "silver chairs", "worn furniture", "damaged furniture",
"rectangular chairs only", "all matching identical chairs"
```

### Category 7 — Clutter guards
```
"clutter", "storage overflowing", "tape on floor", "water bottles",
"trash can", "fan", "paper piles", "boxes",
"chalkboard", "cluttered walls", "dim lighting",
"empty lounge chairs", "unoccupied seating zone"
```

### Category 8 — Border / frame guards
```
"border", "frame", "white border", "black border", "decorative border",
"vignette", "letterbox", "pillarbox", "image frame"
```

### Category 9 — Wall treatment guards
```
"horizontal stripes", "rainbow stripes", "thin stripe lines"
```

*If NOT using stripe wall, also append all of these — Gemini defaults to stripe walls in K-12 spaces even without any explicit prompt to do so:*
```
"striped wall", "vertical stripe wall", "striped wall graphic",
"multicolor stripe wall", "colored vertical stripes on wall",
"bold stripe graphic wall", "teal stripe wall", "rainbow stripe wall",
"stripe wall pattern"
```

### Category 10 — Palette-specific suppression
Append the palette-specific negatives from the selected palette in Part 3D.

### Category 11 — Floor suppression (when applicable)
If the selected floor is NOT navy_forest_carpet, also append:
```
"carpet", "carpet tiles", "navy carpet", "dark carpet", "patterned carpet",
"geometric carpet", "navy blue carpet tiles", "forest green carpet", "navy and green carpet"
```

If the selected floor IS navy_forest_carpet, suppress the palette-adjacent carpet variants from the other floor types as appropriate.

---

## Part 6: Anti-Pattern Production Rules

### Rule 1 — Three-Field Repetition (Ceiling, Wall, Floor)

**Why this exists:** Gemini has strong learned priors for K-12 spaces — drop ceiling tiles, striped feature walls, and navy/forest carpet. A single description in `scene.location` is not sufficient to override these priors. The model frequently reverts on the first or second generation.

**The fix:** Three structurally distinct fields must each carry the same architectural fact. When the ceiling type, wall treatment, and floor appear in `scene.location`, `background_elements`, AND `meta.notes`, they register as load-bearing across different parsing passes and are much harder for the model to ignore.

| Element | scene.location | background_elements | meta.notes |
|---|---|---|---|
| **Ceiling** | Describe mid-paragraph | Dedicated array entry + negations | `"Ceiling: [type]"` |
| **Wall** | Describe inline | Dedicated array entry + negations | `"Wall: [type] ([colors])"` |
| **Floor** | Describe at end of location string | Dedicated array entry | `"Floor: [type]"` |

**Background elements entry structure:**
1. Name the element positively and specifically
2. Restrict the color palette explicitly
3. List what it is NOT (aliases the model might fall back to)

*Ceiling example:*
> `"Round cobalt blue drum pendant lights on a clean flat white ceiling — large fabric-wrapped circular pendants at uniform height — NOT drop ceiling tiles, NOT fluorescent grid, NOT T-bar ceiling grid, NOT suspended acoustic tile ceiling"`

*Wall example:*
> `"Flush-mounted grid of acoustic panels in cobalt blue and bright yellow ONLY — a uniform repeating sound-mitigation grid — NO stripe pattern, NO teal panels, NO lime green panels, NOT a multicolor wall"`

*Floor example:*
> `"Light warm wood-look luxury vinyl plank flooring — pale oak or blonde wood tone — NOT carpet, NOT navy blue carpet, NOT patterned floor tile, NOT dark wood"`

---

### Rule 2 — Acoustic Panel Color Lock

Never describe acoustic panels as a generic "teal, lime green, cobalt blue, and yellow" combination. This produces a palette-confusing wall that bleeds into furniture colors and makes the space read as Classic Palette A regardless of your selection.

Always use exactly two colors from the active palette. The correct colors per palette are in Part 3D next to each palette as *Acoustic panel colors*.

---

### Rule 3 — No Brief Card Approval Prompt

Do not present a Brief Card for user approval. The user-facing interaction ends after they pick a scene concept. All dimension choices (palette, room type, ceiling, floor, wall, signature piece) are internal decisions made in Part 2 and reflected in the schema and brief log — not in the conversation.

---

## Part 7: Brief Log Maintenance

### Before generating concepts:
Read `briefs/brief-log.json`. Note the last 2 entries for: `room_type`, `palette`, `wall_treatment`, `floor`, `student_ages`, `ethnicities`. Run the Auto-Selection Engine with these constraints.

### After schema is submitted to Gemini:
Add a new entry to `briefs/brief-log.json`:

```json
{
  "date": "YYYY-MM-DD",
  "slug": "kebab-case-topic-slug",
  "post_topic": "Brief description of the post topic",
  "archetype": "space | moment | collaboration",
  "room_type": "RT label",
  "palette": "A | B | C | D | E | F",
  "wall_treatment": "stripe | solid_feature | acoustic_panels | graphic_panels",
  "ceiling": "dramatic_geometric | drum_pendants | standard_clean | open_structural",
  "floor": "navy_forest_carpet | polished_concrete | light_wood_vinyl | organic_rug | gray_carpet",
  "signature_piece": "name of signature furniture piece used",
  "student_count": 0,
  "student_ages": "elementary | middle_school | high_school | mixed",
  "ethnicities": ["list", "of", "ethnicities"],
  "activity": "brief activity description",
  "ratio": "1:1 | 4:5 | 16:9 | 9:16",
  "notes": "any generation notes or issues"
}
```

---

## Pre-Submit Verification

Before delivering any schema, confirm every item:

- [ ] Brief log checked — no dimension repeats last 2 entries
- [ ] Palette differs from last 2 briefs; Palette A avoided unless B–F have all cycled
- [ ] Room type differs from last 2 briefs
- [ ] Wall treatment differs from last brief; stripe max once per 3–4 briefs
- [ ] Floor differs from last 2 briefs; matches room type options (Part 3I)
- [ ] Ceiling matched to room type and archetype (NOT acoustic baffles)
- [ ] Signature furniture piece included in both scene.location and background_elements
- [ ] Palette VISUAL DOMINANT color fills majority of seating — named explicitly
- [ ] Acoustic panels use exactly 2 palette-matched colors (if applicable)
- [ ] Ceiling description in scene.location + background_elements + meta.notes (Three-Field Rule)
- [ ] Wall treatment in scene.location + background_elements + meta.notes
- [ ] Floor description in scene.location + background_elements + meta.notes
- [ ] Explicit palette declaration closes scene.location string
- [ ] All student subjects have explicit ethnicity + skin tone
- [ ] No subject at position "left" as solo primary
- [ ] logo_safe_zone present in composition block
- [ ] All 11 negative prompt categories present
- [ ] Palette-specific suppression from Part 3D present
- [ ] Floor suppression prompts added for non-carpet floors (Category 11)
- [ ] Stripe wall suppression in negatives (if NOT using stripe wall)
- [ ] magic_prompt_enhancer: false
- [ ] hdr_mode: true
- [ ] guidance_scale: 7.5
- [ ] steps: 50
- [ ] Correct camera / lens / aperture / ISO for archetype
- [ ] angle: eye_level
- [ ] Brief log to be updated after submission
