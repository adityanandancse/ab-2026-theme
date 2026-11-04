# DESIGN-MAP.md — Live (Prestige) → AB 2026 (Horizon) matching guide

Read-only analysis. No Prestige code is reproduced here — values only.

- **Live / visual reference:** `../live-achin-bindlish` — Prestige 1.1.1 (Maestrooo). Vintage architecture: one global colour set, SCSS-compiled tokens (`assets/theme.scss.liquid`), header/footer/announcement stored in `config/settings_data.json → current.sections`, most templates are `.liquid`.
- **Target:** `./dev-dark-coffee` — "AB 2026" 1.0.0 (custom, Shopify **Horizon** architecture). Named colour schemes in `config/settings_data.json → current.color_schemes`, CSS custom properties in `snippets/theme-styles-variables.liquid` + `snippets/color-palette.liquid` + Shopify-native `color_scheme.css`, JSON templates, `blocks/` + section groups.

> **Headline finding:** AB 2026 has **already been themed to the Prestige "dark coffee" look** — same palette (`#241A14` / `#FFF6E4` / gold `#DBBF7C`), same fonts (Playfair Display + Lora, both overridden to self-hosted Mulish), same logo, square buttons, drawer cart. Most colour/type/radius tokens are therefore **"none"** gaps. The real remaining work is (1) a few setting nudges, (2) per-button letter-spacing/uppercase CSS, and (3) rebuilding ~3 live sections that have no Horizon equivalent (map/location, testimonials, custom runtime widgets).

---

## a) Design token table

Legend for **Gap**: `none` = already matches · `setting` = merchant setting change only · `CSS` = needs a CSS custom-property / rule change · `section` = needs a new/rebuilt section or block.

### Colour

| Design token | Live value (Prestige) | AB 2026 setting / CSS variable | Gap |
|---|---|---|---|
| Page background | `#241A14` (`background`) | scheme-1 `background` → `--color-background`; also `page_background_color` | none |
| Body / heading text | `#FFF6E4` (`text_color`, `heading_color`) | scheme-1 `foreground`/`foreground_heading` → `--color-foreground` | none |
| Muted / light text | `#B5A493` (`text_light_color`) | `--color-foreground-subdued` / `-muted` (derived) ; live uses explicit `#B5A493` | CSS (verify muted tone matches `#B5A493`) |
| Accent / links | `#DBBF7C` gold (`link_color`) | scheme-1 `primary` `#DBBF7C` → `--color-primary` | none |
| On-sale / price accent | `#DBBF7C` (`product_on_sale_color`) | `badge_sale_*`, price colour vars | setting (point sale badge/price at gold) |
| Button background | `#DBBF7C` (`button_background`) | scheme-1 `primary_button_background` `#DBBF7C` | none |
| Button text | `#241A14` (`button_text_color`) | scheme-1 `primary_button_text` `#241A14` | none |
| Secondary button | (outline hover-wipe) | scheme `secondary_button_background: rgba(0,0,0,0)` + border | none (transparent/outline already) |
| Body border line | computed: bg mixed 85% → text | scheme-1 `border` `#4A382C` → `--color-border` | CSS (Prestige line is lighter tan; AB border is dark `#4A382C`) |
| Header background | `#241A14` (`header_background`) | header group uses `#281111` bg on menu/announcement | setting/CSS (AB header is `#281111`, slightly darker than live `#241A14`) |
| Header text / icons | `#FFF6E4` (`header_heading_color`) | header-menu `text #eae6e6` | setting (near-match; align to `#FFF6E4`) |
| Footer background | `#16100D` (`footer_background`) | footer group bg `#281111` | setting/CSS (live footer is near-black `#16100D`; AB is `#281111`) |
| Footer text | `#B5A493` (`footer_text_color`) | footer-copyright/policy text `#e7ce93`; menus `#c9a250` | setting (AB footer text is gold-ish, live is taupe) |
| Light/section bg | `#33251C` (`light_background`) | scheme-3 / scheme-6 `background #33251C` | none |
| Popup/modal bg | `#33251C` (`newsletter_popup_*`, `secondary_elements_*`) | `popover_background_color`, `drawer_background_color` | setting (verify popovers point at `#33251C`) |
| page_text_color (global) | n/a (per-element in Prestige) | `page_text_color` currently `#C9A250` (gold) | CSS note — AB's global text is gold `#C9A250`, but scheme fg is cream `#FFF6E4`; confirm intended default |

### Typography

| Design token | Live value | AB 2026 setting / CSS variable | Gap |
|---|---|---|---|
| Heading font | `playfair_display_n4` (serif 400) | `type_heading_font` = `playfair_display_n4` (→ Mulish override in `snippets/fonts.liquid`) | none (handle matches; note Mulish override) |
| Body font | `lora_n4` (serif 400) | `type_body_font` = `lora_n4` | none |
| Subheading / accent font | — (heading font reused) | `type_subheading_font`/`type_accent_font` = `lora_n4` | none |
| Base body size | **15px** (`base_text_font_size`) | `type_size_paragraph` = **16** | setting (set paragraph 14–16; live is 15) |
| Body line-height | 1.65 | `type_line_height_paragraph` = body-loose | none (close) |
| Heading scale | "normal" → h1 22 / h2 20 / h3 18 / h4 16 px | `type_size_h1..h6` = 48/40/24/20/14/12 | setting — **AB headings are much larger**; Prestige headings are very small (22px h1). Decide whether to shrink AB or keep (see plan) |
| Uppercase headings | **false** (no uppercase, no extra tracking) | `type_case_h1..h6` = none | none |
| Heading weight/style | 400 / normal | derived `--font-heading--weight` | none |
| Heading letter-spacing | normal (none; only if uppercase) | `type_letter_spacing_h*` = heading-tight (h1/h2) / normal | setting (minor) |

### Buttons / inputs / cards

| Design token | Live value | AB 2026 setting / CSS variable | Gap |
|---|---|---|---|
| Button radius | **0** (square) | `button_border_radius_primary/secondary` = **0** (schema default 100) | none (already overridden to 0) |
| Button border width | 1px (outline on hover-wipe) | `primary/secondary_button_border_width` = 0 / 0 | setting (secondary could be 1 for outline look) |
| Button padding | 14px 28px | `--button-padding-block 16px` / `--button-padding-inline 24px` | CSS (close; tune to 14/28 if exact) |
| Button text size | 12px | button font preset (accent) | none/CSS |
| Button text-transform | **uppercase** (always) | `button_text_case_primary/secondary` = default | **setting** → set to `uppercase` |
| Button letter-spacing | **0.2em** | no per-button tracking var; `--letter-spacing-sm 0.06em` | **CSS** — add `letter-spacing` to `.button` (~0.2em) |
| Button hover | gold fill wipes to outlined gold text (`::before` scaleX anim, 0.45s) | `card_hover_effect` is for cards; button hover remaps to `--color-primary-button-hover-*` | **CSS** if the wipe animation is desired (Horizon uses colour-swap hover, not wipe) |
| Input radius | **0** | `inputs_border_radius` = **4** → `--style-border-radius-inputs` | setting → set 0 |
| Input border | 1px solid border-color, transparent bg | `input_border_width` 1, `input_background: rgba(0,0,0,0)` | none |
| Input padding | 12px 14px | `--input-padding-x/y 0.8rem` | none (close) |
| Card radius | 0 (Prestige cards are square) | `product_corner_radius` 0, `card_corner_radius` 0 | none |
| Card hover | secondary image / product video on hover | `card_hover_effect` = lift; product `card_hover` gallery setting | setting (enable second-image/zoom on hover to match) |
| Pills radius | n/a | `pills_border_radius` = 32 (schema default 40) | none (cosmetic) |

### Layout

| Design token | Live value | AB 2026 setting / CSS variable | Gap |
|---|---|---|---|
| Page / container width | `Container--narrow` **1420px (~88.75rem)**; extra-narrow 800px | `page_width` = **normal (120rem)**; options narrow 90rem / normal / wide 150rem → `--page-content-width` | **setting** → set `narrow` (90rem ≈ Prestige 1420px) for a closer match |
| Horizontal gutters | 24 / 50 / 80px (mobile/tablet/desktop) | `--page-margin` (max(16px…)/40px) | CSS (desktop gutter smaller in AB; tune if needed) |
| Product grid H-gap | **100px** (`product_list_horizontal_spacing` extra_large) | `--gap-*` scale (fixed) / product-list section settings | setting/CSS (AB gaps are tighter; widen on product-list) |
| Product grid V-gap | 40–50px (extra_small) | `--gap-*` / section settings | setting |
| Section spacing | per-section (Prestige fixed) | `--section-height-*`, `--spacing-scale`, per-section padding | setting per section |
| Grid per row | collection 4 desktop / 2 mobile | product-list / main-collection section settings | setting (match 4/2) |

### Header / footer

| Design token | Live value | AB 2026 setting / CSS variable | Gap |
|---|---|---|---|
| Logo | `fto-logo-white_3.svg`, max-width **115px** | `logo` = same svg; `logo_height` 50 (mobile 51) + `logo_inverse` | setting (align logo height ~115px look) |
| Logo position | centered nav (`navigation_style: center`) | header `logo_position: center`, menu_position left | none (center matches) |
| Menu style | centered inline + sidebar drawer | header-menu `menu_style: featured_collections`, drawer accordion | CSS/setting (AB menu has featured-collection mega style; live is plain centered) |
| Sticky header | **false** (not sticky) | `enable_sticky_header: always` | **setting** → set off to match live, OR keep sticky as an improvement (decide) |
| Transparent on home | **false** | `enable_transparent_header_home: true` | **setting** → live is NOT transparent; set off to match (or keep) |
| Announcement bar | **disabled** (`enable_bar: false`) | header-announcements active ("Festive Season with ab"), bg `#281111` | **setting** → live has it off; AB has it on (keep if desired, else disable) |
| Footer columns | Text + 2 link menus + Newsletter | footer group: FAB widget + 3 menus + utilities (copyright/policies/social) | section (structure differs — see §c) |
| Payment icons | shown (`show_payment_methods: true`) | `payment-icons` block available | setting/section (add payment-icons block) |

### Product card / product page

| Design token | Live value | AB 2026 setting / CSS variable | Gap |
|---|---|---|---|
| Card image ratio | **natural** (`product_image_size`) | product-card / `_product-card-gallery` ratio setting | setting (set to natural/adapt) |
| Image hover | second image / product video (`product_show_secondary_image: true`) | card gallery hover setting + `card_hover_effect` | setting |
| Info alignment | center | product-card content alignment | setting |
| Price on hover | false (always visible) | price block | none |
| Badges (sale/sold-out) | gold accent `#DBBF7C`, no dedicated radius | `badge_corner_radius` 100, `badge_position` top-right, `badge_sale_*` | setting (set badge colour → gold, square if matching Prestige) |
| Quick add / quick view | none in Prestige | `quick_add` setting (Horizon feature) | setting (disable to match, or keep as upgrade) |
| PDP image size | large; zoom on; thumbnails on | product-information section + `_product-media-gallery` | setting |
| PDP show vendor | false | product-information settings | setting |
| PDP custom size form | **true** (made-to-measure) | `custom-size` block exists | section (ensure `custom-size` block added to product.json) |
| PDP dynamic checkout btn | false | `accelerated-checkout` block | setting (hide to match) |
| Cart type | **drawer** | `cart_type` = **drawer** | none |

---

## b) Per-template section mapping (Live → closest AB 2026)

AB 2026 available sections: `hero, slideshow, layered-slideshow, image-carousel, carousel, marquee, media-with-content, collection-list, collection-links, featured-product, featured-product-information, featured-blog-posts, product-list, product-hotspots, product-information, product-recommendations, main-collection, main-collection-list, main-blog, main-blog-post, main-page, main-cart, main-404, cart-drawer-section, header, header-announcements, footer, footer-utilities, logo, hero, custom-liquid, divider, section, _blocks, password, predictive-search, search-header, search-results, quick-order-list, section-rendering-product-card`.

### Homepage (`index`)
Live `content_for_index` order → AB `index.json` (current):

| # | Live section | Closest AB 2026 section | Status |
|---|---|---|---|
| 1 | slideshow (2 banners) | `hero` (`hero_opening`) / `slideshow` / `layered-slideshow` | **built** (hero) |
| 2–3 | celebrity-carousel ×2 (marquee of celeb photos) | `image-carousel` (`seen_row_one`, `seen_row_two`) + `section` heading | **built** |
| 4 | featured-collections (MEN/WOMEN carousel) | `collection-list` (`wardrobes`) | **built** |
| 5–6 | featured-collections (disabled) | — | n/a (live disabled) |
| 7 | shop-the-look (hotspots, "Worn by Celebrities") | `product-hotspots` | **missing from index** (section exists — add it) |
| 8–10 | text-with-image ×3 ("It begins on paper" story) | `media-with-content` (`chapter_one/two/three`) | **built** |
| 11 | image-with-text-block ("DEFENCE COLONY", Book a fitting CTA, parallax) | `media-with-content` or `hero` with overlay | **missing** (add a media-with-content CTA block) |
| 12 | testimonials (4, autoplay) | `carousel` + text blocks (no dedicated testimonials section) | **missing** — rebuild with `carousel`/`_card` blocks |
| 13 | ab-location (map, address, dark map) | — no map section | **missing** — needs new map section/block (or `custom-liquid` + Maps embed) |
| 14 | newsletter | `section` (`signup`) with `email-signup` block | **built** |
| — | — | `featured-blog-posts` (`journal`) | AB extra (no live equiv on home) |

### Product (`product`)
| Live | AB 2026 | Status |
|---|---|---|
| product-template | `product-information` (`main`) | built |
| related-products | `product-recommendations` | built |
| recently-viewed-products | — (no recently-viewed section) | **missing** (optional; could use `product-recommendations`) |
| made-to-measure form | `custom-size` block | available — ensure added |

### Collection (`collection`)
| Live | AB 2026 | Status |
|---|---|---|
| collection-template | `main-collection` (`main`) + `section` banner | built |
| recently-viewed-products | — | missing (optional) |
| collection-footer | `footer-utilities` / `section` | partial |

### Cart (`cart`)
| Live | AB 2026 | Status |
|---|---|---|
| cart-template (drawer) | `main-cart` + `cart-drawer-section` + `product-list` upsell | built |
| recently-viewed-products | `product-list` (`product_list_NNFgcy`) | built (as upsell) |

### Blog / Article
| Live | AB 2026 | Status |
|---|---|---|
| blog-template | `main-blog` | built |
| article-template | `main-blog-post` | built |
| shop-now (after blog/article) | `featured-collection`/`collection-list` block | missing (optional) |

### Page templates
| Live template | Live sections | AB 2026 | Status |
|---|---|---|---|
| page.book-fitting.json | `book-fitting` | `book-fitting` (`main`) | **built** (dedicated section exists) |
| page.celebrity-closet.json | main-page + celebrity-carousel ×2 + shop-the-look | `main-page` (`main`) only | **partial** — AB page renders main-page but not the carousels/hotspots yet |
| page.measure-guide.json | measure-guide-intro + measure-guide ×2 (49 measurement blocks) | — no measure-guide section | **missing** — no equivalent; rebuild with `accordion`/`section`+`_content` or custom section |
| page.my-board.json | my-board (wishlist/board runtime) | — | **missing** — custom app-like feature (see §c) |
| page.contact.liquid | page-contact-template + ab-location | `main-page` + `contact-form` block | partial (contact form yes; map no) |
| page.faq.liquid | page-faq-template (14 Q&A blocks) | `main-page` + `accordion` blocks | buildable with `accordion` |
| list-collections / page.collections | list-collections-template | `main-collection-list`, `collection-list` | built |
| AB-only pages | — | page.company-profile, page.men, page.women (image-carousel storytelling) | AB extras |

### Header / Footer groups
| Live | AB 2026 (`header-group.json` / `footer-group.json`) | Status |
|---|---|---|
| announcement (disabled) | `header-announcements` (active) | built (toggle to match) |
| header (center logo, not sticky, not transparent) | `header` (center logo, sticky=always, transparent-home=true) | built — **settings differ** (sticky/transparent/announcement) |
| footer (Text + 2 menus + Newsletter + payment icons) | `footer` group: FAB widget + divider + 3 menus + divider + disabled migrated content + `footer-utilities` (copyright/policies/social) | built — **structure differs**; add payment-icons, align columns |

---

## c) Custom CSS / JS / app blocks in the live theme to recreate or reinstall

**No Shopify app embeds and no `custom_liquid` blocks exist in the live `settings_data.json`** (`current.blocks` is empty; no `@app`, no `shopify://apps`). The live store's custom behaviour is baked into Prestige's own Liquid/JS, not app blocks. Items rendered globally from `layout/theme.liquid` that have **no AB 2026 equivalent** and must be rebuilt if wanted:

| Live custom feature | What it is | AB 2026 status / how to recreate |
|---|---|---|
| `whatsapp-concierge` (snippet) | Floating WhatsApp contact button | **Partially present** — AB footer has `ai_gen_block_c0407fb` floating-action widget with WhatsApp (+91 8287222999). Verify number/links. |
| `ab-board-runtime` (snippet, on product/collection/my-board) + `page.my-board` | Custom "My Board" wishlist/lookbook with function labels (Mehendi, Haldi, Sangeet, Wedding, Reception, Other) and localStorage/runtime JS | **Missing** — custom JS feature. Rebuild as a custom block/section + JS asset, or reinstall the wishlist app. |
| `card-hover-video` (snippet) | Plays product's first video muted on card hover | **Missing as-is** — AB has `card_hover_effect` (lift/scale/zoom) but not video-on-hover. Add via card gallery / custom JS. |
| `banana-stand-footer` (snippet) | Custom footer add-on | Verify intent; likely folds into AB `footer-utilities`. |
| Search modal (custom) | Custom search overlay | AB uses `predictive-search` / `search-header` — built. |
| `campaign-popup` (active: "WELCOME10", 10% off, delay 5–8s, new-visitor, WhatsApp opt-in) | Prestige's campaign popup | **Missing** — AB has `popup-link` block but no timed campaign popup. Rebuild as custom section or reinstall a popup app. |
| Currency conversion (USD/CAD/EUR/GBP) | Prestige multi-currency | Use Shopify Markets + AB country/language selectors (present in header). |
| `measure-guide` + `measure-guide-intro` | Size-chart pages (49 measurement rows) | **Missing** — rebuild with `accordion`/`section` blocks or a custom section. |
| `shop-the-look` hotspots | Product hotspots over lifestyle images | Map to AB `product-hotspots` section (exists). |
| `testimonials`, `ab-location` (map) | Reviews carousel; Google-map + address | **Missing** — rebuild testimonials via `carousel`+`_card`; map needs a new section or `custom-liquid` embed. |

**CSS note:** Prestige compiles tokens via SCSS (`theme.scss.liquid`, no `:root` vars) with computed `color_mix` borders and a button hover-wipe animation. AB 2026 uses CSS custom properties consumed by `base.css`. Any pixel-exact Prestige effect (0.2em button tracking, gold hover-wipe, taupe border lines, 100px grid gaps) must be re-expressed as AB CSS variables / rules — **do not port SCSS**.

---

## d) Proposed ordered change plan (smallest-risk first)

**Phase 0 — Confirmed already matching (no action):** palette (scheme-1 coffee), fonts (Playfair/Lora→Mulish), logo, square buttons & cards (radius 0), transparent/outline secondary buttons, drawer cart, center logo. Verify visually only.

**Phase 1 — Pure setting changes (lowest risk, reversible in theme editor):**
1. `inputs_border_radius` 4 → **0** (match square inputs).
2. `button_text_case_primary/secondary` default → **uppercase**.
3. `type_size_paragraph` 16 → **15/14** (optional, match base 15px).
4. `page_width` normal → **narrow** (90rem ≈ Prestige 1420px container).
5. Header: decide sticky (`always`→off), transparent-home (on→off), announcement bar (on→off) to **match live exactly** — or consciously keep as upgrades. Document the decision.
6. Align header/footer colours: header bg `#281111`→`#241A14`, footer bg `#281111`→`#16100D`, footer text `#e7ce93/#c9a250`→`#B5A493` (if exact match wanted).
7. Product card: set natural ratio, center info, enable second-image hover; badge colour → gold, position/ shape to taste.
8. Add `payment-icons` block to footer; confirm WhatsApp number in footer FAB widget.

**Phase 2 — CSS custom-property / rule tweaks (medium risk, theme CSS):**
9. Add `letter-spacing: ~0.2em` to `.button` rules in `assets/base.css` (no per-button tracking var exists).
10. Tune `--button-padding-block/inline` toward 14px/28px if exact.
11. Confirm muted text (`--color-foreground-subdued/muted`) renders as `#B5A493`; adjust if off.
12. Reconcile global `page_text_color` `#C9A250` (gold) vs scheme foreground `#FFF6E4` (cream) — decide default body text colour.
13. Border colour: AB scheme border `#4A382C` is darker than Prestige's light tan line — lighten if matching.
14. Widen product-grid gaps toward Prestige's generous 100px H / 40–50px V (per product-list/collection section settings or gap vars).
15. (Optional) Recreate the gold button hover-wipe animation.

**Phase 3 — Section wiring on existing sections (medium risk, JSON templates):**
16. Add `product-hotspots` ("Worn by Celebrities") to `index.json`.
17. Add a media-with-content CTA block for the "DEFENCE COLONY / Book a fitting" banner on home.
18. Ensure product template has `custom-size` block (made-to-measure) and hides dynamic-checkout if matching live.
19. Flesh out `page.celebrity-closet.json` with the celebrity `image-carousel`s + hotspots (currently only `main-page`).
20. Build FAQ page with `accordion` blocks.

**Phase 4 — Rebuild missing sections/features (highest risk, new code/apps):**
21. **Testimonials** — rebuild via `carousel` + `_card`/`text` blocks.
22. **Map / atelier location** (`ab-location`) — new section or `custom-liquid` + Google Maps embed; address + dark-map style.
23. **Measure guide** pages (49 measurement rows) — custom section or `accordion`/`section` composition.
24. **My Board** wishlist/lookbook — custom block/section + JS, or reinstall a wishlist app (its runtime JS has no Horizon equivalent).
25. **Campaign popup** (WELCOME10, timed, new-visitor, WhatsApp opt-in) — custom section or popup app.
26. **Card-hover video** — custom JS/card-gallery enhancement if the video-on-hover effect is required.

Do each phase, preview, and compare against `../live-achin-bindlish` before advancing. No Prestige code is copied — only the values above are reproduced in AB 2026's own settings/CSS/sections.
