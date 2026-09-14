# praxisverify.com — Design Notes

A working reference for what's actually implemented on the marketing site. Not the brand system itself — that lives in the `pv-branding` skill (`~/.claude/skills/pv-branding/SKILL.md`), which is the canonical source for colour, typography, and component rules across all surfaces (website, MVP platform, admin tool, pitch decks). This file is the site-specific snapshot: the tokens, components, and conventions actually shipped to `praxisverify.com`.

If `pv-branding` and this file disagree, the brand skill wins and this file should be updated.

> **Redesign, 2026-08-15.** All six pages moved from the single-column glass-card layout to a full-width alternating band system, with **Warm Amber `#F59E0B` added as a third accent**. Approved by Lisa: third accent, loosened Authority/Journal split, wider layout, replaced serif headline treatment, dropped the isometric cube background. See *What changed* at the end of this file.

## Stack & conventions

| Item | Choice |
|---|---|
| Markup | Plain HTML, no framework |
| Styles | Inline `<style>` block per page — no external stylesheet, no preprocessor |
| Scripts | Inline `<script>` — fade-in IntersectionObserver (index), nav scroll-spy (index), ledger clock (index), cookie consent (all pages), nav hamburger toggle + Escape-to-close (all pages) |
| Hosting | GitHub Pages from `master`; Cloudflare in front for security headers and TLS |
| Build | None — files served as-is |
| Deploy | `git push origin master` → GitHub Pages rebuilds in ~1–2 min |

Pages: `index.html`, `experts.html`, `about.html`, `contact.html`, `privacy.html`, `terms.html`.

Each page carries its own copy of the CSS block. `index.html` has the full block; the five interior pages share an identical, slightly smaller **interior block** (same `:root`, nav, page-header, card, footer and cookie rules; no hero/ledger/door components). Changes to shared tokens must be repeated across all six files — there is no shared stylesheet. When changing a token, grep for it across the directory.

## Pillars

The dark/light split is now **within** each page rather than between pages. Every page runs the same shell:

- **Dark shell** — sticky navy nav, dark page header (or hero), dark footer. Deep Navy `#0A192F` deepening to `#08152a`.
- **Light content** — `#F3F4F6` and `#FFFFFF` bands carrying the body content.

`index.html` alternates bands down the page (dark hero → deep stat strip → light → white → dark ledger → white FAQ → light CTA → dark footer). The five interior pages run dark header → light content → dark footer.

> The old rule "dark pages stay dark, `experts.html` stays light" is **retired** (Lisa, 2026-08-15). `experts.html` now uses the same shell as every other page. The `.journal-bg-mark` element is retained in markup on that page but set to `display:none`.

> A full-page dark Authority hero + scrolling dark→light journey + motion layer was prototyped and rejected by Lisa (2026-06-11). The 2026-08-15 redesign is a different treatment — light content bands carry all body copy; dark is used only for the shell, the stat strip and the ledger.

## Colour tokens

Custom properties declared on `:root` in every page. **All six pages now share one identical token set** — there is no longer a separate Journal variant.

| Token | Value | Use |
|---|---|---|
| `--bg` | `#0A192F` | Deep Navy — nav, hero, ledger band |
| `--bg-deep` | `#08152a` | Deepest navy — stat strip, footer |
| `--bg-light` | `#F3F4F6` | Light content band |
| `--surface` | `#FFFFFF` | Card / white band |
| `--surface-alt` | `#FBFBFC` | Quiet card fill (features, closed expanders) |
| `--rule` | `rgba(35,53,84,0.9)` | Hairline on dark |
| `--rule-light` | `#E2E4E9` | Hairline on light |
| `--rule-lighter` | `#EDEFF2` | Table row divider |
| `--emerald` | `#10B981` | Primary action fill |
| `--emerald-dark` | `#047857` | Emerald as *text* on white (5.48:1) |
| `--amber` | `#F59E0B` | **Third accent** — stat numerals, expert route, step 04 |
| `--amber-light` | `#FBBF24` | Amber on dark backgrounds |
| `--amber-dark` | `#B45309` | Amber as *text* on light (4.6:1) |
| `--amber-deep` | `#D97706` | Amber button hover |
| `--text` / `--text-bright` | `#E5E9F0` / `#F4F6FA` | Body / emphasis on dark |
| `--text-body` | `#B4BECE` | Long-form body on dark |
| `--text-muted` | `#96A0B4` | Secondary on dark |
| `--text-faint` | `#8892A6` | Mono micro-text on dark (4.6:1 — the floor) |
| `--card-text` | `#0A192F` | Headings on light |
| `--card-text-muted` | `#3F4654` | Body on light |
| `--card-text-faint` | `#5C6473` | Tertiary on light |
| `--link` / `--link-hover` | `#047857` / `#065F46` | Links on light bands |
| `--heading` / `--body` / `--mono` | EB Garamond / Inter / JetBrains Mono | Font stacks |

Supporting values used directly: Elevated Navy `#112240` (audit ledger), Navy Hairline `#233554` (ledger borders), Emerald hover `#059669`.

Retired tokens: `--btn-bg`, `--btn-text`, `--border`, `--surface-solid`, `--accent-platform`. Purple/indigo remains retired system-wide — **including the experts chips**, which now use the emerald/amber/navy families (see below).

### Amber usage rule

Amber is the **accent**, never the action. Emerald remains the only CTA fill on buyer paths, so the eye can always find "what do I click". Amber marks:

- statistic numerals (the stat strip),
- section eyebrows (`.eyebrow-amber`),
- the expert route end-to-end — nav link, hero door, `#apply` CTA, experts-page card borders,
- step 04 of the four-step flow (the "verified" outcome),
- the pull-quote keel.

Never use amber for a buyer-facing primary button, and never as text below `--amber-dark` on light backgrounds.

### Chip palette (experts page)

The 2026-06-11 multi-hue exception (indigo/violet/cyan/blue/slate) is **retired** — the chips now sit inside the three-colour system:

| Group | Class | Treatment |
|---|---|---|
| Quality systems & audit | `.chip` | Emerald tint, `--emerald-dark` text |
| Regulatory submissions | `.chip-regulatory` | Amber tint, `--amber-dark` text |
| Product safety | `.chip-safety` | Navy tint, `--card-text` |
| Design / software / clinical | `.chip-design` | Emerald tint |
| Standalone disciplines | `.chip-standalone` | Outline only, `--card-text-faint` |

Chips are set in JetBrains Mono at `0.7rem`. If the five-way visual separation matters more than palette discipline, the old scheme can be restored — flag it and it goes back.

## Typography

| Element | Font | Weight | Size |
|---|---|---|---|
| Hero `<h1>` (index) | EB Garamond | 700 | `clamp(2.6rem, 7.4vw, 5.4rem)` |
| Hero sub (index) | EB Garamond italic | 600 | `clamp(1.2rem, 2.2vw, 1.75rem)`, amber |
| Page title (interior) | EB Garamond | 700 | `clamp(2.3rem, 5.5vw, 3.9rem)` |
| Section heading (`.h2`) | EB Garamond | 700 | `clamp(2.1rem, 4vw, 3.1rem)` |
| Card `<h2>` (interior) | EB Garamond | 700 | `clamp(1.55rem, 3vw, 2.1rem)` |
| Door / card title | EB Garamond | 700 | `1.5–1.6rem` |
| Body | Inter | 400 | `0.88–1.02rem` |
| Eyebrow, chip, timestamp, footnote | JetBrains Mono | 400–500 | `0.68–0.72rem`, `0.1–0.14em` tracking, uppercase |

All display type uses `letter-spacing: -0.02em` and tight `line-height` (1.0–1.15).

> **`transform: scaleY()` is removed.** The old page titles stretched glyphs but not the layout box, which forced a compensating `line-height: 1.5` and still risked descender collisions. Headings are now set at their true weight with negative tracking. Do not reintroduce the transform.

JetBrains Mono is now **loaded** from Google Fonts on every page (weights 400/500). It was referenced in CSS before this change but never requested, so mono text silently fell back to the system monospace.

## Layout

- `.container { max-width: 1120px; margin: 0 auto; padding: 0 2rem; }` (was 880px).
- Full-width bands: `.band` (5.5rem vertical), `.band-deep` (3.25rem, hairline top and bottom), `.band-light`, `.band-white`, `.band-dark`. Bands own their own background; the container only handles width.
- Multi-column blocks use `repeat(auto-fit, minmax(N, 1fr))` grids, so they reflow without breakpoint rules: `.split` (320px), `.doors` / `.ea-cards` (290px), `.features` (250px), `.stats` (230px), `.flow` (200px).
- Background texture: a faint hairline grid (`64px`, `rgba(148,163,184,0.055)`) radial-masked to fade at the edges, on the index hero and every interior page header. The isometric cube SVG (`.bg-pattern`) is **removed from all pages**.
- `html { scroll-behavior: smooth; scroll-padding-top: 5rem; }`.
- Nav is `position: sticky` (was `fixed`), so it no longer needs body offset compensation.

### Breakpoints

Far fewer than before — the auto-fit grids absorb most of the work.

| Width | Behaviour |
|---|---|
| `≤ 860px` | Hamburger nav engages (was 640px — the link set is wider now). |
| `≤ 760px` | Stat strip drops its vertical rules and stacks with top rules instead. |
| `≤ 736px` | Band padding reduces to 4rem; container padding to 1.5rem; card padding reduces. |
| `≤ 480px` | Container padding 1.25rem; nav brand `<span>` hides; card radius and padding reduce. |

## Components

### Two-door hero (`index.html`)

Radial navy gradient, masked hairline grid, mark watermark at 6% top-right. Amber pill badge with a slow-pulsing dot ("Early access · onboarding first companies"), EB Garamond headline, amber italic sub, body copy, then the two doors:

- **`.door-buyer`** — white card, emerald top rule, "Get a document verified" → cal.eu demo booking.
- **`.door-expert`** — translucent card on navy, amber top rule, "Join the verified bench" → `/experts`.

Both lift 4px on hover. Label, title and paragraph inside each door **must be block-level** — they were spans in the first cut and collapsed onto one line.

### Stat strip (`.band-deep`, index.html)

Three auto-fit columns on the deepest navy, separated by vertical hairlines. Numerals are EB Garamond 600 in **amber** at `clamp(2.6rem, 5vw, 4rem)`; labels are `--text-muted`. Source line below in mono.

### Split section (`.split`)

Two auto-fit columns, `4rem` gap. Used for "Why Trust Can't Be Automated" (narrative + expanders) and the audit ledger (narrative + ledger window).

### Expanders (`<details>`)

Native, no JS. Two shapes:

- **`.acc`** — a bordered stack of `<details>` sharing one white card (the four regulatory frameworks; the homepage FAQ).
- **`.acc-solo`** — a single expander (the comparison table on index; the footer folds).

Chevron is an inline SVG carrying `.pv-chev`, rotated 90° via `details[open] > summary .pv-chev`. The experts-page FAQ (`.faq-item` / `.faq-q`) uses a CSS triangle instead of the SVG but behaves identically.

The comparison table, the References list and the Regulatory Disclaimer are all expanders now — this is the main density fix.

### Homepage FAQ (`#faq`, index.html)

White band between the audit ledger and the early-access CTA, using the `.acc`
stack. Added 2026-08-16 so the five `FAQPage` Q&As have visible counterparts —
they previously existed only in JSON-LD, which fails Google's requirement that
FAQ content be on the page and, more importantly, hides them from LLM crawlers
that read visible text and ignore structured data.

**The visible copy and the `FAQPage` schema must stay verbatim-identical.**
Change one, change the other. Check with:
`python3 ~/Projects/praxisverify/marketing/site-copy/extract_site_copy.py`

Not linked from the nav, and not observed by the scroll-spy.

### Four-step flow (`.flow`)

Auto-fit grid of four. Each step is a top rule + mono numeral (`01`–`04`) + serif title + body. Steps 01–03 are emerald; **step 04 is amber** (the verified outcome). The old 48px numbered circles and connector rail are gone. On `experts.html` the same markup (`.flow-step`, `.flow-num`) is restyled to match; `.flow-num::before { content: "0" }` supplies the leading zero for that page's `1`–`4` numerals.

### Feature cards (`.features`)

Auto-fit grid of four bordered cards. The Security card (`.feature-hero`) is tinted emerald and carries the `.audit-seal` stamp.

### Audit ledger (`.ledger`, index.html)

Elevated Navy window on the dark band, with traffic-light dots and a **live UTC clock** in the header (`#ledger-clock`, updated once per second). Four rows; the third (`Verified approval time-stamped`) is emerald-filled and animates in with `.stamp-on-mount`.

### Interior page header (`.page-header`)

Dark band matching the index hero: gradient, masked grid, `.authority-watermark` at 6%. The old white glass card wrapper is now transparent (`.page-header .glass-card` has no background, border or padding) and `.header-logo` is hidden — the wordmark already sits in the nav. Breadcrumbs are mono uppercase with amber links.

### Interior content cards

`.glass-card` / `.content-card` / `.policy-card` all resolve to: white surface, `1px` `--rule-light` border, `16px` radius, `2.5rem 2.75rem` padding, soft shadow. The 2px emerald border is retired — the emerald outline on every card was doing the work that the band system now does.

### Primary CTA

`.cta-btn` / `.btn-emerald` — emerald fill, **Deep Navy text**, full pill, `#059669` hover, 1px lift. Amber variant (`.btn-amber`, `.apply-card .cta-btn`) for expert-route actions. Quiet variants: `.btn-quiet` / `.cta-outline` (hairline border, muted text).

### Nav (all six pages)

Sticky translucent navy bar on every page. Link set: **The gap · How it works · Audit trail · For experts · About · Contact**, plus a pill CTA — emerald "Book a demo" (cal.eu) on five pages, amber "How to join" (`#apply`) on experts. "For experts" is amber on every page.

- Current page marked with `aria-current="page"` — emerald inset underline, amber for the experts link. `index.html` runs an IntersectionObserver scroll-spy over `#problem`, `#how`, `#trail`, `#early-access`.
- `aria-expanded`/`aria-controls` on the toggle; outside-click and Escape close (Escape returns focus to the toggle).
- Nav logo is the **white mark on every page** now that all navs are dark.

> **Anchor change:** `#solution` no longer exists — it split into `#how` (what we're building) and `#trail` (audit ledger). Any external link to `praxisverify.com/#solution` will land at the top of the page instead. Check LinkedIn posts and the pitch deck before shipping.

### Footer (all six pages)

Dark `--bg-deep` band, hairline-ruled into three rows: brand + contact email / link set; then (index only) two `.footer-fold` expanders holding **References** and **Regulatory disclaimer & editorial policy**; then the copyright + CRO line and the mono `Reviewed:` sign-off. The two folds replaced two full-width white cards, which is most of the density saving on the homepage.

### Audit seal (`.audit-seal`)

Unchanged in role: Emerald Soft pill with a `✓` prefix, `--emerald-dark` text on light, `#10B981` inside the dark ledger. Now also used as credential badges on `about.html`.

### Cookie banner (all six pages)

Unchanged. Fixed at bottom, Accept / Decline, gating GA4 via Consent Mode v2, choice persisted in `localStorage` (`cookie_consent`). GA4 head block + CSP meta still replicated on every page.

## Accessibility

- WCAG 2.1 AA remains the requirement. Emerald `#10B981` is never text on white — `--emerald-dark: #047857` covers that. Amber `#F59E0B` is likewise never text on light — `--amber-dark: #B45309` covers it. Text on emerald and amber fills is Deep Navy, never white.
- Mono micro-text on dark uses `--text-faint: #8892A6` (4.6:1). `#5C6A82` was used in the first cut of the redesign and fails — do not reintroduce it for text.
- `a:focus-visible` / `button:focus-visible` / `summary:focus-visible`: 2px emerald outline, 2px offset; `--emerald-dark` override inside light bands.
- Skip link on every page; `<main id="main-content">` everywhere.
- `prefers-reduced-motion` disables the stamp animation, the hero badge pulse, the door lift and the button lift, and reveals `.fade-in` content immediately.
- Every decorative SVG and watermark carries `aria-hidden="true"`; expanders are native `<details>`, so keyboard and screen-reader behaviour is free.

## Assets

Unchanged. Favicons `img/brand/mark-adaptive.svg` + `img/pvlogo.png` + `/favicon-180.png`; social card `img/social-card.png` (1200×630) with duplicates in `img/brand/` and the repo root; template at `drafts/social-card.html`.

`img/brand/logo-stacked-emerald-10b981-transparent.svg` is no longer rendered (the experts page header logo is hidden) but remains in the repo.

**Google Form header:** `The High-Trust Bridge` banner, 1600×400, centred variant — navy radial gradient, masked grid, centred mark and wordmark, emerald/amber base rule. Matching Forms theme: theme colour `#10B981`, background `#F3F4F6`, Decorative font.

## Page patterns

1. `<head>` — meta, CSP, GA4 + Consent Mode v2, Schema.org JSON-LD, inline `<style>`. **Unchanged by the redesign** — all seven JSON-LD blocks on index and every schema block on the interior pages are byte-identical to before.
2. Skip link → `<nav class="site-nav">` sticky top bar.
3. Dark band: `<section class="hero">` (index) or `<header class="page-header">` (interior).
4. `<main id="main-content">` — light band(s).
5. `<footer class="site-footer">` — dark.
6. Cookie banner + inline `<script>`.

## Conventions

Unchanged: datestamps in `<meta name="date">` + JSON-LD `dateModified` + visible footer `<time>` (plus `sitemap.xml` lastmod and `llms.txt`); Schema.org kept in sync with visible FAQ/specialism content; "PraxisVerify Limited" in legal contexts, CRO 812849; abbreviations expanded on first use per page; UK English; € before $; no emojis.

## Anti-patterns (don't do)

- No purple/indigo anywhere — now including the experts chips.
- No white text on emerald `#10B981` or amber `#F59E0B` — Deep Navy text.
- No emerald `#10B981` or amber `#F59E0B` as *text* on light backgrounds — use `#047857` / `#B45309`.
- Don't use amber for a buyer-facing primary button. Emerald is the action colour; amber is the accent and the expert route.
- Don't reintroduce `transform: scaleY()` on headings.
- Don't put block-axis margin on a `<span>` outside a flex/grid parent — it is silently ignored. This broke the hero doors once already.
- Don't wrap the logo in a white container; don't put the navy mark on a dark nav.
- No `rounded-md` (6px) on interactive elements — 8px minimum, full pills for CTAs.
- Don't add a build step or extract CSS to a stylesheet without a clear migration plan — the inline-CSS-per-page constraint is intentional.

## What changed (2026-08-15)

| Area | Before | After |
|---|---|---|
| Accent colours | Navy + emerald | Navy + emerald + **amber `#F59E0B`** |
| Layout | 880px single column of glass cards | 1120px full-width alternating bands |
| Pillars | Dark pages vs light Journal page | One shell: dark chrome, light content, on every page |
| Homepage entry | One hero card, "Explore the Problem" | Two doors — buyer (emerald) and expert (amber) |
| Density | Frameworks, comparison table, references and disclaimer all fully expanded | All four are expanders |
| Headings | `scaleY(1.15)` + `line-height: 1.5` | True weight + negative tracking |
| Background | Isometric cube SVG at 18% | Masked hairline grid on dark bands only |
| Nav | Problem · Solution · About · For Experts · Early Access · Contact | The gap · How it works · Audit trail · **For experts** · About · Contact |
| Mono | Referenced, never loaded | JetBrains Mono loaded on every page |
| Breakpoint for hamburger | 640px | 860px |

**Open items:** none. The "Read the full framework detail →" link on the homepage was removed on 2026-08-15 (Lisa) rather than repointed — it promised a framework write-up that does not exist. The paragraph it sat in ("None of these can be performed by AI alone…") is unchanged.
