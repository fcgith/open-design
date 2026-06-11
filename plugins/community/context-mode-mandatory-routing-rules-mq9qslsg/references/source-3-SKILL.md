---
name: imperial-resort-design
description: Use this skill to generate well-branded interfaces and assets for Imperial Resort — the internal Housekeeping & Maintenance PWA — for production or throwaway prototypes/mocks. Contains the design foundations (three palettes × dark/light, type, fonts), brand assets, the custom icon set, and pixel-faithful UI-kit components (mobile app + desktop admin console).
user-invocable: true
---

# Imperial Resort — design skill

Read `README.md` first — it carries the full product context, **content
fundamentals** (Bulgarian-first, calm/literal, verb-driven, no emoji),
**visual foundations**, and **iconography**. Then explore:

- `colors_and_type.css` — the token file. Set `data-palette` (`tech` /
  `hospitality` / `nature`) + `data-theme` (`dark` / `light`) on `<html>`.
  Components read semantic + alias tokens, so they reskin with one attribute.
- `assets/` — the monogram logo (`logo-monogram*.svg`) and the custom
  Lucide-style icon set (`icons.js`: `ICONS` map + `iconSvg()` helper).
- `preview/` — small specimen cards for palettes, type, spacing, components, brand.
- `ui_kits/app/` — mobile PWA recreation (Housekeeper / Technician / Install).
- `ui_kits/admin/` — desktop operations console recreation.

## How to use it
- **Visual artifacts** (slides, mocks, throwaway prototypes): copy the assets
  and tokens you need into your output and build static/standalone HTML the user
  can open. Reuse the UI-kit atoms (`shared-ui.jsx`) rather than re-deriving them.
- **Production code**: read the rules here to become an expert in the brand, then
  apply the tokens/components against the real codebase.

## Non-negotiables
- Bulgarian first, English second — never ship one language only.
- No emoji. Numerals / IDs / times in JetBrains Mono. Touch targets ≥ 44px.
- Priority colours (green → amber → orange → red) are universal — never theme them.
- One primary accent does the work; ration the secondary accent. Flat surfaces,
  hairline borders, luminance-based shadows in dark mode. No bluish-purple
  gradients, no coloured-left-border cards, no candy-fill hero buttons.

If invoked with no brief, ask what the user wants to build, ask a few focused
questions (surface, audience, palette, dark/light, variations), then act as an
expert designer who outputs HTML artifacts **or** production code as needed.
