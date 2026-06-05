# Kay-Twelve ImageGeneration — Claude Session Rules

## Mandatory: Read These Files First

At the start of every session involving image creation, content, or design, read these files before doing anything else:

1. `skills/photo-gen/SKILL.md` — primary photo generation skill (v3)
2. `.claude/skills/kay-twelve-social-cards/SKILL.md` — card composition via Pencil MCP
3. `.claude/skills/kay-twelve-brand-voice/SKILL.md` — brand voice for captions and copy
4. `briefs/brief-log.json` — rotation history; check before generating any concepts

## Workflow

1. Read skills + brief log
2. Brainstorm 2–3 scene concepts from the post copy → user picks one
3. Run Auto-Selection Engine (internal, silent) → build and deliver the JSON schema
4. User generates in Gemini
5. Update brief log after submission
6. Social card composition via Pencil MCP (kay-twelve-social-cards skill)

No Brief Card. No approval prompts. All dimension decisions are internal.

## Brand Reminders

- **Kay-Twelve** — always hyphenated, capital K and T
- **Learning Space Integrator** — never "furniture company" or "vendor"
- Brand colors (#208ca9 Pelorous Blue, #ffb41f Yellow, etc.) are for card overlays ONLY — not photo furniture colors
- Card bar: Pelorous Blue `#208ca9`, 80% opacity, bottom 12%
- Card text: Source Sans Pro, white

## Reference Files

- `skills/photo-gen/SKILL.md` — v3 skill (authoritative)
- `briefs/brief-log.json` — all past briefs with rotation tracking
- `examples/nano-banana-schema.json` — Nano Banana schema reference
- `examples/photos/` — good subject/people reference photos
- `examples/bad/` — DO NOT replicate (cluttered, dated, dim, traditional rows)
- `docs/kay-twelve-visual-style-guide.md` — full visual spec (Section 12 = card specs)
