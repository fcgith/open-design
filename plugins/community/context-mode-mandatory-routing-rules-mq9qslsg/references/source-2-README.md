# Imperial Resort — Design System

Internal design system for **Imperial Resort · Housekeeping & Maintenance** — a private,
install-only PWA used by hotel cleaning staff, maintenance technicians, shift managers and
admins at the HI Hotels Imperial Resort, Sunny Beach (Bulgaria).

This folder is a self-contained design system: foundations (type, color, spacing, motion),
brand assets, a documented icon set, and pixel-faithful UI-kit recreations of the real
product screens. Everything here is built to let a design agent produce on-brand interfaces
and assets for Imperial — production or throwaway.

---

## 1 · What the product is

A maintenance & housekeeping issue tracker for a single large resort. It is **not** an HR /
payroll / scheduling / inventory tool — that boundary is deliberate and enforced in the copy.

**The loop it serves:**

- **Housekeepers / room staff** mark rooms as they clean them and **report issues** they find
  (a leaking tap, a dead A/C, a burnt bulb). Reporting is photo-first: *photo · note · done.*
- **Technicians** see a **queue** of reported issues, ordered by priority, and move each one
  through a fixed state machine: **Submitted → Triaged → Owned → Resolved**. A technician must
  *Take* (claim/own) an issue before they are allowed to *Resolve* it. Special repairs (needing
  parts or an external contractor) get flagged and can only be closed by a manager/admin.
- **Managers / admins** get a live operations overview: KPIs, an issues-by-floor heatmap, a
  live activity feed, and a team-load table. Rooms live in multiple buildings, split into
  sectors/floors. Admins onboard staff, manage rooms, set role permissions, and read reports.

**Rooms & structure:** rooms belong to buildings → floors/sectors; each room toggles
clean/dirty; issues attach to a room + category (Climate, Plumbing, Electrical, Furniture,
Cleaning, Other) + priority. Issues carry a conversation-style note thread and photo attachments.

**Onboarding & auth (install-only):** the marketing/entry website does one of two things —
on a **phone** it shows *install instructions* (Add to Home Screen for Android Chrome / iOS
Safari); for **admins** it offers login. Once installed, staff activate by **scanning an
admin-issued QR code** (the primary path) the first time they open the icon; if the camera
can't be opened (or on a recognised device) they fall back to a **personal password** they
set on first activation. The device then remembers the profile — no repeat login on that
device. A **new device = a new QR code** for the same user; they re-enter their password.
There is no employee-number entry. *(The earlier codebase shipped a 6-digit PIN; this system
models the QR-primary + password-fallback flow the product brief describes. Confirm which is
canonical before building production auth.)*

**Audience reality — design for "cavemen":** a meaningful share of front-line staff have
near-zero technical literacy; some can barely type on a phone. Every primary flow therefore
has a **Simple ("Easy") mode** — one huge tap target, one short list, one Help button — beside
a **Detailed ("Full") mode**. Touch targets never drop below 44px; numerals are large; copy is
short and literal. Bulgarian is the first language; English is a toggle.

**The "primitives" principle:** the app is built from **global, data-driven primitives** —
one-piece components written once and reused by *every* view (worker, tech, manager, admin).
A primitive renders the right information + actions for the current role/permission and page.
Charts, activity boards, stat tiles, tables, the issue card, the priority pip — all single
sources of truth. The UI kits here mirror that: a small set of atoms composes every screen.

---

## 2 · Sources (for whoever has access)

Built by reading the real codebase — **not** screenshots. Keep these in case the reader has access:

- **Codebase (read-only, mounted):** `mvpc/`
  - `mvpc/frontend/` — Vite + React + TypeScript PWA (the real product).
    - `src/design/` — the live primitive library (Card, Chip, PriorityPip, Icon, FixedHeader,
      BottomNav, IssueCard, NumTicker, ImpLogo, ImpWordmark, …) — **source of truth for UI.**
    - `src/worker/`, `src/tech/`, `src/admin/`, `src/views/` — role screens.
    - `src/lib/i18n.ts` — full bg/en string dictionary (1000+ lines; tone source of truth).
    - `src/index.css` — native-app shell rules + type scale.
    - `public/manifest.webmanifest` — PWA manifest (`Imperial Resort Issue Tracker`).
  - `mvpc/.design/` — a prior design-canvas recreation (the most useful reference):
    - `vendor/colors_and_type.css` — **Strata foundations** (the base layer).
    - `imperial.css` — **Imperial brand layer** (navy/teal/gold; rebinds Strata aliases).
    - `tailwind.config.js` — the same tokens as Tailwind theme.
    - `shared-ui.jsx`, `worker-home.jsx`, `tech-dashboard.jsx`, `admin-overview.jsx` — JSX
      recreations of the three core screens (adapted into this system's UI kits).
  - `mvpc/docs/staff-training-bg.md` — plain-language staff training (great copy/tone sample).
- **Live URLs referenced in copy:** `issues.imperialsunnybeach.com` (staff PWA),
  `ops.imperialsunnybeach.eu/overview` (manager web). Not verified from here.

No Figma file or slide deck was provided. No real photographic brand imagery was found in the
codebase (the product is chrome-only / data-driven), so this system ships **no photography** —
use neutral placeholders if a layout needs an image, and ask the user for real assets.

---

## 3 · Design lineage: Strata → Imperial

- **Strata** is the neutral foundation layer (type scale, spacing, radii, motion, shadow
  system, semantic tokens). Original primary was "Torch" orange + "Spring" green on Carbon
  neutrals.
- **Imperial** is the brand layer that rebinds Strata's alias ramps to **midnight navy / teal /
  soft gold** — the shipped hospitality look.
- **This system generalises that rebinding into three palettes** (see below), all driving the
  same alias names so components reskin with a single attribute.

---

## 4 · The three palette families  (`colors_and_type.css`)

Selected via two attributes on `<html>`: `data-palette` + `data-theme`.
There are **three families** — Tech (3), Hospitality (5), Nature (3) — for **11
palettes**, and every variant has a **dark (default)** and a **light** mode (22 looks).

| Family | `data-palette` | Primary (`--accent`) | Secondary (`--accent-2`) | Surface |
|---|---|---|---|---|
| **Tech** | `tech` (Azure) | electric azure `#3B6EF5` | cyan `#12B4D8` | blue-slate carbon |
| | `tech-violet` | indigo violet `#7C5CFF` | fuchsia `#E14FC6` | indigo-black |
| | `tech-lime` | signal lime `#9BD615` | cyan `#12B4D8` | graphite |
| **Hospitality** | `hospitality` (Imperial · **default**) | teal `#14B8A6` | soft gold `#F59E0B` | midnight navy |
| | `hospitality-coral` | sunset coral `#F76A47` | gold `#F59E0B` | twilight dusk |
| | `hospitality-plum` | rosé `#D06A9C` | champagne `#D8AE52` | aubergine |
| | `hospitality-marble` | brass `#BE9E52` | pewter `#7C8E9C` | polished slate stone |
| | `hospitality-walnut` | honey-amber `#D88A33` | brass `#C49A4C` | espresso wood |
| **Nature** | `nature` (Forest) | moss `#5B9E45` | terracotta `#C8743C` | deep forest |
| | `nature-clay` | terracotta `#C86A3C` | sage `#8AA556` | warm sand-charcoal |
| | `nature-ocean` | sea-green `#1FA589` | driftwood `#B89557` | deep kelp |

All variants rebind the **same alias ramps** so components are palette-agnostic:
`--teal-*` = PRIMARY · `--gold-*` = SECONDARY/premium · `--spring-*` = success/live ·
`--navy-*` = dark surface ladder. (The names are historical — read them as roles.)

### Exotic palettes (v2)

A second, deliberately **vivid / uncommon** set — same alias ramps + semantics, so
components reskin identically. Three reimagined families, three variants each:

| Family | `data-palette` | Primary | Secondary | Surface |
|---|---|---|---|---|
| **Quantum / Nano** | `quantum-plasma` | plasma magenta `#EC3FD0` | electric cyan `#13C2E0` | obsidian void-violet |
| | `nano-iridium` | iridescent lime `#AEDC1A` | molten copper `#D2773A` | gunmetal |
| | `cryo-qubit` | ice blue `#2E86FF` | hot magenta `#E6469F` | supercooled indigo-black |
| **Exobiology** | `exo-biolume` | bioluminescent aqua `#1FC994` | ultraviolet `#8D5BFF` | abyssal teal-black |
| | `spore-myco` | toxic-spore chartreuse `#C2CE1C` | biolume magenta `#E24E97` | humus brown-black |
| | `xeno-venom` | poison-dart azure `#2C8AEE` | venom acid-green `#A8CE26` | jungle blue-black |
| **Space Hotel** | `orbital-aurora` | aurora green `#1AC885` | aurora magenta `#D94FAC` | deep-space navy |
| | `stellar-nebula` | nebula coral-pink `#F95277` | stellar cyan `#16B4D8` | plum-violet nebula |
| | `zero-g-lumen` | rose-gold peach `#E0865A` | cosmic violet `#8268EE` | warm orbital charcoal |

These are exploratory directions for picking a brand identity — not all are equally
"operational"; the priority ramp stays universal beneath them all.

The **priority ramp is identical in every palette** (`--pri-low` green → `--pri-medium` amber
→ `--pri-high` orange → `--pri-urgent` red). Signal colour must never depend on theme — a
non-technical user must read "red = urgent" everywhere, always.

```html
<html data-palette="hospitality" data-theme="dark">  <!-- default look -->
<html data-palette="tech" data-theme="light">
<html data-palette="nature" data-theme="dark">
```

---

## 5 · CONTENT FUNDAMENTALS

The product is **bilingual, Bulgarian-first**; English is a 1:1 toggle (`BG`/`EN` flag in the
header). Write Bulgarian first, then English; never ship one without the other.

**Tone:** calm, literal, reassuring, operational. It is a tool used mid-shift by tired people —
no marketing voice, no cleverness, no jokes. Sentences are short and imperative. The app speaks
**to** the worker ("you"), using their first name and role ("Good morning, Maria Petrova ·
Housekeeping · Floor 4"). Reassurance over alarm: the install screen is framed as *"this is
normal, here's what to do,"* explicitly **not** an error.

**Casing:** Sentence case for everything readable. **Eyebrows / section labels are UPPERCASE**
with wide tracking (0.22em) in the secondary accent colour (e.g. `MORNING SHIFT · UNTIL 14:00`,
`LIVE ACTIVITY`, `QUICK CATEGORIES`). Button labels are sentence case, one or two words
(`Report an issue`, `Take`, `Resolve`, `Call the coordinator`).

**Verbs are the vocabulary** — the whole UX is verb-driven and matches the state machine:
*Report · Take (own) · Resolve · Escalate · Add photo · Special repair · Call the coordinator.*
Status words are fixed nouns: *Submitted · Triage · In progress / Owned · Resolved.*

**Numbers & IDs are monospace.** Issue IDs read `IM-208`; rooms read `Room 412 · Floor 4`;
times are relative and plain (`22m ago`, `1h ago`, `this morning` / `преди 22 мин`).
KPIs are big tabular-nums with a small +/- delta line (`Median response · 8m · −12% w/w`).

**No emoji, ever.** None appear in product copy and none should. Iconography carries meaning,
not emoji. Unicode is used only for functional glyphs: `·` as a separator, `→` in "see all"
links, `⌘K` in the desktop search hint.

**Examples (verbatim, bg → en):**
- `Подай сигнал` → `Report an issue` · sub: `Снимай · опиши · готово` → `Photo · note · done`
- Simple mode CTA: `Имам проблем` → `I have a problem` · `Натиснете тук` → `Press here`
- `Поеми` → `Take` · `Готово` → `Resolve` · `Поет от теб` → `Owned by you`
- `Обади се на координатор` → `Call the coordinator`
- Training voice: *"PIN е еднократен — унищожи хартията."* ("The PIN is one-time — destroy the paper.")
- Conflict copy: *"Сигналът вече е поет"* → *"This issue is already taken."*

---

## 6 · VISUAL FOUNDATIONS

**Overall feel:** a dark-first, native-app instrument panel with luxury restraint — lots of
calm negative space, one confident accent, hairline rules, big legible numbers. Never busy,
never decorative-for-decoration. Dark is the home state; light is a faithful opt-in.

**Type:** single family — **Schibsted Grotesk** (400–900) for *everything* UI and display;
**JetBrains Mono** for numerals, IDs, times, kbd hints. Headings 600 weight, tight tracking
(−0.015 to −0.035em); body 400 at 15–16px; eyebrows 600 uppercase 0.22em. Calm by default,
emphasis opts up to 600/700. `text-wrap: balance` on headings, `pretty` on body.

**Color usage:** one surface family + ONE primary accent doing the work. Secondary accent
(gold/cyan/terracotta) is rationed — eyebrows, premium/priority highlights, the active nav
rail mark, a single hairline at a card's top edge. Success/live = the spring green with a
pulsing dot. Backgrounds are flat solid tokens, **not** gradients — the only gradients allowed
are (a) the thin animated "sea-horizon" accent line and (b) faint protection fades behind
fixed bars so content scrolls cleanly under them.

**Backgrounds:** flat `--bg`. No photographic backdrops, no busy patterns. The hospitality
canvas may carry an *extremely* faint radial accent glow + a low-opacity hairline grid
(starfield feel) on big surfaces only; tech leans cooler/flatter; nature slightly warmer. No
hand-drawn illustration, no stock imagery.

**Elevation & shadows:** **flat by design — no float, no glow.** Elevation is communicated
by (1) lifting surface lightness `--bg → --bg-elev-1 → --bg-elev-2` and (2) a 1px luminous
rim (`0 0 0 1px rgba(255,255,255,.06)` in dark), with only a **hairline** drop beneath
(`sh-2`…`sh-4` top out at a 3–8px blur, very low alpha). The primary CTA carries a **crisp 1px
accent ring** (`--sh-accent`) — explicitly *not* a coloured glow halo. Light mode uses a single
soft, subtle grey drop. Avoid large blurred drop-shadows and any neon/3D accent glow.

**Cards:** `--bg-elev-1` fill, 1px `--line` border, **16px** radius (`--r-lg`), 14–22px padding,
`--sh-2`. A signature touch: a faint **accent hairline gradient across the top edge** of KPI /
hero cards (`linear-gradient(90deg, transparent, accent, transparent)`, ~0.55 opacity). No
coloured left-border-accent cards (an explicitly avoided trope).

**Corner radii:** controls/chips 10–12px, cards 16px, hero CTAs 18px, sheets/modals 22px,
pills/avatars/toggles fully round (`--r-pill`). Consistent, never sharp.

**Borders & rules:** hairlines only — `--line` (default), `--line-2` (faint dividers),
`--line-strong` (emphasis/focus). A `imp-rule` gold/accent hairline gradient separates sections.

**Buttons & CTAs:** the hero action is a **card-style CTA** (outlined icon tile + title + sub +
trailing arrow), not a candy-fill block — calm and large. Solid filled buttons exist for
discrete actions (Resolve = solid success green; Take = outlined secondary-accent). Min height
44px, primary actions 56–64px. Filled buttons use `--on-accent` text for contrast.

**Hover states:** subtle — a −2px lift (`translateY`), border-colour shift to the accent, and a
faint elevated wash (`--bg-elev-2`); KPI/category tiles lift on hover. Ghost icon buttons get a
`--bg-elev-2` wash. No colour inversion.

**Press / active states:** brief spring scale-down + an accent-soft wash; the HeroCTA emits a
soft success **ripple** on tap. Springy easing (`--ease-spring-2`) on interactive transforms.

**Animation:** **GSAP is the primary, app-wide motion engine** in the UI kits — CSS
keyframes/transitions are intentionally avoided so one system owns all movement. Purposeful
and quick: entrance = short staggered fade-up (`y:16→0`, ~0.5s, `power3.out`, ~0.06s stagger);
SPA navigation slides pages and tweens the bottom-nav / mode-toggle thumbs; hovers lift −2/−3px,
presses use a spring (`elastic.out`). Signature loops via GSAP: the **sea-horizon** line drift,
**pulse rings** on live/in-progress dots, the QR **scan-line**, number **tickers**. A safety
reveal guarantees content is never stuck hidden if the clock is throttled. Honour
`prefers-reduced-motion` for decorative loops.

**Transparency & blur:** used sparingly — accent-soft fills (12–18% alpha) behind chips/active
nav; protection fades behind fixed top/bottom chrome. No heavy frosted-glass everywhere.

**Layout & chrome:** fixed top header (52px), optional iOS-style back/refresh nav row (44px),
and a floating rounded **bottom-nav pill** (60px) on mobile; desktop is a 232px sidebar + fluid
main. Scroll containers reserve top/bottom insets via tokens so fixed chrome never covers
content. Mobile artboards are 402×874 (iPhone-class); desktop is ~1320 wide.

**Imagery vibe:** none shipped. If photography is ever added, keep it cool, calm, architectural,
low-saturation to sit with the navy/teal palette — but confirm with the user first.

---

## 7 · ICONOGRAPHY

- **One custom line-icon set**, Lucide-style: **24×24** viewBox, **1.6** stroke, round caps &
  joins, no fill. It ships as an inline path map in the codebase
  (`src/design/icons-data.ts` / `.design/shared-ui.jsx`) and is mirrored here at
  **`assets/icons.js`** (`ICONS` map + `iconSvg(name, {size,color,stroke})` helper). The UI kits
  use a matching `<Icon>` component reading the same map.
- **Coverage:** `bell plus camera check arrow back search filter user spark wrench drop bolt
  bed sparkle clock pin message moon sun more trend chart refresh gear logout chevron switch
  close edit trash`. Domain categories map to icons: Climate→`spark`, Plumbing→`drop`,
  Electrical→`bolt`, Furniture→`bed`, Cleaning→`sparkle`, Other→`more`.
- **Substitution note:** the set is visually Lucide-compatible. If you need a glyph not in the
  map, pull the matching **Lucide** icon (same 24×24 / ~1.5–1.6 stroke / round caps) so it sits
  seamlessly — and add it to `assets/icons.js`. ⚠️ Don't introduce a second icon family or
  filled/duotone icons.
- **Logo / brand mark:** an **interlocking-monogram** — two vertical bars + a horizontal
  cross-bar + a central ring (an "I·H"-style imperial monogram). Default fill is the secondary
  accent (gold in hospitality); a `currentColor` mono variant exists for one-colour contexts.
  Files: `assets/logo-monogram.svg` (gold) and `assets/logo-monogram-mono.svg` (currentColor).
  The lockup ("wordmark") pairs the mark with `IMPERIAL` (600, 0.18em tracking, uppercase) over
  a small `SUNNY BEACH` line (0.34em, `--fg-3`). Brand header text is fixed:
  line 1 = `IMPERIAL RESORT`, line 2 = `Housekeeping & Maintenance`.
- **Emoji / unicode:** no emoji anywhere. Unicode used functionally only (`·`, `→`, `⌘K`).
- **Note on the codebase favicon:** `frontend/public/favicon.svg` + `icons.svg` are a leftover
  dev-template asset (a purple bolt + social-media sprite) and are **not** Imperial brand — they
  were intentionally **not** copied here. Use the monogram.

---

## 8 · Index / manifest of this folder

```
README.md                     ← you are here (context · content · visual · iconography)
SKILL.md                      ← Agent-Skill front-matter wrapper (for Claude Code)
colors_and_type.css           ← foundations + 3 palettes × dark/light (THE token file)

assets/
  logo-monogram.svg           ← brand mark, gold fill
  logo-monogram-mono.svg      ← brand mark, currentColor
  icons.js                    ← custom Lucide-style icon set (ICONS map + iconSvg helper)

preview/                      ← Design-System tab cards (palettes, type, components, …)

ui_kits/
  app/                        ← mobile PWA. AppShell (TopBar/SubBar/BottomNav) + pages:
                                InstallGate · WorkerHome · TechQueue · IssueDetail. README + index.html
  admin/                      ← desktop manager/admin operations console. README + index.html + JSX
```

See each `ui_kits/*/README.md` for the component inventory of that surface.
