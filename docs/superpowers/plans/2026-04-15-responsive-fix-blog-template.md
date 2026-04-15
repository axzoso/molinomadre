# Responsive Fix + Blog Article Template — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Correggere i problemi responsive in basi.html, panetti.html, contatti.html, news.html e creare un template articolo blog.

**Architecture:** Ogni pagina è un file HTML statico con CSS inline. Il menu mobile è identico in tutte le pagine (stesso pattern CSS + HTML + JS). Il fix basi.html aggiusta i breakpoint del product grid. Il template articolo è un nuovo file standalone che condivide il design system.

**Tech Stack:** HTML5, CSS3 (grid, custom properties), vanilla JS inline. Nessun build system. Nessun framework. Aprire i file direttamente nel browser per testare.

---

## File Map

| File | Tipo | Cosa cambia |
|---|---|---|
| `docs/basi.html` | Modify | CSS responsive grid fix (righe 209–230) + hamburger CSS + HTML + JS |
| `docs/panetti.html` | Modify | Hamburger CSS + HTML (riga 195) + JS (prima di `</body>` riga 503) |
| `docs/contatti.html` | Modify | Hamburger CSS + HTML (riga 165) + JS (prima del blocco script riga 340) |
| `docs/news.html` | Modify | Hamburger CSS + HTML (riga 180) + JS (prima di `</body>` riga 420) |
| `docs/news-articolo-template.html` | Create | Nuovo file completo |

---

## CSS Hamburger — Blocco riusabile (stesso in tutte le pagine)

Il blocco CSS sotto va aggiunto dentro il `<style>` di ciascuna pagina, **prima** del commento `/* ── RESPONSIVE */`. I selettori sono identici; solo contatti.html ha una variante per il colore nel pre-scroll (spiegata nel task 4).

```css
/* ── HAMBURGER / MOBILE MENU ─────────── */
.nav-hamburger {
  display: none; flex-direction: column; gap: 5px;
  cursor: pointer; padding: 8px; background: none; border: none;
  color: var(--nero);
}
.nav-hamburger span {
  display: block; width: 22px; height: 2px;
  background: currentColor; transition: all .3s;
}
.mobile-menu {
  display: none; position: fixed; inset: 0;
  background: rgba(26,20,16,.97); z-index: 1100;
  flex-direction: column; align-items: center;
  justify-content: center; gap: 32px;
}
.mobile-menu.open { display: flex; }
.mobile-menu a {
  font-family: var(--font-d); font-size: clamp(32px,6vw,48px);
  letter-spacing: .06em; color: #fff; text-decoration: none;
  transition: color .3s;
}
.mobile-menu a:hover { color: var(--terracotta); }
.mobile-menu-close {
  position: absolute; top: 24px; right: 24px;
  font-size: 28px; color: rgba(255,255,255,.5);
  cursor: pointer; background: none; border: none;
  font-family: var(--font-b); line-height: 1;
}
```

## Media query mobile da aggiungere/integrare (tutte le pagine tranne contatti)

```css
@media (max-width: 768px) {
  /* aggiungere DENTRO il blocco 768px già esistente: */
  nav { display: grid; grid-template-columns: 1fr auto 1fr; }
  .nav-cta { grid-column: 2; justify-self: center; }
  .nav-hamburger { display: flex; grid-column: 3; justify-self: end; align-self: center; }
}
```

## HTML da aggiungere in ogni `<nav>` (dopo `.nav-cta`)

```html
  <button class="nav-hamburger" id="burger" aria-label="Apri menu">
    <span></span><span></span><span></span>
  </button>
```

## HTML mobile-menu da aggiungere subito dopo `</nav>` (basi, panetti, news)

```html
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
```

## JS da aggiungere prima di `</body>` (tutte le pagine)

```html
<script>
const burger = document.getElementById('burger');
const mobileMenu = document.getElementById('mobileMenu');
const closeMenu = document.getElementById('closeMenu');
burger.addEventListener('click', () => mobileMenu.classList.add('open'));
closeMenu.addEventListener('click', () => mobileMenu.classList.remove('open'));
</script>
```

---

## Task 1: Fix responsive grid basi.html (Piccoli Formati / Formati Grandi)

**Files:**
- Modify: `docs/basi.html:209–230`

- [ ] **Step 1: Rimuovere la regola che crea l'orfano**

Nel blocco `@media (max-width: 1024px)` (riga 210–219 di `basi.html`), rimuovere la riga:
```css
  .products-row--3 { grid-template-columns: repeat(2, 1fr); }
```
Il blocco risultante deve essere:
```css
@media (max-width: 1024px) {
  .page-hero-inner { grid-template-columns: 1fr; }
  .page-hero-visual { display: none; }
  .process-grid { grid-template-columns: 1fr; }
  .process-visual { display: none; }
  .regionali-grid { grid-template-columns: 1fr; }
  .usp-grid { grid-template-columns: repeat(2, 1fr); }
  .footer-top { grid-template-columns: 1fr 1fr; }
}
```

- [ ] **Step 2: Aggiungere il breakpoint tablet (860px)**

Aggiungere il seguente blocco **tra** il blocco 1024px e il blocco 768px:
```css
@media (max-width: 860px) {
  .products-row--3 { grid-template-columns: repeat(3, 1fr); }
  .basi-card { aspect-ratio: 4/5; }
  .bc-name { font-size: 24px; }
  .bc-desc { display: none; }
  .bc-tags { display: none; }
}
```

- [ ] **Step 3: Aggiornare il blocco mobile (768px)**

Nel blocco `@media (max-width: 768px)` aggiungere le righe per ripristinare desc e name a colonna singola:
```css
@media (max-width: 768px) {
  .container { padding: 0 20px; }
  nav { padding: 0 20px; }
  .nav-links { display: none; }
  .products-row--3, .products-row--2 { grid-template-columns: 1fr; }
  .basi-card { aspect-ratio: 3/4; }
  .bc-name { font-size: 32px; }
  .bc-desc { display: block; }
  .bc-tags { display: flex; }
  .usp-grid { grid-template-columns: 1fr 1fr; }
  .footer-top { grid-template-columns: 1fr; }
  .cat-strip { flex-wrap: wrap; }
  .cat-strip-desc { display: none; }
  .page-hero-stats { gap: 28px; }
}
```

- [ ] **Step 4: Verifica visiva**

Aprire `docs/basi.html` nel browser. Verificare:
- A 1280px: 3 colonne portrait, nessun orfano ✓
- A 900px (DevTools): 3 colonne più strette, aspect 4/5, desc nascosta ✓ nessun orfano ✓
- A 420px (DevTools): 1 colonna piena, aspect 3/4, desc visibile ✓

- [ ] **Step 5: Commit**

```bash
git add docs/basi.html
git commit -m "Fix responsive grid basi.html: elimina card orfana a tablet"
```

---

## Task 2: Hamburger menu — basi.html

**Files:**
- Modify: `docs/basi.html` (CSS nel `<style>`, HTML nella `<nav>` e dopo, JS prima di `</body>`)

- [ ] **Step 1: Aggiungere CSS hamburger**

Nel `<style>` di `docs/basi.html`, aggiungere prima del commento `/* ── RESPONSIVE */` (riga 209) il blocco CSS hamburger dalla sezione "CSS Hamburger — Blocco riusabile" qui sopra.

Per contatti.html il colore hamburger è diverso — per basi.html il default `color: var(--nero)` è corretto poiché la nav è sempre su sfondo bianco/chiaro.

- [ ] **Step 2: Aggiornare il blocco `@media (max-width: 768px)`**

Al blocco 768px (già modificato nel Task 1), aggiungere le 3 righe nav grid:
```css
  nav { display: grid; grid-template-columns: 1fr auto 1fr; }
  .nav-cta { grid-column: 2; justify-self: center; }
  .nav-hamburger { display: flex; grid-column: 3; justify-self: end; align-self: center; }
```

Il blocco 768px completo deve risultare:
```css
@media (max-width: 768px) {
  .container { padding: 0 20px; }
  nav { padding: 0 20px; display: grid; grid-template-columns: 1fr auto 1fr; }
  .nav-links { display: none; }
  .nav-cta { grid-column: 2; justify-self: center; }
  .nav-hamburger { display: flex; grid-column: 3; justify-self: end; align-self: center; }
  .products-row--3, .products-row--2 { grid-template-columns: 1fr; }
  .basi-card { aspect-ratio: 3/4; }
  .bc-name { font-size: 32px; }
  .bc-desc { display: block; }
  .bc-tags { display: flex; }
  .usp-grid { grid-template-columns: 1fr 1fr; }
  .footer-top { grid-template-columns: 1fr; }
  .cat-strip { flex-wrap: wrap; }
  .cat-strip-desc { display: none; }
  .page-hero-stats { gap: 28px; }
}
```

- [ ] **Step 3: Aggiungere il button hamburger nella `<nav>`**

All'attuale `<nav id="mainNav">` in basi.html (riga 236–247), aggiungere il button dopo `.nav-cta` (riga 246):
```html
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
```

- [ ] **Step 4: Aggiungere il div mobile-menu dopo `</nav>`**

Subito dopo `</nav>` (riga 247 attuale), inserire:
```html
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
```

- [ ] **Step 5: Aggiungere il JS hamburger prima di `</body>`**

Prima del tag `</body>` (riga 646), aggiungere subito dopo lo `</script>` esistente:
```html
<script>
const burger = document.getElementById('burger');
const mobileMenu = document.getElementById('mobileMenu');
const closeMenu = document.getElementById('closeMenu');
burger.addEventListener('click', () => mobileMenu.classList.add('open'));
closeMenu.addEventListener('click', () => mobileMenu.classList.remove('open'));
</script>
```

- [ ] **Step 6: Verifica visiva**

Aprire `docs/basi.html` in DevTools a 390px (iPhone 14):
- La navbar mostra: logo sinistra | CONTATTI centrato | ☰ destra ✓
- Click su ☰: overlay scuro fullscreen con i link grandi bianchi ✓
- Click su ✕: overlay si chiude ✓
- I link desktop (`.nav-links`) non sono visibili ✓

- [ ] **Step 7: Commit**

```bash
git add docs/basi.html
git commit -m "Aggiunto hamburger menu mobile a basi.html"
```

---

## Task 3: Hamburger menu — panetti.html

**Files:**
- Modify: `docs/panetti.html`

- [ ] **Step 1: Aggiungere CSS hamburger**

Nel `<style>` di `docs/panetti.html`, aggiungere prima del commento `/* ── RESPONSIVE */` (riga ~164) il blocco CSS hamburger dalla sezione "CSS Hamburger — Blocco riusabile".

Il colore hamburger default `var(--nero)` è corretto (nav sempre su sfondo bianco).

- [ ] **Step 2: Aggiornare il blocco `@media (max-width: 768px)`**

Il blocco attuale (righe 173–179) deve diventare:
```css
@media (max-width: 768px) {
  .container { padding: 0 20px; }
  nav { padding: 0 20px; display: grid; grid-template-columns: 1fr auto 1fr; }
  .nav-links { display: none; }
  .nav-cta { grid-column: 2; justify-self: center; }
  .nav-hamburger { display: flex; grid-column: 3; justify-self: end; align-self: center; }
  .footer-top { grid-template-columns: 1fr; }
  .range-grid { grid-template-columns: repeat(2,1fr); }
}
```

- [ ] **Step 3: Aggiungere il button hamburger nella `<nav>`**

La `<nav>` attuale (righe 185–196) diventa:
```html
<nav>
  <a href="index.html" class="nav-logo">MOLINO MADRE<span>Dal 1939 · Calabria</span></a>
  <ul class="nav-links">
    <li><a href="il-molino.html">Il Mulino</a></li>
    <li><a href="farine.html">Farine</a></li>
    <li><a href="basi.html">Basi</a></li>
    <li><a href="panetti.html" class="active">Panetti</a></li>
    <li><a href="il-forno-del-mulino.html">Il Forno</a></li>
    <li><a href="consulenza.html">Consulenza</a></li>
  </ul>
  <a href="contatti.html" class="nav-cta">Contatti</a>
  <button class="nav-hamburger" id="burger" aria-label="Apri menu">
    <span></span><span></span><span></span>
  </button>
</nav>
```

- [ ] **Step 4: Aggiungere il div mobile-menu dopo `</nav>` (riga 196)**

```html
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
```

- [ ] **Step 5: Aggiungere il JS hamburger prima di `</body>`**

Prima del tag `</body>` (riga 504), dopo lo `</script>` esistente:
```html
<script>
const burger = document.getElementById('burger');
const mobileMenu = document.getElementById('mobileMenu');
const closeMenu = document.getElementById('closeMenu');
burger.addEventListener('click', () => mobileMenu.classList.add('open'));
closeMenu.addEventListener('click', () => mobileMenu.classList.remove('open'));
</script>
```

- [ ] **Step 6: Verifica visiva**

Aprire `docs/panetti.html` in DevTools a 390px:
- Navbar: logo | CONTATTI | ☰ ✓
- Hamburger apre overlay ✓, X chiude ✓

- [ ] **Step 7: Commit**

```bash
git add docs/panetti.html
git commit -m "Aggiunto hamburger menu mobile a panetti.html"
```

---

## Task 4: Hamburger menu — contatti.html

**Files:**
- Modify: `docs/contatti.html`

Attenzione: in contatti.html la navbar parte **trasparente** (sfondo `var(--nero)` hero) e logo + link sono bianchi. Diventa bianca solo dopo scroll (classe `.scrolled`). Il hamburger deve essere bianco prima dello scroll e scuro dopo. Si gestisce aggiungendo la regola `.scrolled .nav-hamburger`.

- [ ] **Step 1: Aggiungere CSS hamburger (variante contatti)**

Nel `<style>` di `docs/contatti.html`, aggiungere prima del commento `/* ── RESPONSIVE */` (riga ~135):
```css
/* ── HAMBURGER / MOBILE MENU ─────────── */
.nav-hamburger {
  display: none; flex-direction: column; gap: 5px;
  cursor: pointer; padding: 8px; background: none; border: none;
  color: rgba(255,255,255,.85);
}
.nav-hamburger span {
  display: block; width: 22px; height: 2px;
  background: currentColor; transition: all .3s;
}
nav.scrolled .nav-hamburger { color: var(--nero); }
.mobile-menu {
  display: none; position: fixed; inset: 0;
  background: rgba(26,20,16,.97); z-index: 1100;
  flex-direction: column; align-items: center;
  justify-content: center; gap: 32px;
}
.mobile-menu.open { display: flex; }
.mobile-menu a {
  font-family: var(--font-d); font-size: clamp(32px,6vw,48px);
  letter-spacing: .06em; color: #fff; text-decoration: none;
  transition: color .3s;
}
.mobile-menu a:hover { color: var(--terracotta); }
.mobile-menu-close {
  position: absolute; top: 24px; right: 24px;
  font-size: 28px; color: rgba(255,255,255,.5);
  cursor: pointer; background: none; border: none;
  font-family: var(--font-b); line-height: 1;
}
```

- [ ] **Step 2: Aggiornare il blocco `@media (max-width: 768px)`**

Il blocco attuale (righe 142–149) diventa:
```css
@media (max-width: 768px) {
  .container { padding:0 20px; }
  nav { padding:0 20px; display:grid; grid-template-columns:1fr auto 1fr; }
  .nav-links { display:none; }
  .nav-cta { grid-column:2; justify-self:center; }
  .nav-hamburger { display:flex; grid-column:3; justify-self:end; align-self:center; }
  .hero-visual-strip { display:none; }
  .form-row { grid-template-columns:1fr; }
  .footer-top { grid-template-columns:1fr; }
}
```

- [ ] **Step 3: Aggiungere il button hamburger nella `<nav>`**

La `<nav id="mainNav">` (righe 155–166) diventa:
```html
<nav id="mainNav">
  <a href="index.html" class="nav-logo">MOLINO MADRE<span>Dal 1939 · Calabria</span></a>
  <ul class="nav-links">
    <li><a href="il-molino.html">Il Mulino</a></li>
    <li><a href="farine.html">Farine</a></li>
    <li><a href="basi.html">Basi</a></li>
    <li><a href="panetti.html">Panetti</a></li>
    <li><a href="il-forno-del-mulino.html">Il Forno</a></li>
    <li><a href="consulenza.html">Consulenza</a></li>
  </ul>
  <a href="contatti.html" class="nav-cta" style="background:var(--rosso);border-color:var(--rosso);color:#fff;">Contatti</a>
  <button class="nav-hamburger" id="burger" aria-label="Apri menu">
    <span></span><span></span><span></span>
  </button>
</nav>
```

- [ ] **Step 4: Aggiungere il div mobile-menu dopo `</nav>` (riga 166)**

```html
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
```

- [ ] **Step 5: Aggiungere il JS hamburger nel blocco `<script>` esistente**

Il blocco script attuale (righe 340–348) diventa:
```html
<script>
const nav = document.getElementById('mainNav');
window.addEventListener('scroll', () => { nav.classList.toggle('scrolled', window.scrollY > 60); },{passive:true});
const revEls = document.querySelectorAll('.reveal');
const obs = new IntersectionObserver(entries => {
  entries.forEach(e => { if(e.isIntersecting){ e.target.classList.add('visible'); obs.unobserve(e.target); } });
},{threshold:0.08});
revEls.forEach(el => obs.observe(el));
const burger = document.getElementById('burger');
const mobileMenu = document.getElementById('mobileMenu');
const closeMenu = document.getElementById('closeMenu');
burger.addEventListener('click', () => mobileMenu.classList.add('open'));
closeMenu.addEventListener('click', () => mobileMenu.classList.remove('open'));
</script>
```

- [ ] **Step 6: Verifica visiva**

Aprire `docs/contatti.html` in DevTools a 390px:
- All'apertura (nav trasparente): hamburger icon è bianca ✓
- Dopo scroll 70px: hamburger icon diventa scura ✓
- Click ☰: overlay si apre ✓

- [ ] **Step 7: Commit**

```bash
git add docs/contatti.html
git commit -m "Aggiunto hamburger menu mobile a contatti.html"
```

---

## Task 5: Hamburger menu — news.html

**Files:**
- Modify: `docs/news.html`

- [ ] **Step 1: Aggiungere CSS hamburger**

Nel `<style>` di `docs/news.html`, aggiungere prima del commento `/* ── RESPONSIVE */` (riga ~146) il blocco CSS hamburger dalla sezione "CSS Hamburger — Blocco riusabile" (colore default `var(--nero)`).

- [ ] **Step 2: Aggiornare il blocco `@media (max-width: 768px)`**

Il blocco attuale (righe 154–164) diventa:
```css
@media (max-width:768px) {
  .container { padding:0 20px; }
  nav { padding:0 20px; display:grid; grid-template-columns:1fr auto 1fr; }
  .nav-links { display:none; }
  .nav-cta { grid-column:2; justify-self:center; }
  .nav-hamburger { display:flex; grid-column:3; justify-self:end; align-self:center; }
  .articles-grid { grid-template-columns:1fr; }
  .page-header-inner { flex-direction:column; gap:20px; }
  .footer-top { grid-template-columns:1fr; }
  .newsletter-form { flex-direction:column; }
  .newsletter-input { border-right:1px solid rgba(255,255,255,.12); border-bottom:none; border-radius:2px 2px 0 0; }
  .newsletter-btn { border-radius:0 0 2px 2px; }
}
```

- [ ] **Step 3: Aggiungere il button hamburger nella `<nav>`**

La `<nav>` attuale (righe 170–181) diventa:
```html
<nav>
  <a href="index.html" class="nav-logo">MOLINO MADRE<span>Dal 1939 · Calabria</span></a>
  <ul class="nav-links">
    <li><a href="il-molino.html">Il Mulino</a></li>
    <li><a href="farine.html">Farine</a></li>
    <li><a href="basi.html">Basi</a></li>
    <li><a href="panetti.html">Panetti</a></li>
    <li><a href="il-forno-del-mulino.html">Il Forno</a></li>
    <li><a href="news.html" class="active">News</a></li>
  </ul>
  <a href="contatti.html" class="nav-cta">Contatti</a>
  <button class="nav-hamburger" id="burger" aria-label="Apri menu">
    <span></span><span></span><span></span>
  </button>
</nav>
```

- [ ] **Step 4: Aggiungere il div mobile-menu dopo `</nav>` (riga 181)**

```html
<div class="mobile-menu" id="mobileMenu">
  <button class="mobile-menu-close" id="closeMenu">&#x2715;</button>
  <a href="il-molino.html">Il Mulino</a>
  <a href="farine.html">Farine</a>
  <a href="basi.html">Basi</a>
  <a href="panetti.html">Panetti</a>
  <a href="il-forno-del-mulino.html">Il Forno</a>
  <a href="news.html">News</a>
  <a href="contatti.html">Contatti</a>
</div>
```

- [ ] **Step 5: Aggiungere il JS hamburger prima di `</body>`**

Nel blocco `<script>` esistente (righe 408–420), aggiungere prima del tag `</script>` finale:
```js
const burger = document.getElementById('burger');
const mobileMenu = document.getElementById('mobileMenu');
const closeMenu = document.getElementById('closeMenu');
burger.addEventListener('click', () => mobileMenu.classList.add('open'));
closeMenu.addEventListener('click', () => mobileMenu.classList.remove('open'));
```

- [ ] **Step 6: Verifica visiva**

Aprire `docs/news.html` in DevTools a 390px:
- Navbar: logo | CONTATTI | ☰ ✓
- Hamburger funziona ✓

- [ ] **Step 7: Commit**

```bash
git add docs/news.html
git commit -m "Aggiunto hamburger menu mobile a news.html"
```

---

## Task 6: Creare news-articolo-template.html

**Files:**
- Create: `docs/news-articolo-template.html`

- [ ] **Step 1: Creare il file con il seguente contenuto completo**

```html
<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Lenta Macinazione: perché fa la differenza – Molino Madre Dal 1939</title>
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
*, *::before, *::after { box-sizing: border-box; margin:0; padding:0; }
html { scroll-behavior: smooth; }
body { background: var(--bianco); color: var(--nero); font-family: var(--font-b); overflow-x: hidden; }
.container { max-width: 1280px; margin: 0 auto; padding: 0 40px; }
.label { font-family: var(--font-b); font-size: 11px; letter-spacing: .25em; text-transform: uppercase; color: var(--terracotta); display: block; margin-bottom: 16px; }
.divider { display: inline-block; width: 48px; height: 1px; background: var(--terracotta); margin-bottom: 20px; }

/* ── NAV ──────────────────────────────── */
nav { position: fixed; top:0; left:0; right:0; z-index:1000; padding:0 40px; height:72px; display:flex; align-items:center; justify-content:space-between; background:rgba(245,242,236,.96); backdrop-filter:blur(12px); border-bottom:1px solid rgba(138,36,50,.1); }
.nav-logo { font-family:var(--font-d); font-size:22px; letter-spacing:.12em; color:var(--rosso); text-decoration:none; display:flex; flex-direction:column; line-height:1.1; }
.nav-logo span { font-family:var(--font-s); font-size:10px; letter-spacing:.3em; text-transform:uppercase; opacity:.7; color:var(--nero); }
.nav-links { display:flex; gap:32px; list-style:none; }
.nav-links a { font-size:11px; letter-spacing:.18em; text-transform:uppercase; text-decoration:none; color:var(--nero); transition:color .3s; }
.nav-links a:hover, .nav-links a.active { color:var(--terracotta); }
.nav-cta { padding:9px 22px; border:1.5px solid var(--rosso); border-radius:2px; font-size:10px; letter-spacing:.2em; text-transform:uppercase; text-decoration:none; color:var(--rosso); transition:all .3s; font-weight:500; }
.nav-cta:hover { background:var(--rosso); color:#fff; }

/* ── HAMBURGER / MOBILE MENU ─────────── */
.nav-hamburger { display:none; flex-direction:column; gap:5px; cursor:pointer; padding:8px; background:none; border:none; color:var(--nero); }
.nav-hamburger span { display:block; width:22px; height:2px; background:currentColor; transition:all .3s; }
.mobile-menu { display:none; position:fixed; inset:0; background:rgba(26,20,16,.97); z-index:1100; flex-direction:column; align-items:center; justify-content:center; gap:32px; }
.mobile-menu.open { display:flex; }
.mobile-menu a { font-family:var(--font-d); font-size:clamp(32px,6vw,48px); letter-spacing:.06em; color:#fff; text-decoration:none; transition:color .3s; }
.mobile-menu a:hover { color:var(--terracotta); }
.mobile-menu-close { position:absolute; top:24px; right:24px; font-size:28px; color:rgba(255,255,255,.5); cursor:pointer; background:none; border:none; font-family:var(--font-b); line-height:1; }

/* ── BREADCRUMB ──────────────────────── */
.breadcrumb { padding:100px 0 0; }
.breadcrumb-inner { display:flex; align-items:center; gap:10px; font-size:10px; letter-spacing:.2em; text-transform:uppercase; color:rgba(26,20,16,.35); }
.breadcrumb-inner a { color:inherit; text-decoration:none; transition:color .3s; }
.breadcrumb-inner a:hover { color:var(--terracotta); }
.breadcrumb-sep { color:var(--terracotta); }

/* ── ARTICLE HERO ────────────────────── */
.article-hero { background:var(--nero); padding:72px 0 80px; position:relative; overflow:hidden; margin-top:-1px; }
.article-hero-bg { position:absolute; inset:0; background:radial-gradient(ellipse 60% 80% at 20% 60%, rgba(138,36,50,.2) 0%, transparent 55%); }
.article-hero-grain { position:absolute; inset:0; background-image:url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E"); }
.article-hero-inner { position:relative; z-index:2; max-width:860px; }
.article-category { display:inline-flex; align-items:center; gap:12px; margin-bottom:28px; }
.article-cat-label { font-size:10px; letter-spacing:.28em; text-transform:uppercase; color:var(--grano); opacity:.75; }
.article-cat-sep { width:32px; height:1px; background:var(--terracotta); }
.article-hero-title { font-family:var(--font-d); font-size:clamp(44px,6vw,88px); line-height:.9; letter-spacing:.02em; color:#fff; margin-bottom:28px; }
.article-hero-title em { font-family:var(--font-s); font-style:italic; font-weight:300; font-size:.5em; color:var(--grano); display:block; margin-bottom:12px; letter-spacing:.04em; }
.article-hero-intro { font-family:var(--font-s); font-size:20px; font-weight:300; color:rgba(255,255,255,.55); line-height:1.75; max-width:640px; margin-bottom:44px; }
.article-meta-bar { display:flex; align-items:center; gap:28px; padding-top:28px; border-top:1px solid rgba(255,255,255,.07); flex-wrap:wrap; }
.meta-item { display:flex; flex-direction:column; gap:3px; }
.meta-label { font-size:9px; letter-spacing:.22em; text-transform:uppercase; color:rgba(255,255,255,.3); }
.meta-value { font-family:var(--font-s); font-size:14px; font-style:italic; color:rgba(255,255,255,.7); }
.meta-sep { width:1px; height:36px; background:rgba(255,255,255,.08); }

/* ── ARTICLE COVER IMAGE ─────────────── */
.article-cover { aspect-ratio:16/9; background:linear-gradient(145deg,#3a1a10 0%,#2a1008 50%,#1a0c06 100%); position:relative; overflow:hidden; }
.article-cover::after { content:'IMMAGINE ARTICOLO'; position:absolute; inset:0; display:flex; align-items:center; justify-content:center; font-family:var(--font-d); font-size:14px; letter-spacing:.25em; color:rgba(255,255,255,.12); }

/* ── ARTICLE BODY LAYOUT ─────────────── */
.article-body { padding:80px 0 100px; }
.article-layout { display:grid; grid-template-columns: 2fr 1fr; gap:80px; align-items:start; }

/* ── ARTICLE CONTENT ─────────────────── */
.article-content { }
.article-lead { font-family:var(--font-s); font-size:20px; font-weight:400; line-height:1.8; color:var(--nero); margin-bottom:36px; }
.article-text p { font-family:var(--font-s); font-size:18px; font-weight:300; line-height:1.85; color:#444; margin-bottom:28px; }
.article-text h2 { font-family:var(--font-d); font-size:36px; letter-spacing:.04em; color:var(--rosso); margin:52px 0 20px; line-height:1; }
.article-text h3 { font-family:var(--font-d); font-size:22px; letter-spacing:.06em; color:var(--terracotta); margin:36px 0 16px; }
.article-quote { border-left:3px solid var(--rosso); padding:24px 32px; background:rgba(138,36,50,.04); margin:44px 0; border-radius:0 2px 2px 0; }
.article-quote blockquote { font-family:var(--font-s); font-size:20px; font-style:italic; font-weight:300; color:var(--nero); line-height:1.7; }
.article-quote cite { display:block; margin-top:14px; font-size:10px; letter-spacing:.22em; text-transform:uppercase; color:var(--terracotta); font-style:normal; opacity:.75; }
.article-img-inline { aspect-ratio:16/9; background:linear-gradient(145deg,#2a1810 0%,#1a0e08 100%); position:relative; overflow:hidden; margin:44px 0; border-radius:2px; }
.article-img-inline::after { content:'IMMAGINE SEZIONE'; position:absolute; inset:0; display:flex; align-items:center; justify-content:center; font-family:var(--font-d); font-size:12px; letter-spacing:.2em; color:rgba(255,255,255,.15); }
.article-img-caption { font-size:11px; color:rgba(26,20,16,.4); letter-spacing:.06em; margin-top:-36px; margin-bottom:44px; font-family:var(--font-b); }
.article-tags { display:flex; flex-wrap:wrap; gap:8px; margin-top:48px; padding-top:32px; border-top:1px solid rgba(138,36,50,.08); }
.article-tag { font-size:9px; letter-spacing:.18em; text-transform:uppercase; border:1px solid rgba(138,36,50,.2); color:var(--terracotta); padding:5px 12px; border-radius:2px; }

/* ── SIDEBAR ─────────────────────────── */
.article-sidebar { position:sticky; top:100px; display:flex; flex-direction:column; gap:24px; }
.sidebar-box { background:#fff; border:1px solid rgba(138,36,50,.07); padding:28px; }
.sidebar-box-title { font-family:var(--font-d); font-size:13px; letter-spacing:.2em; color:var(--rosso); margin-bottom:20px; padding-bottom:12px; border-bottom:1px solid rgba(138,36,50,.07); }
.related-item { display:flex; flex-direction:column; gap:4px; padding:12px 0; border-bottom:1px solid rgba(26,20,16,.05); text-decoration:none; }
.related-item:last-child { border-bottom:none; padding-bottom:0; }
.related-cat { font-size:9px; letter-spacing:.2em; text-transform:uppercase; color:var(--terracotta); opacity:.7; }
.related-title { font-family:var(--font-d); font-size:16px; letter-spacing:.04em; color:var(--nero); line-height:1.1; transition:color .3s; }
.related-item:hover .related-title { color:var(--rosso); }
.product-link { display:flex; align-items:center; justify-content:space-between; padding:10px 0; border-bottom:1px solid rgba(26,20,16,.05); text-decoration:none; }
.product-link:last-child { border-bottom:none; }
.pl-name { font-family:var(--font-s); font-size:15px; font-style:italic; color:var(--nero); }
.pl-arrow { color:var(--terracotta); font-size:14px; }
.product-link:hover .pl-name { color:var(--rosso); }
.sidebar-newsletter { background:var(--nero); padding:28px; }
.sidebar-newsletter .sidebar-box-title { color:var(--grano); border-bottom-color:rgba(221,201,162,.1); }
.sn-text { font-family:var(--font-s); font-size:14px; font-weight:300; color:rgba(255,255,255,.45); line-height:1.6; margin-bottom:20px; }
.sn-input { width:100%; padding:10px 14px; background:rgba(255,255,255,.05); border:1px solid rgba(255,255,255,.12); border-radius:2px; color:#fff; font-size:13px; font-family:var(--font-b); margin-bottom:10px; }
.sn-input::placeholder { color:rgba(255,255,255,.3); }
.sn-btn { width:100%; padding:11px; background:var(--rosso); color:#fff; border:none; border-radius:2px; font-size:10px; letter-spacing:.22em; text-transform:uppercase; font-weight:500; cursor:pointer; font-family:var(--font-b); transition:background .3s; }
.sn-btn:hover { background:var(--terracotta); }

/* ── CTA SECTION ─────────────────────── */
.cta-section { background:var(--rosso); padding:90px 0; text-align:center; position:relative; overflow:hidden; }
.cta-pattern { position:absolute; inset:0; background:repeating-linear-gradient(90deg,transparent,transparent 80px,rgba(255,255,255,.025) 80px,rgba(255,255,255,.025) 81px),repeating-linear-gradient(0deg,transparent,transparent 80px,rgba(255,255,255,.025) 80px,rgba(255,255,255,.025) 81px); }
.cta-inner { position:relative; z-index:2; }
.cta-label { font-size:10px; letter-spacing:.3em; text-transform:uppercase; color:rgba(255,255,255,.45); margin-bottom:20px; }
.cta-title { font-family:var(--font-d); font-size:clamp(36px,5vw,64px); line-height:.95; color:#fff; margin-bottom:20px; }
.cta-text { font-family:var(--font-s); font-size:17px; font-weight:300; color:rgba(255,255,255,.6); line-height:1.75; max-width:520px; margin:0 auto 40px; }
.btn-cta { display:inline-flex; align-items:center; gap:12px; padding:16px 36px; background:var(--grano); color:var(--rosso); font-family:var(--font-b); font-size:11px; letter-spacing:.22em; text-transform:uppercase; font-weight:600; text-decoration:none; border-radius:2px; transition:background .3s; }
.btn-cta:hover { background:#fff; }

/* ── FOOTER ──────────────────────────── */
footer { background:var(--nero); padding:80px 0 40px; }
.footer-top { display:grid; grid-template-columns:2fr 1fr 1fr 1fr; gap:48px; padding-bottom:56px; border-bottom:1px solid rgba(255,255,255,.05); margin-bottom:36px; }
.footer-brand-name { font-family:var(--font-d); font-size:30px; letter-spacing:.12em; color:#fff; line-height:1; margin-bottom:4px; }
.footer-brand-sub { font-family:var(--font-s); font-size:11px; letter-spacing:.28em; color:var(--grano); opacity:.6; text-transform:uppercase; margin-bottom:20px; }
.footer-tagline { font-family:var(--font-s); font-size:14px; font-weight:300; font-style:italic; color:rgba(255,255,255,.32); line-height:1.6; max-width:240px; }
.footer-col h5 { font-size:9px; letter-spacing:.28em; text-transform:uppercase; color:var(--grano); opacity:.7; margin-bottom:20px; }
.footer-col ul { list-style:none; display:flex; flex-direction:column; gap:10px; }
.footer-col ul li a { font-size:13px; color:rgba(255,255,255,.4); text-decoration:none; transition:color .3s; font-family:var(--font-s); }
.footer-col ul li a:hover { color:var(--grano); }
.footer-bottom { display:flex; align-items:center; justify-content:space-between; }
.footer-copy { font-size:10px; color:rgba(255,255,255,.2); letter-spacing:.06em; }
.footer-socials { display:flex; gap:12px; }
.footer-social-link { width:34px; height:34px; border:1px solid rgba(255,255,255,.1); border-radius:50%; display:flex; align-items:center; justify-content:center; text-decoration:none; color:rgba(255,255,255,.35); font-size:11px; font-family:var(--font-b); font-weight:500; transition:all .3s; }
.footer-social-link:hover { border-color:var(--grano); color:var(--grano); }

/* ── STICKY ──────────────────────────── */
.sticky-contact { position:fixed; bottom:24px; right:24px; z-index:999; display:flex; flex-direction:column; gap:8px; align-items:flex-end; }
.sticky-btn { display:flex; align-items:center; gap:10px; padding:11px 20px; border-radius:40px; font-size:10px; letter-spacing:.14em; text-transform:uppercase; font-weight:500; text-decoration:none; box-shadow:0 4px 20px rgba(0,0,0,.22); transition:transform .3s,box-shadow .3s; font-family:var(--font-b); }
.sticky-btn:hover { transform:translateY(-2px); }
.sticky-main { background:var(--rosso); color:var(--grano); }
.sticky-wa { background:#25D366; color:#fff; }
.icon-svg { width:14px; height:14px; }
.reveal { opacity:0; transform:translateY(36px); transition:opacity .85s ease,transform .85s ease; }
.reveal.visible { opacity:1; transform:translateY(0); }

/* ── RESPONSIVE ──────────────────────── */
@media (max-width: 1024px) {
  .article-layout { grid-template-columns:1fr; }
  .article-sidebar { position:static; }
  .footer-top { grid-template-columns:1fr 1fr; }
}
@media (max-width: 768px) {
  .container { padding:0 20px; }
  nav { padding:0 20px; display:grid; grid-template-columns:1fr auto 1fr; }
  .nav-links { display:none; }
  .nav-cta { grid-column:2; justify-self:center; }
  .nav-hamburger { display:flex; grid-column:3; justify-self:end; align-self:center; }
  .article-hero-title { font-size:clamp(40px,10vw,72px); }
  .article-lead { font-size:18px; }
  .article-text p { font-size:16px; }
  .article-quote blockquote { font-size:17px; }
  .footer-top { grid-template-columns:1fr; }
}
</style>
</head>
<body>

<!-- NAV -->
<nav>
  <a href="index.html" class="nav-logo">MOLINO MADRE<span>Dal 1939 · Calabria</span></a>
  <ul class="nav-links">
    <li><a href="il-molino.html">Il Mulino</a></li>
    <li><a href="farine.html">Farine</a></li>
    <li><a href="basi.html">Basi</a></li>
    <li><a href="panetti.html">Panetti</a></li>
    <li><a href="il-forno-del-mulino.html">Il Forno</a></li>
    <li><a href="news.html" class="active">News</a></li>
  </ul>
  <a href="contatti.html" class="nav-cta">Contatti</a>
  <button class="nav-hamburger" id="burger" aria-label="Apri menu">
    <span></span><span></span><span></span>
  </button>
</nav>

<!-- MOBILE MENU -->
<div class="mobile-menu" id="mobileMenu">
  <button class="mobile-menu-close" id="closeMenu">&#x2715;</button>
  <a href="il-molino.html">Il Mulino</a>
  <a href="farine.html">Farine</a>
  <a href="basi.html">Basi</a>
  <a href="panetti.html">Panetti</a>
  <a href="il-forno-del-mulino.html">Il Forno</a>
  <a href="news.html">News</a>
  <a href="contatti.html">Contatti</a>
</div>

<!-- BREADCRUMB -->
<div class="breadcrumb">
  <div class="container">
    <div class="breadcrumb-inner">
      <a href="index.html">Home</a>
      <span class="breadcrumb-sep">&#8594;</span>
      <a href="news.html">News</a>
      <span class="breadcrumb-sep">&#8594;</span>
      <span>Tecnica</span>
    </div>
  </div>
</div>

<!-- ARTICLE HERO -->
<section class="article-hero">
  <div class="article-hero-bg"></div>
  <div class="article-hero-grain"></div>
  <div class="container">
    <div class="article-hero-inner reveal">
      <div class="article-category">
        <span class="article-cat-label">Tecnica</span>
        <div class="article-cat-sep"></div>
      </div>
      <h1 class="article-hero-title">
        <em>Il segreto della nostra farina</em>
        LENTA MACINAZIONE:<br>PERCHÉ FA LA<br>DIFFERENZA
      </h1>
      <p class="article-hero-intro">24 passaggi di macinazione non sono un dettaglio tecnico: sono il motivo per cui le nostre farine preservano il germe di grano e garantiscono sapori e profumi unici che la grande industria non può replicare.</p>
      <div class="article-meta-bar">
        <div class="meta-item">
          <span class="meta-label">Data</span>
          <span class="meta-value">Marzo 2025</span>
        </div>
        <div class="meta-sep"></div>
        <div class="meta-item">
          <span class="meta-label">Autore</span>
          <span class="meta-value">Team Molino Madre</span>
        </div>
        <div class="meta-sep"></div>
        <div class="meta-item">
          <span class="meta-label">Lettura</span>
          <span class="meta-value">5 min</span>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- COVER IMAGE -->
<div class="article-cover"></div>

<!-- ARTICLE BODY -->
<section class="article-body">
  <div class="container">
    <div class="article-layout">

      <!-- CONTENT -->
      <div class="article-content reveal">
        <p class="article-lead">Quando parliamo di lenta macinazione, parliamo di una scelta precisa: rinunciare alla velocità industriale per preservare ciò che il grano porta con sé dalla natura. Una scelta che Molino Madre compie ogni giorno dal 1939.</p>

        <div class="article-text">
          <h2>IL PROCESSO IN 24 PASSAGGI</h2>
          <p>La macinazione industriale riduce il grano in farina in pochi passaggi ad alta velocità. Il calore generato dall'attrito distrugge buona parte del germe di grano, sede degli aromi più delicati e dei grassi insaturi. Il risultato è una farina tecnicamente performante ma povera di carattere.</p>
          <p>Il nostro processo prevede 24 passaggi successivi su macine a cilindri raffreddati. La temperatura del prodotto non supera mai i 24°C. Il germe di grano rimane intatto. Gli oli essenziali non si ossidano. L'aroma di grano arriva nella farina e, da lì, nel vostro impasto.</p>

          <div class="article-quote">
            <blockquote>«La farina è solo un tramite. Il nostro lavoro è portare il sapore del grano direttamente sulla tavola dei vostri clienti — intatto, come la natura lo ha creato.»</blockquote>
            <cite>Amodio &amp; Vittoria — Molino Madre</cite>
          </div>

          <h2>COSA CAMBIA NELL'IMPASTO</h2>
          <p>Una farina con germe intatto assorbe l'acqua in modo diverso. Gli impasti risultano più estensibili, con una maglia glutinica più elastica e una lievitazione più lenta e armonizzata. Per un pizzaiolo professionista, questo si traduce in meno correzioni in cella e più consistenza nel risultato finale.</p>
          <p>I panetti a 24h reggono la temperatura ambiente molto meglio. Le basi precotte mantengono la fragranza dopo il surgelamento. Non è magia: è chimica degli aromi preservata dal processo.</p>

          <div class="article-img-inline"></div>
          <p class="article-img-caption">Macinazione a cilindri raffreddati — Stabilimento Molino Madre, Calabria</p>

          <h3>IL RUOLO DEL GERME DI GRANO</h3>
          <p>Il germe rappresenta circa il 2–3% del chicco di grano ma concentra oltre il 25% dei suoi nutrienti e gran parte degli aromi. Contiene tocoferoli (vitamina E), acidi grassi insaturi, enzimi e zuccheri complessi che alimentano il processo fermentativo della lievitazione naturale.</p>
          <p>Preservarlo significa ottenere farine che lavorano in sinergia con il lievito madre, non contro di esso. I risultati sono visibili: alveolatura più aperta, crosticina più sottile, digeribilità superiore.</p>

          <h2>COME USARE AL MEGLIO LE NOSTRE FARINE</h2>
          <p>Per sfruttare appieno le proprietà di una farina con germe integro, raccomandare un'idratazione leggermente superiore alla norma (+ 3–5%) e tempi di lievitazione più lunghi (minimo 24h in cella a 4°C). La farina ha più fame di acqua e più capacità fermentativa: dategliene il tempo.</p>
          <p>Per gli impasti diretti, un autolisi di 30–60 minuti prima dell'aggiunta del lievito migliora notevolmente l'estensibilità. Per i panetti surgelati già formati, lo scongelamento lento in frigorifero (8–12h) restituisce un prodotto praticamente identico al fresco.</p>
        </div>

        <!-- TAGS -->
        <div class="article-tags">
          <span class="article-tag">Tecnica di macinazione</span>
          <span class="article-tag">Germe di grano</span>
          <span class="article-tag">Lievito madre</span>
          <span class="article-tag">Impasto pizza</span>
          <span class="article-tag">HoReCa</span>
        </div>
      </div>

      <!-- SIDEBAR -->
      <aside class="article-sidebar reveal">

        <!-- Articoli correlati -->
        <div class="sidebar-box">
          <div class="sidebar-box-title">Articoli Correlati</div>
          <a href="#" class="related-item">
            <span class="related-cat">Tecnica</span>
            <span class="related-title">LIEVITO MADRE FRESCO: COME CONSERVARLO</span>
          </a>
          <a href="#" class="related-item">
            <span class="related-cat">Prodotti</span>
            <span class="related-title">NUOVA LINEA PANETTI: LA PRESENTAZIONE</span>
          </a>
          <a href="#" class="related-item">
            <span class="related-cat">Community</span>
            <span class="related-title">PIZZAIOLI AMBASSADOR: LE LORO STORIE</span>
          </a>
        </div>

        <!-- Prodotti citati -->
        <div class="sidebar-box">
          <div class="sidebar-box-title">Prodotti Citati</div>
          <a href="farine.html" class="product-link">
            <span class="pl-name">Farine Molino Madre</span>
            <span class="pl-arrow">&#8594;</span>
          </a>
          <a href="panetti.html" class="product-link">
            <span class="pl-name">Panetti Surgelati</span>
            <span class="pl-arrow">&#8594;</span>
          </a>
          <a href="basi.html" class="product-link">
            <span class="pl-name">Basi per Pizza</span>
            <span class="pl-arrow">&#8594;</span>
          </a>
        </div>

        <!-- Newsletter -->
        <div class="sidebar-box sidebar-newsletter">
          <div class="sidebar-box-title">Newsletter</div>
          <p class="sn-text">Ricevi aggiornamenti tecnici, novità di prodotto e approfondimenti direttamente nella tua casella.</p>
          <input type="email" class="sn-input" placeholder="La tua email professionale">
          <button class="sn-btn">Iscriviti &#8594;</button>
        </div>

      </aside>
    </div>
  </div>
</section>

<!-- CTA -->
<section class="cta-section">
  <div class="cta-pattern"></div>
  <div class="container cta-inner">
    <p class="cta-label">Hai domande sulla farina giusta per il tuo impasto?</p>
    <h2 class="cta-title">PARLA CON<br>UN ESPERTO</h2>
    <p class="cta-text">Il team di Molino Madre è a disposizione per consigliarti la farina più adatta al tuo processo produttivo, con supporto tecnico dedicato.</p>
    <a href="consulenza.html" class="btn-cta">Richiedi Consulenza &#8594;</a>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="container">
    <div class="footer-top">
      <div>
        <div class="footer-brand-name">MOLINO MADRE</div>
        <div class="footer-brand-sub">Dal 1939 &middot; Calabria</div>
        <p class="footer-tagline">La farina artigianale per i professionisti dell&rsquo;impasto.</p>
      </div>
      <div class="footer-col">
        <h5>Prodotti</h5>
        <ul>
          <li><a href="farine.html">Farine</a></li>
          <li><a href="basi.html">Basi per Pizza</a></li>
          <li><a href="panetti.html">Panetti Surgelati</a></li>
          <li><a href="il-forno-del-mulino.html">Il Forno</a></li>
        </ul>
      </div>
      <div class="footer-col">
        <h5>Azienda</h5>
        <ul>
          <li><a href="il-molino.html">Il Mulino</a></li>
          <li><a href="news.html">News</a></li>
          <li><a href="consulenza.html">Consulenza</a></li>
          <li><a href="contatti.html">Contatti</a></li>
        </ul>
      </div>
      <div class="footer-col">
        <h5>Certificazioni</h5>
        <ul>
          <li><a href="#">IFS Food</a></li>
          <li><a href="#">BRC Food</a></li>
        </ul>
      </div>
    </div>
    <div class="footer-bottom">
      <span class="footer-copy">&copy; 2025 Molino Madre Srl &mdash; P.IVA 00000000000</span>
      <div class="footer-socials">
        <a href="#" class="footer-social-link">IG</a>
        <a href="#" class="footer-social-link">FB</a>
        <a href="#" class="footer-social-link">LI</a>
      </div>
    </div>
  </div>
</footer>

<!-- STICKY CONTACT -->
<div class="sticky-contact">
  <a href="https://wa.me/39000000000" class="sticky-btn sticky-wa" target="_blank" rel="noopener">
    <svg class="icon-svg" viewBox="0 0 24 24" fill="currentColor"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51a12.8 12.8 0 00-.57-.01c-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347z"/><path d="M12 0C5.373 0 0 5.373 0 12c0 2.104.546 4.084 1.5 5.8L0 24l6.337-1.46A11.943 11.943 0 0012 24c6.627 0 12-5.373 12-12S18.627 0 12 0zm0 21.818a9.811 9.811 0 01-5.003-1.368l-.36-.213-3.76.866.934-3.659-.236-.376A9.768 9.768 0 012.182 12C2.182 6.57 6.57 2.182 12 2.182S21.818 6.57 21.818 12 17.43 21.818 12 21.818z"/></svg>
    WhatsApp
  </a>
  <a href="contatti.html" class="sticky-btn sticky-main">
    <svg class="icon-svg" viewBox="0 0 24 24" fill="currentColor"><path d="M20 4H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/></svg>
    Contatti
  </a>
</div>

<script>
const revEls = document.querySelectorAll('.reveal');
const obs = new IntersectionObserver(entries => {
  entries.forEach(e => { if(e.isIntersecting){ e.target.classList.add('visible'); obs.unobserve(e.target); } });
},{threshold:0.08});
revEls.forEach(el => obs.observe(el));
const burger = document.getElementById('burger');
const mobileMenu = document.getElementById('mobileMenu');
const closeMenu = document.getElementById('closeMenu');
burger.addEventListener('click', () => mobileMenu.classList.add('open'));
closeMenu.addEventListener('click', () => mobileMenu.classList.remove('open'));
</script>
</body>
</html>
```

- [ ] **Step 2: Verifica visiva desktop**

Aprire `docs/news-articolo-template.html` nel browser a schermo pieno:
- Hero scuro con titolo, category label, meta bar ✓
- Immagine cover full-width ✓
- Layout a 2 colonne: articolo (sx) + sidebar (dx) ✓
- Quote block con bordo rosso a sinistra ✓
- Sidebar sticky con 3 box ✓
- CTA section rossa in fondo ✓

- [ ] **Step 3: Verifica visiva tablet (1024px)**

In DevTools a 1024px:
- Layout diventa 1 colonna ✓
- Sidebar va sotto il contenuto dell'articolo ✓
- Sidebar non è più sticky ✓

- [ ] **Step 4: Verifica visiva mobile (390px)**

In DevTools a 390px:
- Navbar: logo | CONTATTI | ☰ ✓
- Hero si ridimensiona correttamente ✓
- Testo articolo leggibile (16px body) ✓
- Hamburger funziona ✓

- [ ] **Step 5: Commit**

```bash
git add docs/news-articolo-template.html
git commit -m "Aggiunto template pagina articolo blog/news"
```

---

## Checklist finale

Dopo tutti i task, verificare:
- [ ] Aprire `docs/basi.html` a 900px: le card piccoli/grandi formati sono in 3 colonne bilanciate, nessuna card orfana
- [ ] Aprire tutte e 4 le pagine a 390px: hamburger visibile e funzionante
- [ ] Aprire `docs/news-articolo-template.html`: desktop e mobile corretti
- [ ] `git log --oneline -6` mostra 6 commit separati (uno per task)
