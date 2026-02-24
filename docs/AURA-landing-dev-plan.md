# AURA Landing Page – Development Plan

**Figma:** [Desktop 608:4](https://www.figma.com/design/Fe537d1xdyHVElxwTjy353/Chamelo?node-id=608-4&m=dev) · [Mobile 721:4](https://www.figma.com/design/Fe537d1xdyHVElxwTjy353/Chamelo?node-id=721-4&m=dev)  
**Target URL:** `https://your-store.myshopify.com/products/<aura-handle>?view=aura-v2`

This plan follows the existing Home v2 / AROZA patterns and applies **audit recommendations 2, 3, and 4** from the landing-page audit.

---

## 1. Decisions from audit (2, 3, 4)

### 2. Align “Learn more” link
- **Decision:** Use **one** canonical tech/innovation URL for all “LEARN MORE” / “Discover more” CTAs.
- **Recommendation:** Use `/pages/innovation` (match Home v2). Update AROZA Features to the same in a separate change if not already done.
- **In AURA:** Every section that has a “Learn more” / “Discover more” CTA (e.g. features, technology, color-for-mood) will use a single section setting or shared value pointing to `/pages/innovation`. No mixing with `/pages/technology`.

### 3. Quote bar and `quote_logo_text`
- **Decision:** Support **both** logo image and text fallback from the start (same as AROZA hero).
- **In AURA:** `aura-hero-new` schema will include:
  - `quote_text`
  - `quote_logo` (image_picker)
  - `quote_logo_text` (text, for when no logo image)
- Liquid: if `quote_logo` present → show image; else if `quote_logo_text` present → show text. No template keys that don’t exist in schema.

### 4. Container strategy (and consistency)
- **Decision:** AURA matches **AROZA**: use theme **`page-width`** for all sections that need a contained width (product info, stats, reviews, collections, features, community, newsletter, FAQ).
- **Do not** introduce custom max-width wrappers (e.g. 1280px + 120px padding) for AURA; keep layout consistent with AROZA and avoid future “which approach?” confusion.
- **Document:** In this doc and, if useful, in theme conventions: “Product landing pages (AROZA, AURA) use `page-width`; Home v2 uses custom section wrappers.”

---

## 2. Template and URL

| Item | Value |
|------|--------|
| Template file | `templates/product.aura-v2.json` |
| View parameter | `?view=aura-v2` |
| Product handle | TBD (e.g. main AURA product; set in template section settings) |

Template will reference **only** `aura-*-new` sections and follow the section order below.

---

## 3. Section list (from Figma)

Sections are ordered as they appear on desktop; mobile may reorder or collapse content within the same sections.

| # | Section | Purpose | Reference |
|---|---------|---------|------------|
| 1 | **aura-hero-new** | “Change Color With A Tap” hero, gradient, CTA(s), quote bar | aroza-hero-new (quote bar with quote_logo_text) |
| 2 | **aura-product-info-new** | Gallery, title, price, variant selectors (color, size), quantity, Add to Cart, trust badges | aroza-product-info-new |
| 3 | **aura-stats-new** (optional) | Key stats (e.g. tint speed, battery, protection) if in design | aroza-stats-new |
| 4 | **aura-features-accordion-new** | “Why Chamelo?” – accordion: Change color, Crystal-clear vision, Smart-tint, UV400+ | New; accordion pattern from theme/AROZA |
| 5 | **aura-reviews-new** | “Real Chamelo Love” – UGC grid + optional “Upload your photo” | aroza-reviews-new |
| 6 | **aura-press-new** | Press logos band (Forbes, WWD, Allure, etc.) | Index-press-cards-new (simplified: logos only) |
| 7 | **aura-technology-new** | “Effortlessly Switch Up Your Style” + “A Color For Every Mood” (two content blocks or two subsections) | aroza-features-new + second block; single “Learn more” link → /pages/innovation |
| 8 | **aura-collections-new** | “Shop by Collections” – e.g. Trail, Snow, Lifestyle | aroza-collections-new |
| 9 | **aura-community-new** | “Say Hello To Chamelo” – Instagram grid + CTA | aroza-community-new |
| 10 | **aura-faq-new** | FAQs accordion + “View All FAQs” | New; reuse theme FAQ accordion pattern |
| 11 | **aura-newsletter-new** | “Sign up and save 10%” / Stay in the loop | aroza-newsletter-new |

**Sections to create (files):**

- `sections/aura-hero-new.liquid` + `assets/aura-hero-new.css`
- `sections/aura-product-info-new.liquid` + `assets/aura-product-info-new.css`
- `sections/aura-stats-new.liquid` + `assets/aura-stats-new.css` (if needed)
- `sections/aura-features-accordion-new.liquid` + `assets/aura-features-accordion-new.css`
- `sections/aura-reviews-new.liquid` + `assets/aura-reviews-new.css`
- `sections/aura-press-new.liquid` + `assets/aura-press-new.css`
- `sections/aura-technology-new.liquid` + `assets/aura-technology-new.css`
- `sections/aura-collections-new.liquid` + `assets/aura-collections-new.css`
- `sections/aura-community-new.liquid` + `assets/aura-community-new.css`
- `sections/aura-faq-new.liquid` + `assets/aura-faq-new.css`
- `sections/aura-newsletter-new.liquid` + `assets/aura-newsletter-new.css`

---

## 4. Naming and code conventions

- **Section files:** `aura-<feature>-new.liquid` (kebab-case).
- **CSS classes:** `Aura_<feature>_<element>_new` (underscores, `_new` suffix).
- **Assets:** `aura-<feature>-new.css` (lowercase kebab).
- **Responsive:** `Desktop_only_new`, `Mobile_only_new`.
- **Structure:** CSS link at top → markup with `{%- if -%}` blank checks → schema at end. Use `page-width` for contained sections (per §4 above).

---

## 5. Schema and content defaults

- **Hero:** heading, subtitle, desktop/mobile image or video, primary CTA (product or URL), `quote_text`, `quote_logo`, `quote_logo_text` (all in schema).
- **Technology / features:** one `button_link` (or single “Learn more” URL) defaulting to `/pages/innovation`.
- **Reviews:** heading, collection (e.g. metaobject or collection for AURA reviews).
- **Press:** blocks or settings for logo image + optional link.
- **FAQ:** blocks for question + answer; “View All FAQs” link to /pages/faq or equivalent.

---

## 6. Deploy safety (rule 4)

After implementation, add to **`.cursor/rules/shopify-deploy-safety.mdc`**:

**Templates**
- `templates/product.aura-v2.json` (AURA product landing)

**Sections**
- Each `sections/aura-*-new.liquid` listed in §3

**Assets**
- Each `assets/aura-*-new.css` for the sections above

Push only these files (and existing allowed files) when deploying; use `--only` for theme push.

---

## 7. Build order (suggested)

1. **Template + hero** – `product.aura-v2.json`, `aura-hero-new` (with quote bar and quote_logo_text).
2. **Product info** – `aura-product-info-new` (gallery, variants, Add to Cart).
3. **Stats** (if in scope) – `aura-stats-new`.
4. **Features accordion** – `aura-features-accordion-new`.
5. **Reviews + press** – `aura-reviews-new`, `aura-press-new`.
6. **Technology** – `aura-technology-new` (both “Effortlessly…” and “A Color For Every Mood”; single Learn more → `/pages/innovation`).
7. **Collections + community** – `aura-collections-new`, `aura-community-new`.
8. **FAQ + newsletter** – `aura-faq-new`, `aura-newsletter-new`.
9. **Polish** – responsive pass, then add all new files to deploy-safety allowed list.

---

## 8. Out of scope (do not change)

- `templates/index.json`, existing sections/snippets/assets, `layout/theme.liquid`, `config/settings_data.json`.
- Changing Home v2 or AROZA section logic; only add AURA sections and template, and optionally fix AROZA “Learn more” link in a separate task.

---

## 9. Figma reference

- **Desktop:** [Chamelo – node 608:4](https://www.figma.com/design/Fe537d1xdyHVElxwTjy353/Chamelo?node-id=608-4&m=dev)
- **Mobile:** [Chamelo – node 721:4](https://www.figma.com/design/Fe537d1xdyHVElxwTjy353/Chamelo?node-id=721-4&m=dev)

Use these for layout, copy, and breakpoints; implement with `page-width` and the conventions above so AURA stays consistent with AROZA and audit 2–4 are satisfied.
