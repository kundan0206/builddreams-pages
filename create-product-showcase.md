# Create Product Showcase Page — Guide & LLM Prompt

## Purpose

This document contains a complete checklist, step-by-step instructions, and an LLM prompt template to create a new product showcase page in this repo using the `products/subdomain-product-page.html` template (the same approach used for `flexi-rag`). It is written so an LLM or a developer can repeat the process reliably without changing the site's UI/UX.

## Where to start (template)

- Template path: `products/subdomain-product-page.html`
- Target location pattern: `/<product-slug>/` (folder) containing `<product-slug>.html` + `assets/`.

## Required dependencies (present in the template)

- Tailwind CSS (via CDN)
- Lucide icons: `https://unpkg.com/lucide@latest/dist/umd/lucide.min.js`
- GSAP + ScrollTrigger
- Lenis (smooth scroll)
- Google fonts used in templates (Inter, Instrument Serif, JetBrains Mono)

## Assets & naming convention

Create a folder for the product and add an `assets/` subfolder. Recommended structure:

product-slug/

- product-slug.html
- assets/
  - hero.png (hero/lead image)
  - 1.png (screenshot 1)
  - 2.png (screenshot 2)
  - 3.png (screenshot 3)
  - 4.png (optional)
  - 5.png (optional)

Images: use sensible sizes (hero ~1200–1600px wide, screenshots ~1000px wide). Use descriptive `alt` text.

## Step-by-step (manual)

1. Choose a product slug (lowercase, hyphenated, e.g. `flexi-rag`).
2. Make folder & copy template:

```bash
mkdir -p product-slug/assets
cp products/subdomain-product-page.html product-slug/product-slug.html
```

1. Edit `product-slug/product-slug.html` — required replacements (only change content, not layout or global scripts):

- Head meta
  - `<title>` → `Product Name — Product Showcase | BuildDreams`
  - `<meta name="description">` → concise product description

- NAV label
  - Replace the inline small label text inside the nav link (example):
    - From: `/ SYNCHR SHOWCASE`
    - To: `/ FLEXIRAG SHOWCASE`

- HERO section
  - `chip` text (small label): e.g. `PRODUCT · FLEXIRAG`
  - `h1` headline: product name and one-liner
  - description paragraph: short summary + `product_url` link if available
  - Hero image `src` → `assets/hero.png` and set `alt`
  - CTA (`Try`/`Open dashboard`): update `href` to product dashboard or external link

- FEATURES / OVERVIEW
  - Replace the title, paragraph and bullets with the product's core capabilities.
  - Use `i data-lucide="<icon-name>"` for icons; keep `data-lucide` attributes intact so lucide renders SVGs.

- SCREENSHOTS
  - Replace the screenshot `<img>` `src` attributes with `assets/1.png`, `assets/2.png`, `assets/3.png` (or more).

- OFFERINGS / CAPABILITIES
  - Update plan names and copy (if applicable).

- CTA / DEMO
  - Update call-to-action URL(s) and label(s).

- CONTACT
  - Leave the form markup (id `contactForm`) intact; update mailto in JS or backend integration as needed.

- FOOTER
  - Update footer label text (e.g., `BuildDreams · FlexiRAG`).
  - If the footer is in a white theme (light background), ensure social icons remain dark — add `.footer-social` class to container and/or `.text-ink` utility.

1. Save and preview locally.

## Preview (local)

From project root run:

```bash
cd "builddreams-pages"
python -m http.server 8000
# then open http://localhost:8000/<product-slug>/<product-slug>.html
```

## JavaScript & icons notes

- Ensure `lucide.createIcons();` is present after the lucide script so icon placeholders (`<i data-lucide="...">`) get replaced by SVGs.
- If icons are not visible (CDN blocked or network issues), add a small fallback: either include explicit SVG `<img>` fallbacks or inject inline SVGs at page load (the repo's `product.html` shows a small `ensureFooterIcons()` fallback snippet).

## Checklist before PR

- [ ] `assets/hero.png` exists and loads
- [ ] Screenshots `assets/1.png`.. exist and load
- [ ] `title` and `meta description` are updated
- [ ] `h1` and hero paragraph reflect product messaging
- [ ] Lucide icons render (or fallback in place)
- [ ] No changes to global CSS or main JS bundles
- [ ] Footer and nav reflect product name
- [ ] Contact form still works (mailTo or hooked backend)
- [ ] Page previewed locally and visually validated (mobile + desktop)

## Commit & PR guidance

- Branch name: `feat/product-<slug>`
- Commit message: `feat: add product showcase <Product Name>`
- Files to include in PR:
  - `/<product-slug>/<product-slug>.html`
  - `/<product-slug>/assets/*`
- PR description: short purpose + list of changed files + preview link instruction (local server command above)

## LLM prompt template (use this to automate page creation)

Use the following prompt when instructing an LLM to create the page. Replace placeholders in ALL CAPS.

Prompt:

```
You are editing this repository: builddreams-pages.

Task: Create a new product showcase page using the template at `products/subdomain-product-page.html`.
Constraints (do not change these):
- Keep the UI/UX, layout, inline CSS and core JS intact. Only replace content: metadata, hero content, feature text, screenshots, offerings, CTAs, and footer labels.
- Do not remove or re-order any existing `<script>` or `<style>` blocks.
- Place assets in a new folder named PRODUCT_SLUG/assets/ with filenames hero.png, 1.png, 2.png, 3.png, ...

Inputs (replace these values):
- PRODUCT_SLUG: `flexi-rag` (lowercase, hyphens allowed)
- PRODUCT_NAME: `FlexiRAG`
- PRODUCT_URL: `https://flexirag.vercel.app/dashboard`
- SHORT_DESCRIPTION: `FlexiRAG is an AI-powered document intelligence and “document-to-API chatbot” platform that lets you upload files, ask questions, and deploy your documents as APIs or chatbots in minutes.`
- HERO_IMAGE: `assets/hero.png`
- SCREENSHOTS: [`assets/1.png`, `assets/2.png`, `assets/3.png`]
- FEATURES: a list of short bullets, each with optional icon name (lucide)
  e.g. [{icon: 'upload-cloud', title: 'Document upload & management', text: 'Upload PDFs, Word, Excel, PowerPoint and manage them in a secure workspace.'}, ...]
- CTA_PRIMARY: label and href (e.g. `Open dashboard`, PRODUCT_URL)

Output requirements:
1. Add a new file at `/<PRODUCT_SLUG>/<PRODUCT_SLUG>.html` copied from `products/subdomain-product-page.html`.
2. Replace the following spots with user-provided values: head title, meta description, nav small label text, chip label, hero h1 and paragraph, hero image src/alt, features grid, screenshots grid, offerings (if any), CTA links, and footer product label.
3. Ensure `lucide.createIcons()` remains in the page. Do not remove it.
4. If lucide icons are used, keep `data-lucide` attributes on `<i>` elements unchanged.
5. Output the complete new file content only (or produce an apply_patch diff if requested) and list the `assets/` filenames to be added.

Example (filled) inputs for FlexiRAG already supplied above — produce the final HTML file content and list the asset filenames to add to the repo.
```

## Example LLM output expectations

- A new HTML file content (ready to save at `product-slug/product-slug.html`).
- A list of assets the operator must upload: `hero.png`, `1.png`, `2.png`, `3.png`, ...

## Troubleshooting

- If icons don't render: confirm lucide CDN is reachable from your network; add the small JS fallback used in `product.html` if necessary.
- If fonts look off: confirm Google Fonts are loading (or use local fallbacks).

## Suggested next improvements (optional)

- Extract repeated CSS and JS into `assets/css/site.css` and `assets/js/site.js` to avoid copying large inline blocks across pages.
- Add automated script (Node/Python) to scaffold product folders, copy template, and inject content from a JSON/YAML file.

## Contact

If you want, I can:

---

- If icons don't render: confirm lucide CDN is reachable from your network; add the small JS fallback used in `product.html` if necessary.
- If fonts look off: confirm Google Fonts are loading (or use local fallbacks).

## Suggested next improvements (optional)

- Extract repeated CSS and JS into `assets/css/site.css` and `assets/js/site.js` to avoid copying large inline blocks across pages.
- Add automated script (Node/Python) to scaffold product folders, copy template, and inject content from a JSON/YAML file.

## Contact

If you want, I can:

- run a preview locally and verify visuals,
- generate the scaffold script to automate page creation,
- or run an LLM to auto-generate a product page from a list of input JSON objects.
