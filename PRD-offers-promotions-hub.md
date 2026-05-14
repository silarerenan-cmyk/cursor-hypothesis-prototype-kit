# PRD — Offers & Promotions Hub

**Product:** BEES Link
**Section:** Commercial Management
**Status:** Draft — example PRD for prototype builder demo
**Author:** Product Management (BRE squad)
**Last updated:** 2026-05-14

---

## 1. Overview

BEES Link currently displays promotions and commercial offers scattered across the home banner and category pages. Retailers (points of consumption — POCs) have no single place to browse all available offers, check expiry dates, or activate a promotion with a single action.

This document describes a **new Offers & Promotions Hub page** inside the "Commercial Management" menu of BEES Link. The page consolidates all active, upcoming, and expiring offers into a filterable list, so retailers can discover and act on commercial opportunities without navigating multiple sections.

---

## 2. Problem statement

> "As a BEES Link retailer, I don't know what promotions I'm eligible for right now — I only find out about them when I accidentally land on a product page or when a sales rep calls me."

Key pain points confirmed in user research:
- Offers are buried in category pages; no central discovery surface.
- Retailers miss short-window promotions because there is no proactive visibility or expiry signal.
- Activating an offer requires going through checkout — there is no lightweight "I'm interested" flow.

---

## 3. Goals

| Goal | Metric |
|------|--------|
| Give retailers a single place to discover all available offers | ≥ 70% of active retailers visit the Hub at least once per month |
| Reduce missed-offer rate | ≥ 50% of sessions that open the Hub click at least one offer card |
| Shorten time to first offer activation | Median time from Hub page load to "Activate" CTA click < 90 seconds |

---

## 4. Non-goals (this version)

- Push notifications or email alerts about new offers.
- Personalised offer recommendations (ML/AI ranking).
- Negotiation or price-bargaining flows.
- Multi-brand comparison or bundle builder.
- Mobile app version (web only for this prototype).

---

## 5. Users

| Persona | Description |
|---------|-------------|
| **Retailer (POC owner / buyer)** | The primary user — logs into BEES Link to manage purchases. Wants quick visibility into what deals are available and how much time they have left to act. |
| **Key Account (KA) manager** | Secondary — may demo the Hub to retailers during in-person visits; monitors which offers are being activated. |

---

## 6. User stories

**US-1 — Browse all offers**
*As a retailer, I need to see all current promotions available to me in one place, so that I don't miss commercial opportunities.*

**US-2 — Filter by status and category**
*As a retailer, I need to filter offers by status (Active / Expiring Soon / New / Used) and product category (Beer / Non-Alcoholic / Spirits), so that I can focus on what's most relevant.*

**US-3 — See offer details before committing**
*As a retailer, I need to read the full offer terms, discount value, and expiry date in a side panel without leaving the list, so that I can evaluate multiple offers quickly.*

**US-4 — Activate an offer**
*As a retailer, I need to activate a promotion with a single click, so that the discount is applied automatically on my next qualifying order.*

**US-5 — Understand offer status at a glance**
*As a retailer, I need a clear visual indicator (status pill + count card) that tells me how many offers are expiring this week, so that I can prioritise the ones with the shortest window.*

---

## 7. Feature scope

### 7.1 Page entry point

- Available under the **"Commercial Management"** section in the BEES Link top navigation bar.
- Page name: **"Offers"** (secondary nav link, with the active-page yellow underline on selection).

### 7.2 Page structure

#### Summary cards row
A row of 4 cards at the top of the page, each clickable as a quick-filter:

| Card | Status | Description |
|------|--------|-------------|
| **All** | — | Total number of available offers |
| **New** | `is-info` | Offers added in the last 7 days |
| **Expiring Soon** | `is-warning` | Offers that expire within 7 days |
| **Active** | `is-success` | Offers the retailer has already activated |
| **Used / Expired** | `is-neutral` | Closed offers (for reference) |

#### Filter panel
A collapsible filter panel with a pill-shaped "Filter" toggle button:
- **Status** — multi-select dropdown (New / Expiring Soon / Active / Used)
- **Product category** — multi-select dropdown (Beer / Non-Alcoholic / Spirits / Snacks)
- **Brand** — multi-select dropdown (Budweiser / Corona / Stella Artois / Spaten / Becks / Other)
- **Expiry period** — date range picker (Start date / End date)
- **Apply filters** + **Clear all** buttons
- A red dot indicator on the button when unapplied changes exist in the draft state.

#### Offer list table
A list of offer cards in a data-table layout (single-line rows):

| Column | Content |
|--------|---------|
| Offer name | Bold text, 15px |
| Brand | Brand name + logo placeholder |
| Category | Text label |
| Discount | e.g. "15% off" or "Buy 10, get 2 free" |
| Minimum order | e.g. "Min. 5 CASE" |
| Expiry date | Formatted date, red if < 7 days away |
| Status | Status pill (New / Expiring Soon / Active / Used) |
| Actions | "View details" icon button + "Activate" primary button |

Table behavior:
- Sticky header row.
- Horizontal scroll below 1180px.
- Clicking a row opens the detail drawer.
- Rows per page: 10 / 25 / 50 (default 10), with pagination controls below.

#### Detail drawer (non-modal)
A right-hand sliding panel (560px wide) that opens when a row is clicked:

Sections:
1. **Header** — Offer name, brand logo, status pill, and "Activate" CTA.
2. **Key facts** (key-value grid) — Discount value, minimum order quantity, eligible products, period, offer code.
3. **Terms & conditions** — Plain-English description of how the discount is applied and any restrictions.
4. **Eligible SKUs** — Compact list of qualifying products (name + SKU + unit).
5. **Status alert** — Contextual message:
   - *Expiring Soon:* "This offer expires in 4 days. Activate before [date] to lock in the discount."
   - *Active:* "You activated this offer on [date]. The discount applies automatically."
   - *New:* "This offer is new — activate it now before it's claimed by other buyers in your area."

Drawer behavior:
- Closes via X button, clicking the scrim overlay, or pressing Escape.
- Does not trap focus (background page remains interactive).

### 7.3 Interactions

| Action | Trigger | Result |
|--------|---------|--------|
| **Open drawer** | Click any row or "View details" icon | Detail drawer slides in from the right; scrim appears |
| **Close drawer** | X button / scrim click / Escape key | Drawer slides out; scrim fades |
| **Activate offer** | Click "Activate" in list row or drawer | Row flashes (amber), status pill changes to "Active", button becomes disabled with "Activated ✓" label |
| **Apply filters** | Click "Apply filters" in filter panel | List re-renders to matching rows; filter panel closes |
| **Clear filters** | Click "Clear all" | All filter fields reset; list returns to full set |
| **Quick-filter by card** | Click a summary card | Filter auto-applies for that status; card gets active border |

---

## 8. UX states

| State | Trigger | Display |
|-------|---------|---------|
| **Loading** | Initial page load | Skeleton rows in the table |
| **Empty (no offers)** | No offers match the active filters | Illustration (`NoSearchResults`) + "No offers match your filters" + "Clear filters" link |
| **Empty (no eligible offers)** | Account has no offers | Illustration (`EmptyBox`) + "No offers available right now. Check back soon." |
| **Error** | API failure | Illustration (`ErrorLoadingData`) + "We couldn't load your offers. Try again." + "Retry" button |
| **Activated** | User clicks "Activate" | Inline row flash animation + status pill update (no full-page reload) |

---

## 9. Non-functional requirements

| NFR | Target |
|-----|--------|
| **Performance** | Page load (LCP) < 2.5 s on a 4G connection |
| **Accessibility** | WCAG 2.1 AA — keyboard navigable, screen-reader labels on all interactive elements |
| **Responsiveness** | Functional at 1440px (desktop), 1024px (tablet); horizontal scroll for the table below 1180px |
| **Browser support** | Latest Chrome, Firefox, Edge, Safari |

---

## 10. Success metrics

| # | Metric | Type | Threshold |
|---|--------|------|-----------|
| 1 | Offer card click-through rate | Primary conversion | ≥ 50% of Hub sessions click at least one offer row or card |
| 2 | Hub page discovery | Discovery | ≥ 70% of active retailers open the Hub at least once within 30 days of launch |
| 3 | Offer activation rate | Task completion | ≥ 25% of sessions that open the detail drawer also click "Activate" |
| 4 | Filter panel engagement | Engagement | Median ≥ 1 filter interaction per session |
| 5 | Time to first activation | Time-to-action | Median < 90 seconds from Hub pageview to "Activate" click |

---

## 11. Open questions

| # | Question | Owner | Due |
|---|----------|-------|-----|
| Q1 | What is the maximum number of simultaneous active offers a retailer can hold? | Commercial / BRE | TBD |
| Q2 | Should "Activate" require a confirmation dialog, or is a single click sufficient? | UX Research | TBD |
| Q3 | Can a retailer de-activate an offer after activating it? | Business rules / Commercial | TBD |
| Q4 | Are offer results already paginated server-side, or is all data returned in one payload? | Backend engineering | TBD |
| Q5 | Should the "Expiring Soon" threshold be configurable by region? | Operations | TBD |

---

## 12. Out of scope

- Push / in-app notification when a new offer appears.
- Offer creation or editing (back-office / KA-facing tooling).
- Integration with the checkout flow (out of scope for this prototype — "Activate" is a standalone action).
- Analytics dashboard for KA managers tracking offer adoption.
- Any personalisation or recommendation algorithm.

---

## 13. Prototype scope (for Design team demo)

This PRD is the input for the **Cursor prototype builder agent**. The prototype should cover:

**Screens / states to build:**
1. **Offers Hub** — Main list page with summary cards, filter panel (collapsed by default), and the full offer table with mock data (≥ 8 rows across 3 statuses).
2. **Detail drawer** — Slides in when a row is clicked; all sections populated with mock data.
3. **Activated state** — Row flash + status pill change after clicking "Activate".
4. **Empty state** — Triggered when all filters are applied and no rows match (e.g. filter for "Used" + "Spirits" with no matching mock data).

**Navigation:**
- BEES topbar with "Commercial Management" active in secondary nav, "Offers" highlighted with yellow underline.
- The topbar links should be navigable (at minimum, clicking "Order" navigates to a placeholder screen; other links are inert).

**Not required in prototype:**
- Real API calls (all data is mocked in `data.js`).
- Pagination beyond page 1.
- The "Retry" error state (can be a stretch goal).
- Mobile layout.

---

*End of PRD*
