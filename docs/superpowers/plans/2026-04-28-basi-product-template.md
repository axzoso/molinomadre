# Basi Product Template Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create `docs/basi-prodotto-template.html` — a reusable product detail page for single Basi items, styled like an e-commerce product page within the Molino Madre design system.

**Architecture:** Single static HTML file. All CSS is inline `<style>`. JS is inline `<script>`. No build tools. Design tokens, nav, footer, sticky contact and utility classes are copied verbatim from `docs/basi.html`. New CSS is appended after the copied base. Thumbnail gallery swap is handled by a small vanilla JS function.

**Tech Stack:** HTML5, CSS3 (custom properties, CSS Grid, Flexbox), vanilla JS (IntersectionObserver, click handlers). Google Fonts (already loaded). No dependencies.

---

## Files

| Action | Path | Responsibility |
|--------|------|----------------|
| Create | `docs/basi-prodotto-template.html` | Complete product page template (example: Pizza Tonda) |

No existing files are modified.

---

### Task 1: Scaffold — head, CSS base, nav, breadcrumb

**Files:**
- Create: `docs/basi-prodotto-template.html`

Copy the foundational shell from `docs/basi.html`: `<head>` with Google Fonts, all CSS variables and utility classes, nav, mobile menu, breadcrumb. The page uses Pizza Tonda as the demo product.

- [ ] **Step 1: Create the file with head + CSS base**

Create `docs/basi-prodotto-template.html` with the following content (everything up to and including the CSS for nav, breadcrumb, buttons, footer and sticky — copied from basi.html):

```html
<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pizza Tonda – Basi per Pizza – Molino Madre Dal 1939</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300;1,400&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
:root {
  --rosso:      #8a2432;
  --terracotta: #a9483d;
  --grano:      #ddc9a2;
  --lavanda:    #e3e0e5;
  --oliva:      #888f64;
  --bianco:     #f5f2ec;
  --nero:       #1a1410;
  --font-d:     'Bebas Neue', sans-serif;
  --font-s:     'Cormorant Garamond', serif;
  --font-b:     'DM Sans', sans-serif;
}
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body { background: var(--bianco); color: var(--nero); font-family: var(--font-b); overflow-x: hidden; }
.container { max-width: 1280px; margin: 0 auto; padding: 0 40px; }

.label { font-family: var(--font-b); font-size: 11px; letter-spacing: .25em; text-transform: uppercase; color: var(--terracotta); display: block; margin-bottom: 16px; }
.divider { display: inline-block; width: 48px; height: 1px; background: var(--terracotta); margin-bottom: 20px; }
.section-title { font-family: var(--font-d); font-size: clamp(44px,6vw,88px); line-height: .93; letter-spacing: .02em; color: var(--rosso); }
.btn { display: inline-flex; align-items: center; gap: 10px; font-family: var(--font-b); font-size: 12px; font-weight: 500; letter-spacing: .2em; text-transform: uppercase; text-decoration: none; color: var(--rosso); border-bottom: 1.5px solid var(--rosso); padding-bottom: 4px; transition: gap .3s; }
.btn:hover { gap: 18px; }
.btn-light { color: var(--grano); border-color: var(--grano); }

/* ── NAV ──────────────────────────────── */
nav {
  position: fixed; top: 0; left: 0; right: 0; z-index: 1000;
  padding: 0 40px; height: 72px;
  display: flex; align-items: center; justify-content: space-between;
  background: rgba(245,242,236,.96); backdrop-filter: blur(12px);
  border-bottom: 1px solid rgba(138,36,50,.1);
}
.nav-logo { font-family: var(--font-d); font-size: 22px; letter-spacing: .12em; color: var(--rosso); text-decoration: none; display: flex; flex-direction: column; line-height: 1.1; }
.nav-logo span { font-family: var(--font-s); font-size: 10px; font-weight: 400; letter-spacing: .3em; text-transform: uppercase; opacity: .7; color: var(--nero); }
.nav-links { display: flex; gap: 32px; list-style: none; }
.nav-links a { font-size: 11px; letter-spacing: .18em; text-transform: uppercase; text-decoration: none; color: var(--nero); transition: color .3s; font-weight: 400; }
.nav-links a:hover, .nav-links a.active { color: var(--terracotta); }
.nav-cta { padding: 9px 22px; border: 1.5px solid var(--rosso); border-radius: 2px; font-size: 10px; letter-spacing: .2em; text-transform: uppercase; text-decoration: none; color: var(--rosso); transition: all .3s; font-weight: 500; }
.nav-cta:hover { background: var(--rosso); color: #fff; }

/* ── BREADCRUMB ──────────────────────── */
.breadcrumb { padding: 100px 0 0; background: var(--bianco); }
.breadcrumb-inner { display: flex; align-items: center; gap: 10px; font-size: 10px; letter-spacing: .2em; text-transform: uppercase; color: rgba(26,20,16,.35); }
.breadcrumb-inner a { color: inherit; text-decoration: none; transition: color .3s; }
.breadcrumb-inner a:hover { color: var(--terracotta); }
.breadcrumb-sep { color: var(--terracotta); }

/* ── CTA SECTION ─────────────────────── */
.cta-section { background: var(--rosso); padding: 90px 0; text-align: center; position: relative; overflow: hidden; }
.cta-pattern { position: absolute; inset: 0; background: repeating-linear-gradient(90deg,transparent,transparent 80px,rgba(255,255,255,.025) 80px,rgba(255,255,255,.025) 81px), repeating-linear-gradient(0deg,transparent,transparent 80px,rgba(255,255,255,.025) 80px,rgba(255,255,255,.025) 81px); }
.cta-inner { position: relative; z-index: 2; }
.cta-label { font-size: 10px; letter-spacing: .3em; text-transform: uppercase; color: rgba(255,255,255,.45); margin-bottom: 20px; }
.cta-title { font-family: var(--font-d); font-size: clamp(36px,5vw,64px); line-height: .95; color: #fff; margin-bottom: 20px; }
.cta-text { font-family: var(--font-s); font-size: 17px; font-weight: 300; color: rgba(255,255,255,.6); line-height: 1.75; max-width: 520px; margin: 0 auto 40px; }
.btn-cta { display: inline-flex; align-items: center; gap: 12px; padding: 16px 36px; background: var(--grano); color: var(--rosso); font-family: var(--font-b); font-size: 11px; letter-spacing: .22em; text-transform: uppercase; font-weight: 600; text-decoration: none; border-radius: 2px; transition: background .3s; }
.btn-cta:hover { background: #fff; }

/* ── FOOTER ──────────────────────────── */
footer { background: var(--nero); padding: 80px 0 40px; }
.footer-top { display: grid; grid-template-columns: 2fr 1fr 1fr 1fr; gap: 48px; padding-bottom: 56px; border-bottom: 1px solid rgba(255,255,255,.05); margin-bottom: 36px; }
.footer-brand-name { font-family: var(--font-d); font-size: 30px; letter-spacing: .12em; color: #fff; line-height: 1; margin-bottom: 4px; }
.footer-brand-sub { font-family: var(--font-s); font-size: 11px; letter-spacing: .28em; color: var(--grano); opacity: .6; text-transform: uppercase; margin-bottom: 20px; }
.footer-tagline { font-family: var(--font-s); font-size: 14px; font-weight: 300; font-style: italic; color: rgba(255,255,255,.32); line-height: 1.6; max-width: 240px; }
.footer-col h5 { font-size: 9px; letter-spacing: .28em; text-transform: uppercase; color: var(--grano); opacity: .7; margin-bottom: 20px; }
.footer-col ul { list-style: none; display: flex; flex-direction: column; gap: 10px; }
.footer-col ul li a { font-size: 13px; color: rgba(255,255,255,.4); text-decoration: none; transition: color .3s; font-family: var(--font-s); }
.footer-col ul li a:hover { color: var(--grano); }
.footer-bottom { display: flex; align-items: center; justify-content: space-between; }
.footer-copy { font-size: 10px; color: rgba(255,255,255,.2); letter-spacing: .06em; }
.footer-socials { display: flex; gap: 12px; }
.footer-social-link { width: 34px; height: 34px; border: 1px solid rgba(255,255,255,.1); border-radius: 50%; display: flex; align-items: center; justify-content: center; text-decoration: none; color: rgba(255,255,255,.35); font-size: 11px; font-family: var(--font-b); font-weight: 500; transition: all .3s; }
.footer-social-link:hover { border-color: var(--grano); color: var(--grano); }

/* ── STICKY ──────────────────────────── */
.sticky-contact { position: fixed; bottom: 24px; right: 24px; z-index: 999; display: flex; flex-direction: column; gap: 8px; align-items: flex-end; }
.sticky-btn { display: flex; align-items: center; gap: 10px; padding: 11px 20px; border-radius: 40px; font-size: 10px; letter-spacing: .14em; text-transform: uppercase; font-weight: 500; text-decoration: none; box-shadow: 0 4px 20px rgba(0,0,0,.22); transition: transform .3s, box-shadow .3s; font-family: var(--font-b); }
.sticky-btn:hover { transform: translateY(-2px); box-shadow: 0 8px 28px rgba(0,0,0,.28); }
.sticky-main { background: var(--rosso); color: var(--grano); }
.sticky-wa { background: #25D366; color: #fff; }
.icon-svg { width: 14px; height: 14px; flex-shrink: 0; }

/* ── HAMBURGER / MOBILE MENU ─────────── */
.nav-hamburger { display: none; flex-direction: column; gap: 5px; cursor: pointer; padding: 8px; background: none; border: none; color: var(--nero); }
.nav-hamburger span { display: block; width: 22px; height: 2px; background: currentColor; transition: all .3s; }
.mobile-menu { display: none; position: fixed; inset: 0; background: rgba(26,20,16,.97); z-index: 1100; flex-direction: column; align-items: center; justify-content: center; gap: 32px; }
.mobile-menu.open { display: flex; }
.mobile-menu a { font-family: var(--font-d); font-size: clamp(32px,6vw,48px); letter-spacing: .06em; color: #fff; text-decoration: none; transition: color .3s; }
.mobile-menu a:hover { color: var(--terracotta); }
.mobile-menu-close { position: absolute; top: 24px; right: 24px; font-size: 28px; color: rgba(255,255,255,.5); cursor: pointer; background: none; border: none; font-family: var(--font-b); line-height: 1; }

/* ── REVEAL ──────────────────────────── */
.reveal { opacity: 0; transform: translateY(36px); transition: opacity .85s ease, transform .85s ease; }
.reveal.visible { opacity: 1; transform: translateY(0); }

</style>
</head>
<body>
```

- [ ] **Step 2: Add nav + mobile menu + breadcrumb HTML**

Append immediately after `<body>`:

```html
<!-- NAV -->
<nav id="mainNav">
  <a href="index.html" class="nav-logo">MOLINO MADRE<span>Dal 1939 · Calabria</span></a>
  <ul class="nav-links">
    <li><a href="il-molino.html">Il Mulino</a></li>
    <li><a href="farine.html">Farine</a></li>
    <li><a href="basi.html" class="active">Basi</a></li>
    <li><a href="panetti.html">Panetti</a></li>
    <li><a href="il-forno-del-mulino.html">Il Forno</a></li>
    <li><a href="consulenza.html">Consulenza</a></li>
  </ul>
  <a href="contatti.html" class="nav-cta">Contatti</a>
  <button class="nav-hamburger" id="burger" aria-label="Apri menu">
    <span></span><span></span><span></span>
  </button>
</nav>

<div class="mobile-menu" id="mobileMenu">
  <button class="mobile-menu-close" id="closeMenu">&#x2715;</button>
  <a href="il-molino.html">Il Mulino</a>
  <a href="farine.html">Farine</a>
  <a href="basi.html">Basi</a>
  <a href="panetti.html">Panetti</a>
  <a href="il-forno-del-mulino.html">Il Forno</a>
  <a href="consulenza.html">Consulenza</a>
  <a href="contatti.html">Contatti</a>
</div>

<!-- BREADCRUMB -->
<div class="breadcrumb">
  <div class="container">
    <div class="breadcrumb-inner">
      <a href="index.html">Home</a>
      <span class="breadcrumb-sep">&#8594;</span>
      <a href="basi.html">Basi per Pizza</a>
      <span class="breadcrumb-sep">&#8594;</span>
      <span>Pizza Tonda</span>
    </div>
  </div>
</div>
```

- [ ] **Step 3: Open in browser and verify nav renders correctly**

Open `docs/basi-prodotto-template.html` in browser. Expected: nav bar visible at top, breadcrumb below with correct links, no broken styles.

- [ ] **Step 4: Commit**

```bash
git add docs/basi-prodotto-template.html
git commit -m "feat: scaffold basi product template – nav, breadcrumb, base CSS"
```

---

### Task 2: Hero section — CSS + left column (info)

**Files:**
- Modify: `docs/basi-prodotto-template.html` — add `.product-hero` CSS and left-column HTML

- [ ] **Step 1: Add hero CSS inside `<style>`**

Append the following inside the `<style>` block, before `</style>`:

```css
/* ── PRODUCT HERO ────────────────────── */
.product-hero { padding: 56px 0 80px; background: var(--bianco); }
.product-hero-grid {
  display: grid;
  grid-template-columns: 5fr 7fr;
  gap: 72px;
  align-items: start;
}

/* Left column */
.product-info {}
.product-category { margin-bottom: 8px; }
.product-title {
  font-family: var(--font-d);
  font-size: clamp(52px, 7vw, 96px);
  line-height: .88;
  letter-spacing: .02em;
  color: var(--rosso);
  margin-bottom: 24px;
}
.product-subtitle {
  font-family: var(--font-s);
  font-size: 17px;
  font-style: italic;
  font-weight: 300;
  color: #666;
  line-height: 1.75;
  margin-bottom: 24px;
  max-width: 420px;
}
.product-tags { display: flex; flex-wrap: wrap; gap: 7px; margin-bottom: 28px; }
.product-tag {
  font-size: 9px;
  letter-spacing: .16em;
  text-transform: uppercase;
  border: 1px solid rgba(136,143,100,.5);
  color: var(--oliva);
  padding: 5px 12px;
  border-radius: 2px;
}
.product-sep { width: 100%; height: 1px; background: rgba(169,72,61,.14); margin-bottom: 28px; }
.product-specs { display: flex; flex-direction: column; gap: 14px; margin-bottom: 36px; }
.spec-row { display: flex; align-items: baseline; gap: 12px; }
.spec-key {
  font-family: var(--font-b);
  font-size: 10px;
  letter-spacing: .18em;
  text-transform: uppercase;
  color: rgba(26,20,16,.35);
  min-width: 100px;
  flex-shrink: 0;
}
.spec-val {
  font-family: var(--font-s);
  font-size: 15px;
  font-weight: 400;
  color: var(--nero);
  line-height: 1.4;
}
```

- [ ] **Step 2: Add hero HTML — left column**

After the breadcrumb closing `</div>`, add:

```html
<!-- PRODUCT HERO -->
<section class="product-hero">
  <div class="container">
    <div class="product-hero-grid">

      <!-- LEFT: Info -->
      <div class="product-info reveal">
        <span class="label product-category">Piccoli Formati &middot; Al piatto</span>
        <div class="divider"></div>
        <h1 class="product-title">PIZZA<br>TONDA</h1>
        <p class="product-subtitle">Base precotta al piatto, pasta alveolata e altamente digeribile. Ideale per locali con posti a sedere. Cuoce in soli 3&ndash;4 minuti.</p>
        <div class="product-tags">
          <span class="product-tag">48h lievitazione</span>
          <span class="product-tag">Lievito madre</span>
          <span class="product-tag">Forno a legna</span>
          <span class="product-tag">Stesa a mano</span>
          <span class="product-tag">Filiera controllata</span>
        </div>
        <div class="product-sep"></div>
        <div class="product-specs">
          <div class="spec-row">
            <span class="spec-key">Formato</span>
            <span class="spec-val">30 cm &middot; 33 cm &middot; 36 cm</span>
          </div>
          <div class="spec-row">
            <span class="spec-key">Peso</span>
            <span class="spec-val">250 g &middot; 300 g &middot; 350 g</span>
          </div>
          <div class="spec-row">
            <span class="spec-key">Cottura</span>
            <span class="spec-val">3&ndash;4 min a 280&deg;C (forno a legna o elettrico)</span>
          </div>
          <div class="spec-row">
            <span class="spec-key">Conservazione</span>
            <span class="spec-val">Surgelato &mdash; catena del freddo</span>
          </div>
          <div class="spec-row">
            <span class="spec-key">Scadenza</span>
            <span class="spec-val">12 mesi dalla produzione</span>
          </div>
          <div class="spec-row">
            <span class="spec-key">Confez.</span>
            <span class="spec-val">Cartone da 10 basi &middot; pallet disponibile</span>
          </div>
        </div>
        <a href="consulenza.html" class="btn">Richiedi Scheda Tecnica &#8594;</a>
      </div>
      <!-- RIGHT: Gallery — added in Task 3 -->

    </div>
  </div>
</section>
```

- [ ] **Step 3: Open in browser, verify left column**

Expected: large red title "PIZZA TONDA", italic subtitle, olive-colored tags, spec rows with grey label + dark value, CTA link at bottom. No layout shift.

- [ ] **Step 4: Commit**

```bash
git add docs/basi-prodotto-template.html
git commit -m "feat: product hero left column – title, specs, tags, CTA"
```

---

### Task 3: Hero section — right column (gallery)

**Files:**
- Modify: `docs/basi-prodotto-template.html` — add gallery CSS + HTML

- [ ] **Step 1: Add gallery CSS inside `<style>`**

Append after the last CSS block (before `</style>`):

```css
/* ── PRODUCT GALLERY ─────────────────── */
.product-gallery {}
.gallery-main {
  position: relative;
  width: 100%;
  aspect-ratio: 4 / 3;
  border-radius: 2px;
  overflow: hidden;
  margin-bottom: 10px;
  background: linear-gradient(145deg, #5a2812 0%, #3a1808 100%);
}
.gallery-main img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: opacity .3s;
}
.gallery-thumbs { display: flex; gap: 10px; }
.gallery-thumb {
  flex: 1;
  aspect-ratio: 1;
  border-radius: 2px;
  overflow: hidden;
  cursor: pointer;
  border: 2px solid transparent;
  transition: border-color .25s;
  background: linear-gradient(145deg, #6a2820 0%, #4a1810 100%);
}
.gallery-thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}
.gallery-thumb.active { border-color: var(--terracotta); }
.gallery-thumb:hover { border-color: rgba(169,72,61,.5); }
```

- [ ] **Step 2: Add gallery HTML — right column**

Replace the comment `<!-- RIGHT: Gallery — added in Task 3 -->` with:

```html
      <!-- RIGHT: Gallery -->
      <div class="product-gallery reveal">
        <div class="gallery-main">
          <img id="galleryMain"
               src="images/basi-pizza-tonda-main.jpg"
               alt="Pizza Tonda – vista principale"
               onerror="this.style.display='none'">
        </div>
        <div class="gallery-thumbs">
          <div class="gallery-thumb active" onclick="swapImage(this, 'images/basi-pizza-tonda-main.jpg', 'Pizza Tonda – vista principale')">
            <img src="images/basi-pizza-tonda-main.jpg" alt="Vista 1" onerror="this.style.display='none'">
          </div>
          <div class="gallery-thumb" onclick="swapImage(this, 'images/basi-pizza-tonda-2.jpg', 'Pizza Tonda – dettaglio bordi')">
            <img src="images/basi-pizza-tonda-2.jpg" alt="Vista 2" onerror="this.style.display='none'">
          </div>
          <div class="gallery-thumb" onclick="swapImage(this, 'images/basi-pizza-tonda-3.jpg', 'Pizza Tonda – confezione')">
            <img src="images/basi-pizza-tonda-3.jpg" alt="Vista 3" onerror="this.style.display='none'">
          </div>
        </div>
      </div>
```

Note: `onerror="this.style.display='none'"` ensures the colored gradient background shows gracefully when images are missing (they will be missing in the template — that's expected).

- [ ] **Step 3: Open in browser, verify gallery**

Expected: gradient colored block (4:3 ratio) on the right, 3 equal square thumbnails below. Left info column and right gallery are side-by-side on desktop.

- [ ] **Step 4: Commit**

```bash
git add docs/basi-prodotto-template.html
git commit -m "feat: product gallery – main image + 3 thumbnails"
```

---

### Task 4: Description section

**Files:**
- Modify: `docs/basi-prodotto-template.html` — add description CSS + HTML

- [ ] **Step 1: Add description CSS inside `<style>`**

Append after the gallery CSS:

```css
/* ── PRODUCT DESCRIPTION ─────────────── */
.product-description { padding: 80px 0; background: var(--bianco); }
.product-description-inner { max-width: 720px; }
.product-description-body {
  font-family: var(--font-s);
  font-size: 18px;
  font-weight: 300;
  line-height: 1.85;
  color: #555;
  margin-top: 4px;
}
.product-description-body p + p { margin-top: 20px; }
```

- [ ] **Step 2: Add description HTML**

After the closing `</section>` of `.product-hero`, add:

```html
<!-- PRODUCT DESCRIPTION -->
<section class="product-description">
  <div class="container">
    <div class="product-description-inner reveal">
      <span class="label">Il Prodotto</span>
      <div class="divider"></div>
      <h2 class="section-title">IL PRODOTTO</h2>
      <div class="product-description-body">
        <p>La Pizza Tonda &egrave; la base per pizza al piatto che nasce dall&rsquo;esperienza trentennale di Molino Madre nella macinazione delle farine. L&rsquo;impasto, realizzato con la nostra Farina Tipo 1 con germe di grano, viene lavorato con lievito madre e lasciato maturare per 48 ore a temperatura controllata.</p>
        <p>Ogni base viene stesa a mano, una a una, nel rispetto della tradizione artigianale. La precottura avviene nel forno a legna con base in pietra: il risultato &egrave; un prodotto con una pasta alveolata, bordi irregolari e autentici, e un profumo che ricorda le vere pizzerie italiane.</p>
        <p>La surgelazione immediata con catena del freddo preserva intatte tutte le qualit&agrave; organolettiche. In laboratorio: farcire e infornare. Nessun personale qualificato necessario, risultato garantito ogni volta.</p>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Open in browser, verify**

Expected: wide serif body text at comfortable reading size, max-width ~720px, label and Bebas title above. Distinct visual break from the hero above.

- [ ] **Step 4: Commit**

```bash
git add docs/basi-prodotto-template.html
git commit -m "feat: product description long-form text section"
```

---

### Task 5: Technical table section

**Files:**
- Modify: `docs/basi-prodotto-template.html` — add table CSS + HTML

- [ ] **Step 1: Add table CSS inside `<style>`**

Append after the description CSS:

```css
/* ── SCHEDA TECNICA TABLE ────────────── */
.product-table-section { padding: 80px 0; background: #fff; }
.table-wrap { overflow-x: auto; margin-top: 40px; }
.product-table {
  width: 100%;
  max-width: 860px;
  border-collapse: collapse;
  font-family: var(--font-b);
}
.product-table thead tr {
  background: var(--rosso);
}
.product-table thead th {
  padding: 14px 20px;
  font-size: 10px;
  letter-spacing: .2em;
  text-transform: uppercase;
  color: #fff;
  font-weight: 500;
  text-align: left;
}
.product-table tbody tr:nth-child(odd)  { background: var(--bianco); }
.product-table tbody tr:nth-child(even) { background: #fff; }
.product-table tbody tr { border-bottom: 1px solid rgba(169,72,61,.07); }
.product-table tbody td {
  padding: 13px 20px;
  font-size: 13px;
  color: #555;
  vertical-align: top;
  line-height: 1.5;
}
.product-table tbody td:first-child {
  font-size: 11px;
  letter-spacing: .1em;
  text-transform: uppercase;
  color: var(--nero);
  font-weight: 500;
  white-space: nowrap;
}
.product-table tbody td:last-child {
  font-family: var(--font-s);
  font-size: 14px;
  font-style: italic;
  color: #888;
}
```

- [ ] **Step 2: Add table HTML**

After the closing `</section>` of `.product-description`, add:

```html
<!-- SCHEDA TECNICA -->
<section class="product-table-section">
  <div class="container">
    <div class="reveal">
      <span class="label">Dati Tecnici</span>
      <div class="divider"></div>
      <h2 class="section-title">SCHEDA<br>TECNICA</h2>
    </div>
    <div class="table-wrap reveal">
      <table class="product-table">
        <thead>
          <tr>
            <th>Parametro</th>
            <th>Valore</th>
            <th>Note</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Formato disponibile</td>
            <td>30 cm &middot; 33 cm &middot; 36 cm</td>
            <td>Misura riferita al diametro della base stesa</td>
          </tr>
          <tr>
            <td>Peso</td>
            <td>250 g &middot; 300 g &middot; 350 g</td>
            <td>Peso netto per singola base surgelata</td>
          </tr>
          <tr>
            <td>Idratazione impasto</td>
            <td>72%</td>
            <td>Alta idratazione per massima alveolatura</td>
          </tr>
          <tr>
            <td>Tipo di farina</td>
            <td>Tipo 1 con germe di grano</td>
            <td>Macinazione a 24 passaggi, filiera Molino Madre</td>
          </tr>
          <tr>
            <td>Lievitazione</td>
            <td>48 ore a temperatura controllata</td>
            <td>Lievito madre naturale, senza agenti chimici</td>
          </tr>
          <tr>
            <td>Precottura</td>
            <td>Forno a legna con base in pietra</td>
            <td>Temperatura 350–400°C</td>
          </tr>
          <tr>
            <td>Cottura finale</td>
            <td>3&ndash;4 min a 280&deg;C</td>
            <td>Forno a legna, elettrico o a gas</td>
          </tr>
          <tr>
            <td>Conservazione</td>
            <td>Surgelato &mdash;18&deg;C</td>
            <td>Mantenere la catena del freddo</td>
          </tr>
          <tr>
            <td>Scadenza</td>
            <td>12 mesi dalla produzione</td>
            <td>Vedere data impressa sul cartone</td>
          </tr>
          <tr>
            <td>Confezionamento</td>
            <td>Cartone da 10 basi</td>
            <td>Pallet disponibile su richiesta</td>
          </tr>
          <tr>
            <td>Additivi</td>
            <td>Nessuno</td>
            <td>Conservato naturalmente dal freddo</td>
          </tr>
          <tr>
            <td>Certificazioni</td>
            <td>IFS Food &middot; BRC Food</td>
            <td>Audit annuale da ente certificatore terzo</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Open in browser, verify table**

Expected: red header row with white uppercase text, alternating bianco/white rows, parameter column in bold uppercase, note column in italic serif. Table scrolls horizontally if viewport is narrow.

- [ ] **Step 4: Commit**

```bash
git add docs/basi-prodotto-template.html
git commit -m "feat: scheda tecnica table – 12 rows, 3 columns"
```

---

### Task 6: CTA block + Footer + Sticky contact

**Files:**
- Modify: `docs/basi-prodotto-template.html` — add CTA, footer, sticky HTML

- [ ] **Step 1: Add CTA + footer + sticky HTML**

After the closing `</section>` of `.product-table-section`, add:

```html
<!-- CTA -->
<section class="cta-section">
  <div class="cta-pattern"></div>
  <div class="cta-inner">
    <div class="container">
      <p class="cta-label">Vuoi distribuire le nostre basi?</p>
      <h2 class="cta-title">HAI BISOGNO<br>DI UN PARTNER?</h2>
      <p class="cta-text">Offriamo formati personalizzati, produzione flessibile e distribuzione anche all&rsquo;estero. Parla con un nostro consulente per trovare la soluzione pi&ugrave; adatta alla tua attivit&agrave;.</p>
      <a href="consulenza.html" class="btn-cta">Richiedi Consulenza &#8594;</a>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="container">
    <div class="footer-top">
      <div>
        <div class="footer-brand-name">MOLINO MADRE</div>
        <div class="footer-brand-sub">Dal 1939 &middot; Calabria</div>
        <p class="footer-tagline">&ldquo;Il grano macinato lentamente, il lievito madre che fermenta con pazienza, la tradizione che innova.&rdquo;</p>
      </div>
      <div class="footer-col">
        <h5>Prodotti</h5>
        <ul>
          <li><a href="farine.html">Farine</a></li>
          <li><a href="basi.html">Basi per Pizza</a></li>
          <li><a href="panetti.html">Panetti Surgelati</a></li>
          <li><a href="#">I Regionali</a></li>
        </ul>
      </div>
      <div class="footer-col">
        <h5>Azienda</h5>
        <ul>
          <li><a href="il-molino.html">Il Molino</a></li>
          <li><a href="il-forno-del-mulino.html">Il Forno del Mulino</a></li>
          <li><a href="consulenza.html">Consulenza</a></li>
          <li><a href="news.html">News</a></li>
        </ul>
      </div>
      <div class="footer-col">
        <h5>Contatti</h5>
        <ul>
          <li><a href="mailto:info@molinomadre.it">info@molinomadre.it</a></li>
          <li><a href="tel:+390000000000">+39 000 000 0000</a></li>
          <li><a href="#">Calabria, Italia</a></li>
          <li><a href="#">Instagram</a></li>
          <li><a href="#">LinkedIn</a></li>
        </ul>
      </div>
    </div>
    <div class="footer-bottom">
      <p class="footer-copy">&copy; 2025 Molino Madre S.r.l. &nbsp;&middot;&nbsp; Privacy Policy &nbsp;&middot;&nbsp; Cookie Policy</p>
      <div class="footer-socials">
        <a href="#" class="footer-social-link" title="Instagram">IG</a>
        <a href="#" class="footer-social-link" title="Facebook">FB</a>
        <a href="#" class="footer-social-link" title="LinkedIn">LI</a>
        <a href="#" class="footer-social-link" title="YouTube">YT</a>
      </div>
    </div>
  </div>
</footer>

<!-- STICKY BAR -->
<div class="sticky-contact">
  <a href="https://wa.me/39000000000" class="sticky-btn sticky-wa">
    <svg class="icon-svg" viewBox="0 0 24 24" fill="currentColor"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347z"/><path d="M12 0C5.373 0 0 5.373 0 12c0 2.118.552 4.107 1.516 5.842L0 24l6.336-1.483A11.953 11.953 0 0012 24c6.627 0 12-5.373 12-12S18.627 0 12 0zm0 22c-1.861 0-3.601-.5-5.1-1.371l-.364-.217-3.762.881.941-3.673-.237-.374A9.944 9.944 0 012 12C2 6.477 6.477 2 12 2s10 4.477 10 10-4.477 10-10 10z"/></svg>
    WhatsApp
  </a>
  <a href="contatti.html" class="sticky-btn sticky-main">
    <svg class="icon-svg" viewBox="0 0 24 24" fill="currentColor"><path d="M20 4H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/></svg>
    Contatti
  </a>
</div>
```

- [ ] **Step 2: Open in browser, verify full page**

Expected: red CTA block with grid pattern, dark footer with 4-column grid, sticky WhatsApp + Contatti buttons bottom-right. Scroll through the entire page.

- [ ] **Step 3: Commit**

```bash
git add docs/basi-prodotto-template.html
git commit -m "feat: CTA block, footer, sticky contact bar"
```

---

### Task 7: Responsive CSS — tablet and mobile

**Files:**
- Modify: `docs/basi-prodotto-template.html` — add responsive media queries

- [ ] **Step 1: Add responsive CSS inside `<style>`** (before `</style>`)

Append at the very end of the `<style>` block:

```css
/* ── RESPONSIVE ──────────────────────── */

/* Tablet: 768px – 1024px */
@media (max-width: 1024px) {
  .product-hero-grid { grid-template-columns: 1fr 1fr; gap: 40px; }
  .product-title { font-size: clamp(44px, 7vw, 72px); }
  .footer-top { grid-template-columns: 1fr 1fr; }
}

/* Tablet portrait / large mobile: ≤ 768px */
@media (max-width: 768px) {
  .container { padding: 0 20px; }
  nav { padding: 0 20px; display: grid; grid-template-columns: 1fr auto 1fr; }
  .nav-links { display: none; }
  .nav-cta { grid-column: 2; justify-self: center; }
  .nav-hamburger { display: flex; grid-column: 3; justify-self: end; align-self: center; }

  /* Hero: stack — gallery first, then info */
  .product-hero-grid {
    grid-template-columns: 1fr;
    gap: 32px;
  }
  .product-gallery { order: -1; }
  .product-info { order: 1; }

  .product-title { font-size: clamp(44px, 10vw, 72px); }
  .product-subtitle { font-size: 16px; max-width: 100%; }

  /* Spec rows: 2-column grid */
  .product-specs { display: grid; grid-template-columns: 1fr 1fr; gap: 16px 24px; }
  .spec-row { flex-direction: column; gap: 3px; }
  .spec-key { min-width: unset; }

  /* Description */
  .product-description-body { font-size: 16px; }

  /* Table */
  .product-table-section { padding: 60px 0; }

  /* Footer */
  .footer-top { grid-template-columns: 1fr; gap: 36px; }
}

/* Mobile: ≤ 480px */
@media (max-width: 480px) {
  .product-hero { padding: 40px 0 60px; }
  .product-title { font-size: clamp(40px, 12vw, 64px); }

  /* Spec rows: back to single column */
  .product-specs { grid-template-columns: 1fr; gap: 12px; }
  .spec-row { flex-direction: row; gap: 12px; }
  .spec-key { min-width: 90px; }

  /* Gallery thumbnails: show only 2 */
  .gallery-thumbs { gap: 8px; }
  .gallery-thumb:last-child { display: none; }

  /* Table: horizontal scroll */
  .table-wrap { overflow-x: auto; -webkit-overflow-scrolling: touch; }
  .product-table { min-width: 540px; }

  .product-description { padding: 60px 0; }
  .cta-section { padding: 60px 0; }
}
```

- [ ] **Step 2: Test tablet layout (768px)**

Resize browser to 768px width. Expected:
- Hero: gallery block on top, info below (order reversed)
- Spec rows: 2-column grid
- Nav: hamburger visible, links hidden

- [ ] **Step 3: Test mobile layout (375px)**

Resize to 375px. Expected:
- Spec rows: back to single column list
- Only 2 thumbnails visible
- Table has horizontal scroll indicator
- No text overflow anywhere

- [ ] **Step 4: Commit**

```bash
git add docs/basi-prodotto-template.html
git commit -m "feat: responsive layout – tablet 768px and mobile 480px breakpoints"
```

---

### Task 8: JavaScript — thumbnail swap, reveal, hamburger

**Files:**
- Modify: `docs/basi-prodotto-template.html` — add closing scripts before `</body>`

- [ ] **Step 1: Add scripts before `</body>`**

```html
<script>
/* Thumbnail gallery swap */
function swapImage(thumb, src, alt) {
  const main = document.getElementById('galleryMain');
  main.src = src;
  main.alt = alt;
  document.querySelectorAll('.gallery-thumb').forEach(t => t.classList.remove('active'));
  thumb.classList.add('active');
}

/* Reveal on scroll */
const revEls = document.querySelectorAll('.reveal');
const obs = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting) { e.target.classList.add('visible'); obs.unobserve(e.target); }
  });
}, { threshold: 0.08 });
revEls.forEach(el => obs.observe(el));

/* Hamburger menu */
const burger = document.getElementById('burger');
const mobileMenu = document.getElementById('mobileMenu');
const closeMenu = document.getElementById('closeMenu');
burger.addEventListener('click', () => mobileMenu.classList.add('open'));
closeMenu.addEventListener('click', () => mobileMenu.classList.remove('open'));
</script>
```

- [ ] **Step 2: Verify thumbnail swap works**

Open in browser. Click the 2nd and 3rd thumbnails. Expected: main image src changes, active border moves to clicked thumbnail.

- [ ] **Step 3: Verify reveal animations**

Scroll slowly down the page. Expected: each section fades in from below as it enters the viewport.

- [ ] **Step 4: Verify hamburger on mobile**

Resize to 375px. Tap hamburger icon. Expected: full-screen dark menu opens. Tap ✕ to close.

- [ ] **Step 5: Final commit**

```bash
git add docs/basi-prodotto-template.html
git commit -m "feat: JS thumbnail swap, reveal on scroll, hamburger menu"
```

---

## Self-Review Checklist

- [x] **Spec coverage:** Hero split (5fr/7fr) ✓ · Info column with label/title/tags/specs/CTA ✓ · Gallery with main + 3 thumbs ✓ · Long description ✓ · Technical table 3-col ✓ · CTA + footer ✓ · Tablet 768px ✓ · Mobile 480px ✓ · Thumbnail swap JS ✓ · Reveal JS ✓ · Hamburger JS ✓
- [x] **No placeholders:** All steps contain complete HTML/CSS/JS code
- [x] **Type consistency:** `swapImage()` defined in Task 8, called in Task 3 `onclick` — consistent. `galleryMain` id defined in Task 3, used in Task 8 — consistent. `.reveal` class used throughout, `.visible` applied by JS in Task 8 — consistent.
- [x] **File path:** Single output file `docs/basi-prodotto-template.html` — confirmed in all tasks
