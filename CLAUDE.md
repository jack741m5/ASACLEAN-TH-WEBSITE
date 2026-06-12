# CLAUDE.md

Guidance for AI assistants (and humans) working in this repository.

## What this is

A **single-page marketing website** for **ASACLEAN™** purging compound
(เม็ดล้างเครื่องจักรพลาสติก / plastic-machine purging pellets), built for
**บริษัท จิตตรงเคมีภัณฑ์ จำกัด (CSC)** — the authorized ASACLEAN distributor in
Thailand since พ.ศ. 2538 (1995). The site is in **Thai** (`<html lang="th">`)
and targets plastics manufacturers (injection molding / extrusion) who want to
cut color-change downtime, scrap, and carbon-buildup problems.

The production site is deployed at **https://asaclean-thailand.com**.

## Repository layout

```
asaclean-website.html      The entire website — HTML + CSS + JS in one file (~1244 lines)
Asaclean_Thailand_Logo.png Brand logo (also embedded as base64 inside the HTML)
rheology_asacleanR.mp4     Background video for the dark "R-series" section
screw_01.jpg.jpg           Before/after screw photos (note the doubled .jpg extension)
screw-02.jpg.jpg
.gitattributes             LF normalization (* text=auto)
```

There is **no build system, no package manager, no framework, and no tests.**
The `.html` file is self-contained and runs by opening it in a browser.

`PROJECT-STATUS.md` (Thai) is the owner's running **status/handoff log** — deploy
inventory, SEO status, TODOs, and a changelog kept in sync across machines. Read
it before starting work, and append to its changelog when you change site
content. It is the source of truth for *what is live*; this CLAUDE.md is the
source of truth for *how the code works*.

> A sibling repo, `website-asacleanth-`, currently holds only a placeholder
> README and is not the active codebase.

> **This repo vs. the deploy project:** This Git repo holds the landing-page
> **source** only. The owner maintains a separate local project,
> `asaclean-thailand_v02/`, whose `deploy-hostinger/` folder is the actual
> artifact uploaded to Hostinger `public_html` (see
> [Hosting & deployment](#hosting--deployment)). That project also contains an
> unused React template (`client/` `server/` `shared/` `dist/`) left over from a
> previous scaffold — it is **not** used to build the live site. Those folders
> are **not** in this repo.

## Anatomy of `asaclean-website.html`

The file is one self-contained document with three regions:

| Lines (approx.) | Region | Contents |
|-----------------|--------|----------|
| 1–442  | `<head>` + `<style>` | All CSS. Design tokens live in `:root`. |
| 444–983 | `<body>` markup | `<nav>` + all `<section>`s. Many sections are empty shells filled by JS. |
| 984–1242 | `<script>` | Vanilla JS: data arrays + render functions + the ROI calculator + the quiz. |

### Sections (in DOM order)

`hero` → `features` → `rseries` (dark, video background) → `rseries-grades` →
`painpoint` → `calc` (ROI calculator) → `myths` → `quiz` (grade recommender) →
`grades` (grade catalog) → `faq` → `contact`.

Each `<section>` has an `id` used by the sticky nav for smooth-scroll anchors.

### How content is rendered

Most content is **data-driven**: JS holds an array of objects and injects HTML
into a container `<div>` by `id`. To change copy or add items, edit the **data
array**, not hand-written markup. Key pairings:

| Data array (in `<script>`) | Renders into element id | Section |
|----------------------------|-------------------------|---------|
| `FEATURES`                 | `#featGrid`             | features |
| `STEPS`                    | `#stepGrid`             | rseries (how it works) |
| `PROBS`                    | `#probGrid`             | painpoint |
| `GRADES` + `GTABS`         | `#gradeGrid` / `#gradeTabs` | grades catalog |
| `FAQS` + `FCATS`           | `#faqWrap` / `#faqTabs` | faq accordion |
| `QSTEPS` + `GRADES_Q`      | `#quizbox`              | quiz |
| `FIELDS` + `PRESETS`       | calculator inputs       | calc |

### Interactive pieces

- **ROI calculator (`calc`)** — `FIELDS` defines slider/number inputs grouped as
  `prod` / `before` / `after`; `PRESETS` (small/medium/large/sample) seed values.
  `compute()` does the math (labor + material + scrap cost, before vs after),
  updates KPI cards, comparison bars, a donut breakdown, and a multi-year savings
  line chart drawn as inline SVG in `drawLine()`. `scale`, `years`, and
  `stopMode` toggles change the view. Numbers formatted with `Intl.NumberFormat('th-TH')`.
- **Grade recommender quiz (`quiz`)** — 4 questions in `QSTEPS`; `recommend()`
  maps answers (purpose / temperature / hot-runner / resin) to grade names, shown
  from the `GRADES_Q` lookup. State held in `qStep` / `qAnswers` / `qSel`.
- **Grade catalog (`grades`)** — `GRADES` entries carry `cat`
  (`power` / `rinse` / `heat`), temperature range, `hr` (Hot Runner safe), and
  `gf` (contains glass fiber). **Domain rule: glass-fiber (GF) grades must NOT be
  used with Hot Runners** — this constraint is reflected throughout the copy and
  the quiz logic; keep it consistent if you edit grade data.
- **Lead form** — both the "request a sample" modal (`#sampleModal`) and the
  contact form POST to a **Google Apps Script** endpoint (`GOOGLE_SCRIPT_URL`)
  via `sendLead()` using `fetch(..., {mode:'no-cors'})`. Success always shows the
  `#toast`. If you change the backend, update `GOOGLE_SCRIPT_URL`.
- **Scroll behavior** — `IntersectionObserver` adds `.in` to `.reveal` elements
  and triggers `animateCounters()` for `[data-count]` stats; nav gains `.scrolled`
  past 20px.

## Conventions

- **Language:** Thai-first UI copy. Keep technical/grade names (newEX, RX, PX2,
  GL3, PF, etc.) and acronyms (CSC, GF, Hot Runner) as-is.
- **Styling:** Plain CSS, no preprocessor. Use the design tokens in `:root`
  rather than hard-coding colors. Brand palette:
  `--red:#E60012` (ASACLEAN red), `--navy:#1a2436`, `--navy2:#3A4F7A`,
  `--teal:#2FA4A9`, plus orange CTA gradient tokens.
- **Fonts:** `Prompt` (headings) + `Sarabun` (body) from Google Fonts;
  **Font Awesome 6.5.1** icons via CDN. These are external CDN dependencies — the
  page needs network access to render fonts/icons correctly.
- **JS style:** ES5-flavored vanilla JS (`var`, `function`, no modules/build).
  Match the existing terse, single-purpose-function style. No external JS libs.
- **Images:** In the repo HTML, the logo and photos are inlined as **base64 data
  URIs**, which is why the file is large. See the deployment note below — the
  production build swaps these for external optimized files.

### ⚠️ Working with the large file

`asaclean-website.html` contains long base64 blobs. **Do not read or print the
whole file**, and never paste base64 into edits. Instead:

- Use `grep`/search to locate a section, data array, or function by name.
- Make **targeted edits** to the specific data array or markup you need.
- When grepping, filter out noise with `grep -v base64` if needed.

## Contact / business facts (used across the page)

- **Company:** บริษัท จิตตรงเคมีภัณฑ์ จำกัด (CSC) — authorized ASACLEAN
  distributor in Thailand, 30+ years (since 1995).
- **Phone:** `tel:0632149268`, `tel:0835907389` (the `083-590-7389` number,
  `+66-83-590-7389`, is the primary one used in the Schema.org business data).
- **LINE:** `@asacsc` (`https://lin.ee/uYh18ag`)
- **Email:** `info@asaclean-thailand.com`
- **Facebook:** AsacleanThailand · **YouTube:** `@asacsc` · **Group site:** cscth.com
- **Hours (per Schema):** Mon–Fri 08:30–17:30, Bangkok.
- Keep these consistent everywhere they appear if you update contact info.

## Production site architecture

This repo holds the **landing-page source**, but the live site at
**asaclean-thailand.com** is a **multi-page SEO content site**. Keep the bigger
picture in mind when editing:

```
/                                   Landing page  ← built from asaclean-website.html
/.htaccess                          Cache + Gzip + Force HTTPS
/robots.txt                         → points at /sitemap.xml
/sitemap.xml                        8 URLs + image sitemap
/articles/                          Articles hub (index.html) listing all posts
/articles/style.css                 Shared external stylesheet for all article pages
/articles/extrusion-purging-guide.html
/articles/mechanical-purging-explained.html
/articles/routine-purging-sealing.html
/articles/carbon-black-specks-purging.html
/articles/oee-roi-injection-molding.html
/articles/purge-vs-rinse-asaclean.html
```

- The **landing page** (`/`) and the **articles** are styled differently: the
  landing page keeps **all CSS inline**, while every article page links a shared
  **external `style.css`** (`/articles/style.css`). Both deliberately share the
  same brand tokens (`--red`, `--navy`, `--teal`, fonts `Prompt`/`Sarabun`), so
  if you change a brand color, change it in **both** places.
- **Articles** are long-form Thai SEO content (Extrusion guide, "what is
  Mechanical Purging", Routine Purging & Sealing, black specks, OEE/ROI, purge vs
  rinse). Each carries full SEO scaffolding: tuned `<title>`/description,
  `<link rel="canonical">`, Open Graph + Twitter meta, and **JSON-LD `Article`
  structured data** (author = "CSC Thailand", publisher = CSC). They share the
  landing page's nav/footer look and cross-link back to landing anchors
  (`/#calc`, `/#quiz`, `/#contact`, etc.).
- When adding or renaming a page/asset: update **`/sitemap.xml`**, the article
  hub list, canonical/OG URLs, the JSON-LD, and any cross-links.

### Landing page: source vs. production

The repo `asaclean-website.html` is the **authoring/source** version. The
deployed landing `index.html` differs in two ways:

1. **SEO/social head tags** are added in production: tuned `<title>`/description,
   canonical, Open Graph + Twitter Card meta, JSON-LD structured data,
   `theme-color`, and `robots` meta.
2. **External assets replace base64**: favicons (`favicon.ico`, `favicon-32.png`,
   `favicon-48.png`, `apple-touch-icon.png`), `og-image.jpg` (1200×630), and
   optimized images (`logo-nav.webp`, `screw-before.webp`/`screw-after.webp`,
   `video-poster.jpg` + `rheology_asacleanR.mp4`) are referenced as files instead
   of inlined data URIs.

The deployed landing page's JSON-LD covers `Organization`, `LocalBusiness`,
`WebSite`, `Product`, and `FAQPage`; article pages add `Article`.

## Hosting & deployment

The live site is hosted on **Hostinger**; deployment is a **manual file upload**
into `public_html` (no CI/CD, no pipeline). The owner builds the deploy artifact
in a **separate** local project — `asaclean-thailand_v02/deploy-hostinger/` — and
uploads the changed files. That `deploy-hostinger/` tree (landing `index.html`,
`articles/`, `sitemap.xml`, `robots.txt`, `.htaccess`, and the image/video/favicon
assets) is what maps 1:1 onto the production paths above. **It is not part of this
Git repo** — this repo is the landing-page source only.

Release checklist (mirrors `PROJECT-STATUS.md`):

1. Edit the landing page (`index.html`) or an article under `articles/`.
2. Bump `lastmod` in `sitemap.xml` whenever content changes.
3. Upload changed files to Hostinger `public_html`. **Binary files** (images,
   video, `.ico`) often need a manual re-upload — when overwriting an image from
   OneDrive you must download (Read) it first; `.ico` usually has to be uploaded
   by hand.
4. Re-submit the sitemap in Google Search Console after a sitemap change.
5. Record the change in the `PROJECT-STATUS.md` changelog.

## Development workflow

- **Edit:** change the relevant data array, markup, CSS token, or JS function in
  `asaclean-website.html`.
- **Preview:** open the file directly in a browser. (Fonts/icons/video need
  network access; the lead form needs the Apps Script endpoint to actually
  record submissions.)
- **No build, lint, or test step exists.** Verify changes visually.
- **Git:** development happens on branch `claude/claude-md-docs-yyij6h`. Commit
  with clear messages and push to that branch; open a PR for review.
