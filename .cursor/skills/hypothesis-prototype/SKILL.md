---
name: hypothesis-prototype
description: >-
  Scaffolds low-code, navigable web prototypes styled with the Hexa design
  system (Bees Yellow theme) and instrumented with usage tracking (clicks,
  pageviews, scroll depth, forms, time on page, custom events) so the user
  can run hypothesis tests with real users and measure where they click and
  what actions they take. Use when the user asks to create a prototype,
  hypothesis test, A/B test, landing page experiment, fake-door test,
  navigable mockup, or any web page where they want to track user
  behaviour. Expects a PRD, HLR, or minimum requirements brief before
  scaffolding. The skill writes a new prototype under prototypes/<id>/,
  reuses the shared Hexa CSS + tracker library in prototypes/_shared/, and
  registers it in prototypes/prototypes.json.
---

# Hypothesis Prototype Builder (Hexa edition)

This skill scaffolds **low-code, navigable web prototypes** that look and feel like real BEES products because they use the **Hexa design system port** in `prototypes/_shared/`.

> **Reference implementation:** `prototypes/edi-central-tracker/` is the canonical example of every pattern below. Read its `index.html`, `styles.css`, and `data.js` when unsure how to apply a pattern.

## Hard rules (read first)

1. **Use Hexa primitives.** Never write a `<button>` from scratch — use `<button class="hexa-btn hexa-btn--primary">`. Same for inputs, cards, alerts, tags, etc. The full inventory and copy-pasteable examples live in `prototypes/_shared/gallery.html`. Read it first if unsure.
2. **Use `<hexa-icon name="…">` and `<hexa-illustration name="…">`** for any iconography. 276 icons + 18 illustrations are available. Names live in `prototypes/_shared/icons.json` and `illustrations.json`. The gallery page filters them visually.
3. **Two stylesheets, one script** — every prototype must include exactly:
   ```html
   <link rel="stylesheet" href="../_shared/hexa-tokens.css" />
   <link rel="stylesheet" href="../_shared/hexa-components.css" />
   <script src="../_shared/hexa.js" defer></script>
   <script src="../_shared/hypothesis-tracker.js" data-prototype-id="…" data-hypothesis="…"></script>
   ```
4. **Zero build, vanilla HTML/CSS/JS only.** No npm, no bundlers, no React. Files must open by double-clicking `index.html`.
5. **One folder per prototype**: `prototypes/<kebab-id>/` containing `index.html`, optional `styles.css` for prototype-specific tweaks, and `HYPOTHESIS.md`.
6. **Never edit `prototypes/_shared/`** unless extending the design system port is the explicit task.
7. **Tag every CTA** the hypothesis depends on with `data-track="…"`. Use stable hyphenated ids like `cta-hero-primary`, `nav-pricing`, `plan-pro`.
8. **No PII tracking by default.** Form values are not captured unless the input has `data-track-value="true"` *and* the value is non-PII (e.g. plan choice).
9. **BEES visual identity.** Every prototype that mimics a BEES product page must use the BEES topbar, footer, and page patterns documented below — not the generic Hexa topbar.
10. **Requirements before scaffold.** Do not copy `prototypes/_template/` until there is an agreed **product requirements** input: a **PRD**, a **HLR** (High Level Requirements / Confluence-style BLK requirements), or a **minimum requirements** brief the user accepts as sufficient. If none exists, ask for it (or offer to draft a one-page minimum-requirements summary from whatever context they have) and get explicit confirmation before scaffolding.

## Workflow

### 1. Gather inputs

#### 1a. Product requirements (expected)

Before screens or code, align on **one** of the following (paste, link, or attach):

| Source | What to extract |
|---|---|
| **PRD** | Problem, users, scope, flows, data fields, edge cases, success criteria, out-of-scope |
| **HLR** | As / I need to / So that statements, mandatory context, acceptance themes, NFRs relevant to the UI |
| **Minimum requirements** | Bullet list: must-have screens, fields, actions, filters, states (empty/loading/error), and what is explicitly **not** in this prototype |

**Agent behaviour**

- If the user only gives a vague idea, **stop and ask** for PRD, HLR, or minimum requirements — or propose a numbered minimum-requirements list for them to edit in one reply, then proceed once they confirm.
- If the user explicitly waives documentation (“no PRD, just build X”), capture that waiver in `HYPOTHESIS.md` under **Assumptions** and still write down the agreed scope in your own words so the prototype stays testable.
- Map confirmed requirements into `HYPOTHESIS.md` (scope, in-scope / out-of-scope, open questions) so reviewers know what the build is validating.

#### 1b. Hypothesis prototype metadata

Ask the user (use AskQuestion when available):

| Field | Example |
|---|---|
| **Hypothesis** | "Showing price upfront on the hero increases sign-up conversion." |
| **Prototype id** (kebab-case, used as folder + storage key) | `pricing-upfront-v1` |
| **Display name** | "Pricing Upfront — V1" |
| **Screens** | Home, Features, Pricing, Signup (default) — or custom list |
| **Tracked CTA ids** | e.g. `cta-hero-primary`, `cta-plan-pro` |
| **Success metric** | e.g. "≥ 15% of visitors click cta-hero-primary" |

If the user gave a free-text request, infer sensible defaults **only after** requirements in **1a** are settled (or waived in writing); confirm hypothesis + id before scaffold.

### 2. Scaffold

Only after **1a** (PRD / HLR / minimum requirements, or documented waiver + agreed scope).

Copy the template and substitute placeholders:

```
prototypes/_template/   →   prototypes/<id>/
```

Substitute in every file:
- `{{PROTOTYPE_ID}}` → kebab-case id
- `{{PROTOTYPE_NAME}}` → display name
- `{{HYPOTHESIS}}` → one-sentence hypothesis

### 3. Customise screens

Each `<section data-screen>` is one screen, navigated via hash routes. Use Hexa primitives:

```html
<section id="home" data-screen class="hexa-screen">
  <h1 class="hexa-display mb-md">Headline</h1>
  <p class="text-body-lg text-fg-neutral-secondary mb-xl">Subhead</p>
  <button class="hexa-btn hexa-btn--primary hexa-btn--lg" data-track="cta-hero-primary">
    Get started
    <hexa-icon name="ArrowRightIcon"></hexa-icon>
  </button>
</section>
```

### 4. Pick icons and illustrations

Look up names in `prototypes/_shared/icons.json` (276 entries — all end with `Icon`, e.g. `CartIcon`, `BellIcon`, `ArrowRightIcon`, `AlertIcon`, `CheckIcon`, `HomeIcon`, `SearchIcon`, `EditIcon`, `DownloadIcon`, `EyeOnIcon`, `EyeOffIcon`, `BarChartIcon`, `TruckIcon`, `AwardIcon`, `FilterIcon`, `MenuIcon`, `CloseIcon`, `SyncIcon`, `InfoIcon`, `BlockedIcon`, `AlertOctagonIcon`, `AlertTriangleIcon`, `ChevronDownIcon`, `ChevronRightIcon`, `ChevronLeftIcon`, `StoreIcon`, `UserIcon`).

Illustrations (18, no `Icon` suffix): `AvailableSoon`, `DateError`, `EmptyBox`, `EmptyTruck`, `ErrorLoading`, `ErrorLoadingData`, `ImageLoadingError`, `NoConnectionFound`, `NoData`, `NoDdc`, `NoPoc`, `NoSearchResults`, `NoSeller`, `ProductImagePlaceholder`, `RedeemProductsError`, `SearchEmpty`, `SomethingWentWrong`, `Unavailable`.

If unsure, tell the user "open `prototypes/_shared/gallery.html` to browse" — that page lets them filter and click-to-copy any icon/illustration tag.

### 5. Track milestone events with HT.track

Click events are auto-captured. For state changes that aren't a single click:

```js
HT.track('plan_selected', { plan: 'pro', source: 'hero' });
HT.track('signup_completed', { source: 'prototype' });
```

### 6. Register the prototype

Read `prototypes/prototypes.json`, append, write back:

```json
{
  "id": "<id>",
  "name": "<display name>",
  "hypothesis": "<one-sentence hypothesis>",
  "createdAt": "<YYYY-MM-DD>",
  "metric": "<primary success metric>"
}
```

### 7. Tell the user how to use it

- Local preview: `powershell -File .serve.ps1` (or `node .serve.cjs` if Node is installed) then `http://127.0.0.1:8765/prototypes/<id>/`
- Live dashboard: `…/prototypes/_shared/dashboard.html?id=<id>`
- GitHub Pages URL after pushing to `main`: `https://<gh-user>.github.io/<repo>/prototypes/<id>/`
- Always use `127.0.0.1` (not `localhost`) so `localStorage` keys match the tracker dashboard.

> **Note:** Node.js may not be available on the machine. A pure PowerShell server `.serve.ps1` is included alongside `.serve.cjs` as a fallback. Try PowerShell first.

---

## BEES design patterns (learned from EDI Central Tracking)

These are the **proven, tested patterns** extracted from real prototyping sessions. Use them instead of inventing new chrome.

### Page background

```css
body { background: #FAFAFA; }
```

All BEES product pages use a subtle light grey background. Cards and tables sit on white (`var(--hexa-surface-primary)`).

### BEES topbar (replaces the default Hexa topbar)

The BEES topbar has two rows and is sticky at the top. **Hide the default Hexa topbar** and render a custom one.

```css
.hexa-topbar { display: none !important; }
```

**Row 1 (primary, 64px tall):** logo PNG (`prototypes/_shared/img/bees-logo.png`), center search bar, account selector pill, icon buttons (user, bell, cart with badge).

**Row 2 (secondary nav, 56px):** left-aligned links (Product categories, Order, Commercial management, Payments), right-aligned links (Help & Support, active page name with yellow underline via `--hexa-brand-500`).

Key CSS values:
- Max-width: `1440px`, horizontal padding: `56px`
- Logo: `height: 28px`, inline `<img>` tag
- Search bar: `background: #F2F2F2`, `border-radius: var(--hexa-radius-pill)`, black circular submit button
- Account pill: `border: 1px solid var(--hexa-border-default)`, `border-radius: var(--hexa-radius-pill)`, uses `StoreIcon`
- Icon buttons: `40×40px`, transparent bg, `border-radius: 50%`, hover: `#F2F2F2`
- Notification badge: `background: #2D7DC1`, white text, `10px` font, positioned absolute top-right
- Active nav link: `border-bottom: 2px solid var(--hexa-brand-500)`, `font-weight: 600`

### App shell (page container)

Use `<main class="app-shell">` instead of `<main class="hexa-screen">` for BEES-style pages:

```css
.app-shell {
  max-width: 1440px;
  margin: 0 auto;
  padding: var(--hexa-space-xl) 56px var(--hexa-space-3xl);
}
```

### Page title

```css
.page-title {
  font-family: var(--hexa-font-header);  /* Barlow */
  font-size: 24px;
  line-height: 32px;
  font-weight: 600;
  letter-spacing: -0.01em;
  margin: 0 0 var(--hexa-space-md);
}
```

### Status pills

Dot-prefixed, pastel-background, pill-shaped badges. Six tones:

| Tone | Background | Text | Dot | Use for |
|------|-----------|------|-----|---------|
| `is-success` | `#E6F4EA` | `#1E6A33` | `#2D7D45` | Accepted, completed, OK |
| `is-warning` | `#FFF4E0` | `#8A4F00` | `#F57C00` | Accepted with alerts, needs attention |
| `is-error` | `#FDECEC` | `#B0211B` | `#D32F2F` | Rejected, failed |
| `is-blocked` | `#EFE7F7` | `#5A2E8E` | `#7B3FBE` | Blocked, on-hold |
| `is-info` | `#E6F0FB` | `#14507A` | `#2D7DC1` | Waiting, processing, in queue |
| `is-neutral` | `#F2F2F2` | `#555555` | `#888888` | Unknown, N/A |

```css
.status-pill {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 4px 12px;
  border-radius: var(--hexa-radius-pill);
  font-size: 13px; font-weight: 600; line-height: 16px;
  white-space: nowrap; flex-shrink: 0;
}
.status-pill::before {
  content: ""; width: 6px; height: 6px; border-radius: 50%;
  flex-shrink: 0;
}
```

### Summary cards row

A grid of KPI summary cards at the top of a list page. Each card shows a colored dot + label, percentage, and count. Cards act as quick-filters when clicked.

```css
.summary-cards {
  display: grid;
  grid-template-columns: repeat(5, minmax(0, 1fr));
  gap: var(--hexa-space-sm);
  margin-bottom: var(--hexa-space-lg);
}
```

Active card: `border-color: #111111; box-shadow: 0 0 0 1px #111111;`. Breakpoints: 720px → 2 cols, 420px → 1 col.

### Collapsible filter panel

A pill-shaped "Filter" toggle button that reveals a grid form when clicked. Filters use a **draft/applied** state pattern: dropdown selections are staged in a `draft` object; only when the user clicks "Apply filters" does the `draft` copy to `applied` and trigger the list to re-render. A red dot (`.is-dirty`) appears on the button when unapplied changes exist.

Filter grid: `grid-template-columns: repeat(3, 1fr)` at desktop; collapses to 2 at 900px and 1 at 560px. Period inputs (start + end date) share one grid cell. Dropdown panels use `position: fixed; z-index: 1000` with JS-driven positioning to avoid overlap issues.

### Data table (single-line rows)

The order list is a **true table layout** with a sticky header row and single-line data rows sharing the same CSS grid template. The wrapping `.order-table` container scrolls horizontally if the viewport is narrower than the `min-width` — rows never wrap to multiple lines.

```css
.order-table {
  background: var(--hexa-surface-primary);
  border: 1px solid var(--hexa-border-default);
  border-radius: var(--hexa-radius-lg) var(--hexa-radius-lg) 0 0;
  overflow-x: auto;
  overflow-y: hidden;
}
.order-list { min-width: 1180px; }  /* horizontal scroll below this */

.order-row {
  display: grid;
  grid-template-columns: /* fixed widths for predictable columns, fr for fluid ones */
    32px 180px 170px minmax(220px, 1.6fr) minmax(140px, 1fr) 140px 180px 96px;
  gap: var(--hexa-space-lg);
  align-items: center;
  padding: var(--hexa-space-md) var(--hexa-space-xl);
  border-bottom: 1px solid var(--hexa-border-default);
}
```

**Header row** (`.order-row--head`): sticky, `background: #F7F7F7`, uppercase 11px labels, `letter-spacing: 0.04em`. Each data cell is `white-space: nowrap; overflow: hidden; text-overflow: ellipsis`. Monospace columns (PO#, BEES order#) use `font-family: ui-monospace, Menlo, monospace; font-size: 13px`.

### Inline row actions

Each data row contains circular icon buttons for secondary actions (e.g., "View summary", "Reprocess") that sit in the last grid column. These use `event.stopPropagation()` to avoid triggering the row's primary click action.

```css
.row-action {
  width: 32px; height: 32px;
  border-radius: 50%;
  background: var(--hexa-surface-primary);
  border: 1px solid var(--hexa-border-default);
}
.row-action--primary { background: #111111; color: #FFFFFF; border-color: #111111; }
```

### Bulk actions (checkboxes + toolbar)

For rows with actionable statuses, a checkbox column is added. A `.bulk-bar` toolbar above the list shows the selection count, "Clear selection" and a primary action button (e.g., "Reprocess selected"). Only rows matching specific statuses get checkboxes; others show a muted dash placeholder. A "Select all on this page" checkbox is provided.

Selection state is stored in a `Set` and persists across pagination.

### Pagination

Compact, right-aligned pagination controls below the table: total count, rows-per-page dropdown (`10/25/50/100`), first/prev/page-numbers/next/last circular buttons. Active page: `background: #111111; color: #FFFFFF`.

### Non-modal drawer (detail panel)

A right-hand sliding panel (`<aside>`) with a separate scrim overlay (`<div class="drawer-scrim">`). **Not** a `<dialog>` — the user can close it via X button, clicking the scrim, or pressing Escape. The drawer doesn't trap focus, allowing the user to interact with the background page if needed.

```css
.order-drawer {
  position: fixed; top: 0; right: 0;
  height: 100vh; width: min(560px, 100vw);
  background: var(--hexa-surface-primary);
  box-shadow: -8px 0 24px rgba(0,0,0,0.10);
  z-index: 210;
  display: flex; flex-direction: column;
}
.drawer-scrim {
  position: fixed; inset: 0;
  background: rgba(0, 0, 0, 0.30);
  z-index: 200;
}
```

Drawer sections use key-value grids (`.kv` — `grid-template-columns: 140px 1fr`), section headings (`h4`, uppercase 13px), and status-tinted alert blocks at the bottom.

### Detail page — dark band header

For detail/sub-pages (e.g., order items), use a dark band flush under the topbar containing a breadcrumb on top and the page title + action buttons below. This breaks out of the `app-shell` padding using negative margins.

```css
.order-details-band {
  background: #1A1A1A;
  color: #FFFFFF;
  margin: calc(-1 * var(--hexa-space-xl)) -56px var(--hexa-space-xl);
  padding: var(--hexa-space-md) 56px var(--hexa-space-xl);
}
```

**Breadcrumb:** `HomeIcon → Parent page → Current page` in light grey (`#B0B0B0`), current crumb in white. Chevron separators (`ChevronRightIcon`, `#6E6E6E`).

**Band buttons:** pill-shaped. Ghost variant (outline, white text on transparent) and primary variant (white bg, dark text).

### Detail page — facts card

Below the dark band, an order facts card with the status pill, reason text (italic), and a responsive key-value grid:

```css
.order-details-grid {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: var(--hexa-space-lg);
}
```

Labels: uppercase 11px, `letter-spacing: 0.04em`, `color: var(--hexa-fg-neutral-tertiary)`. Values: 14px, `color: var(--hexa-fg-neutral-primary)`. Collapses to 2 at 1100px, 1 at 600px.

### Line item cards

Each product/line item is a card with a 3-column layout: **thumbnail | identity | metrics**.

- **Thumbnail:** 56×56 rounded tile with two-letter initials, background tinted to match the line's status tone (amber for alerts, purple for blocked, red for rejected, blue for pending, neutral beige for OK).
- **Identity:** line number label (`LINE 02`, uppercase monospace 11px), status pill, product name (bold 15px), SKU/family/UOM in monospace 12px.
- **Metrics:** three side-by-side values (Requested Qty, Requested Unit Price, Requested line total). When the requested and delivered values diverge, the cell shows the original struck-through + arrow + new value highlighted in amber (`background: #FFF4E0; color: #8A4F00`).
- **Issue note:** tinted box below the main row, with a warning/error/blocked icon and a plain-English user-facing description. No rule names or internal codes — explain what happened in terms the end user understands.

### Issue description copy guidelines

Issue notes should be written for **the retailer reading their order**, not for the engineer who configured the matching rule. Include the actual numbers when possible:

- **Good:** "Unit price was adjusted from $501.00 to $430.00 to match the current contract."
- **Good:** "Only 70% of the requested quantity is available — we will deliver 23 of the 33 CASE requested."
- **Good:** "We could not match this product to the BEES catalog. The order is on hold until the SKU is registered."
- **Bad:** "Rule: Price issue — Price was adjusted to the current contract." (don't expose rule names)
- **Bad:** "PRICE_ADJUSTED" (don't expose internal codes)

### BEES footer

```html
<footer class="bees-footer">
  <div class="bees-footer-inner">
    <div class="bees-footer-col">
      <h4>About BEES</h4>
      <a href="#">Learn about us</a> / <a href="#">Privacy policy</a> / <a href="#">Terms and conditions</a>
    </div>
    <div class="bees-footer-col">
      <h4>Need help?</h4>
      <a href="#">Frequent questions</a> / <a href="#">Contact us</a>
      <p>Email: bees@za.ab-inbev.com / Tel: +27 860 000722</p>
    </div>
    <div class="bees-footer-col">
      <h4>Schedule</h4>
      <p>Monday to Friday: 7am – 5pm / Weekends: 8am – 2pm</p>
    </div>
  </div>
  <div class="bees-footer-bottom">
    <img src="../_shared/img/bees-logo.png" alt="BEES" />
    <p>© AB InBev 2026. All rights reserved.</p>
    <a href="#">Cookie settings</a>
  </div>
</footer>
```

Footer styling: `background: var(--hexa-surface-primary)`, 3-column grid above, centered logo + copyright below. Max-width `1440px`, padding `56px`.

### Hash-based routing

Multi-screen prototypes use hash routing. The `showScreen()` function hides all `[data-screen]` sections and reveals the one matching the hash. Sub-pages like `#order/<id>` are parsed with `location.hash.startsWith('#order/')`.

### Mock data generation

For prototypes that need realistic lists, use a `data.js` file that generates deterministic-looking random data using a seeded PRNG. This keeps mock data consistent across page reloads while still looking realistic.

### Row flash animation

When an action changes a row's state (e.g., reprocess), flash the row briefly:

```css
@keyframes hexa-row-flash {
  from { background: rgba(255, 192, 0, 0.20); }
  to { background: transparent; }
}
.order-row.flash { animation: hexa-row-flash 1.6s ease-out; }
```

---

## Hexa quick reference

### Spacing tokens
`2xs` (2px) · `xs` (4) · `sm` (8) · `md` (12) · `lg` (16) · `xl` (24) · `2xl` (32) · `3xl` (48) · `4xl` (64)

Used in: `gap-md`, `p-lg`, `mb-xl`, `mt-sm`, `px-md`, `py-xs`, etc.

### Type ramp
- Headers (Barlow): `.hexa-display`, `.hexa-headline`, `h1`–`h6`
- Body (Work Sans): `.text-body-lg/md/sm/xs`, `.text-body-bold-lg/md/sm/xs`, `.text-caption`

### Colors (semantic)
- Foregrounds: `text-fg-neutral-primary/secondary/tertiary`
- Surfaces: `bg-surface-primary/secondary/tertiary/inverse`
- Brand: yellow Bees palette via `--hexa-brand-*` (avoid raw hex; use tokens).
- Status tones: success (`#E6F4EA` / `#1E6A33`), warning (`#FFF4E0` / `#8A4F00`), error (`#FDECEC` / `#B0211B`), blocked (`#EFE7F7` / `#5A2E8E`), info (`#E6F0FB` / `#14507A`)
- Dark surfaces: `#1A1A1A` for dark bands, `#111111` for primary dark buttons
- Light grey page background: `#FAFAFA`

### Components (always prefer these classes)

| Need | Use |
|---|---|
| Button | `<button class="hexa-btn hexa-btn--{primary,secondary,tertiary,destructive} hexa-btn--{sm,md,lg}">` |
| Icon button | `<button class="hexa-icon-btn"><hexa-icon name="…"/></button>` |
| Text/link button | `<button class="hexa-text-btn">` |
| Input | `<label class="hexa-field"><span class="hexa-label">…</span><span class="hexa-input-wrap"><input class="hexa-input"/></span></label>` |
| Select | `<select class="hexa-select">` |
| Textarea | `<textarea class="hexa-textarea">` |
| Checkbox | `<label class="hexa-check"><input type="checkbox"/> …</label>` |
| Radio | `<label class="hexa-radio"><input type="radio"/> …</label>` |
| Toggle | `<label class="hexa-toggle"><input type="checkbox"/></label>` |
| Card | `<article class="hexa-card [hexa-card--brand|--success|--warning|--error|--elevation|--clickable]">` |
| Alert | `<div class="hexa-alert hexa-alert--{info,success,warning,error}">` |
| Tag | `<span class="hexa-tag [hexa-tag--brand|--success|--warning|--error]">` |
| Status badge | `<span class="hexa-status-badge hexa-status-badge--{success,warning,error,info}">` |
| Status pill | `<span class="status-pill is-{success,warning,error,blocked,info,neutral}">Label</span>` |
| Notification badge | `<span class="hexa-badge">3</span>` (or `.hexa-badge--dot`) |
| Chip | `<button class="hexa-chip" [aria-pressed="true"]>` |
| Tabs | `<div class="hexa-tabs"><button class="hexa-tab" aria-selected="true">…</button>…</div>` |
| Segmented control | `<div class="hexa-segments"><button aria-pressed="true">…</button>…</div>` |
| Avatar | `<span class="hexa-avatar [--sm|--lg]">JD</span>` |
| Spinner / progress / skeleton | `.hexa-spinner` / `.hexa-progress > span` / `.hexa-skeleton` |
| Tooltip | Add `class="hexa-tooltip" data-tooltip="…"` to any wrapper |
| Dialog | `<dialog class="hexa-dialog">…</dialog>` |
| Empty state | `<div class="hexa-empty"><hexa-illustration name="…"/><h3>…</h3><p>…</p></div>` |
| Top bar (generic) | `<header class="hexa-topbar"><a class="hexa-brand">…</a><nav>…</nav><div class="hexa-topbar-actions">…</div></header>` |
| Top bar (BEES) | Custom `.bees-topbar` with two rows (see BEES topbar pattern above) |
| Page container (generic) | `<main class="hexa-screen">` (max-width 1080) |
| Page container (BEES) | `<main class="app-shell">` (max-width 1440, padding 56px) |

### Layout helpers (Tailwind-style subset)
`flex`, `flex-col`, `items-center`, `justify-center/between/end`, `grid`, `grid-cols-2/3/4`, `place-items-center`, `w-full`, `w-fit`, `min-h-screen`, `cursor-pointer`.

### Key dimensions

| Element | Value |
|---------|-------|
| Max content width | `1440px` |
| Horizontal page padding | `56px` |
| Topbar primary row height | `64px` |
| Topbar secondary row height | `56px` |
| Row action button | `32×32px`, `border-radius: 50%` |
| Topbar icon button | `40×40px`, `border-radius: 50%` |
| Pill border-radius | `var(--hexa-radius-pill)` |
| Form field border-radius | `var(--hexa-radius-pill)` |
| Card/panel border-radius | `var(--hexa-radius-lg)` |

---

## Output checklist

Before handing back to the user, verify:

- [ ] **Requirements traceability:** `HYPOTHESIS.md` states the source (PRD / HLR / minimum requirements) or documents a waiver plus agreed scope; scope matches what was scaffolded.
- [ ] `prototypes/<id>/index.html` references `../_shared/hexa-tokens.css`, `../_shared/hexa-components.css`, `../_shared/hexa.js`, and `../_shared/hypothesis-tracker.js` with `data-prototype-id="<id>"` + `data-hypothesis="…"`.
- [ ] Uses the BEES topbar pattern (not the generic Hexa topbar) if mimicking a BEES product page.
- [ ] Uses `<main class="app-shell">` with `max-width: 1440px` and `padding: 56px` for BEES pages.
- [ ] Page background is `#FAFAFA`.
- [ ] Status indicators use `.status-pill` with the correct tone class.
- [ ] Data tables use the single-line row pattern with a sticky header and horizontal scroll.
- [ ] Drawers/panels use non-modal `<aside>` + scrim, not `<dialog>`.
- [ ] Issue/alert descriptions are written in plain English for end users — no rule names or internal codes.
- [ ] No raw `<button>` / `<input>` / `<select>` / `<textarea>` styled with bespoke CSS — all use `hexa-*` classes.
- [ ] Every interactive CTA has a `data-track` attribute.
- [ ] Icons use `<hexa-icon name="…">` (with `Icon` suffix for the name).
- [ ] `HYPOTHESIS.md` documents hypothesis, target metric, and out-of-scope.
- [ ] `prototypes/prototypes.json` includes the new entry.
- [ ] BEES footer is included with logo, copyright, and standard links.
- [ ] You told the user the local URL, the dashboard URL, and the GitHub Pages URL.
