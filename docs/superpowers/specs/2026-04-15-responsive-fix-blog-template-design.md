# Design Spec: Responsive Fix + Blog Article Template
**Date:** 2026-04-15  
**Scope:** basi.html, panetti.html, contatti.html, news.html + nuovo template articolo

---

## 1. Contesto

Il sito Molino Madre è statico (HTML/CSS inline per pagina, nessun build system). Tutte le pagine condividono lo stesso design system (variabili CSS, font, pattern UI). Le quattro pagine da correggere hanno problemi responsive e mancano di un menu mobile funzionante.

---

## 2. Problemi da risolvere

### 2.1 Menu mobile — tutte e 4 le pagine

**Problema attuale:** A `≤768px` la `.nav-links` viene nascosta (`display: none`) senza nessun sostituto. Il pulsante "Contatti" rimane ma non c'è modo di navigare il sito da mobile.

**Soluzione:**
- Su `≤768px` la navbar mostra: **logo** (sinistra) · **pulsante Contatti** (centro) · **hamburger icon** (destra)
- L'hamburger (3 linee → X animato) apre un overlay fullscreen con sfondo `var(--nero)` semitrasparente
- I link di navigazione nel menu sono in Bebas Neue, grandi (clamp 32px–48px), con hover in `var(--terracotta)`
- Click su un link o sulla X chiude il menu
- Il menu overlay ha `z-index: 1100` (sopra la navbar a 1000)

**CSS da aggiungere (stessa struttura in tutte le pagine):**
```css
.nav-hamburger { display: none; flex-direction: column; gap: 5px; cursor: pointer; padding: 8px; background: none; border: none; }
.nav-hamburger span { display: block; width: 22px; height: 2px; background: currentColor; transition: all .3s; }
.mobile-menu { display: none; position: fixed; inset: 0; background: rgba(26,20,16,.97); z-index: 1100;
  flex-direction: column; align-items: center; justify-content: center; gap: 32px; }
.mobile-menu.open { display: flex; }
.mobile-menu a { font-family: var(--font-d); font-size: clamp(32px,6vw,48px); letter-spacing: .06em;
  color: #fff; text-decoration: none; transition: color .3s; }
.mobile-menu a:hover { color: var(--terracotta); }
.mobile-menu-close { position: absolute; top: 24px; right: 24px; font-size: 28px; color: rgba(255,255,255,.5);
  cursor: pointer; background: none; border: none; font-family: var(--font-b); }
@media (max-width: 768px) {
  /* Griglia a 3 zone: logo | Contatti | hamburger
     La nav rimane position:fixed, non usiamo absolute per non romperla */
  nav { display: grid; grid-template-columns: 1fr auto 1fr; padding: 0 20px; }
  .nav-logo { grid-column: 1; }
  .nav-links { display: none; }
  .nav-cta { grid-column: 2; justify-self: center; }
  .nav-hamburger { display: flex; grid-column: 3; justify-self: end; align-self: center; color: currentColor; }
}
```

**JS da aggiungere (inline, alla fine del body):**
```js
const burger = document.getElementById('burger');
const mobileMenu = document.getElementById('mobileMenu');
const closeMenu = document.getElementById('closeMenu');
burger.addEventListener('click', () => mobileMenu.classList.add('open'));
closeMenu.addEventListener('click', () => mobileMenu.classList.remove('open'));
```

**HTML da aggiungere:**
- Hamburger button `id="burger"` nella `<nav>` dopo `.nav-cta`
- Div `id="mobileMenu"` con classe `.mobile-menu` subito dopo `<nav>`, contenente tutti i link + close button

---

### 2.2 basi.html — Piccoli Formati / Formati Grandi

**Problema attuale:**
- A `1024px`: `products-row--3` → `repeat(2, 1fr)` crea un orphan (3ª card mezza larghezza nella seconda riga)
- Le card `basi-card` con `aspect-ratio: 3/4` diventano ~480×640px a tablet — troppo alte

**Soluzione:**

| Breakpoint | Colonne | Aspect-ratio card |
|---|---|---|
| Desktop `>860px` | 3 | `3/4` (originale) |
| Tablet `768–860px` | 3 | `4/5` |
| Mobile `≤768px` | 1 | `3/4` (originale) |

Modifiche al media query `@media (max-width: 1024px)`:
- Rimuovere la regola `products-row--3 { grid-template-columns: repeat(2, 1fr); }`

Aggiungere nuovo breakpoint `@media (max-width: 860px)`:
```css
@media (max-width: 860px) {
  .products-row--3 { grid-template-columns: repeat(3, 1fr); }
  .basi-card { aspect-ratio: 4/5; }
  .bc-name { font-size: 24px; }
  .bc-desc { display: none; } /* nasconde la descrizione per non sovraffollare le card strette */
}
```

A mobile (`≤768px`) le card tornano a colonna singola con l'aspect-ratio originale e la descrizione di nuovo visibile.

---

### 2.3 panetti.html

Nessuna modifica strutturale al layout. Solo hamburger menu (vedi §2.1).

---

### 2.4 contatti.html

Nessuna modifica strutturale al layout. Solo hamburger menu (vedi §2.1). 
Nota: la nav ha logica transparent→scrolled gestita via JS già presente; il menu mobile deve rispettare questo stato (usare `color: inherit` per l'hamburger icon).

---

### 2.5 news.html

Nessuna modifica strutturale al layout (articoli 3→2→1 già corretti). Solo hamburger menu (vedi §2.1).

---

## 3. Template articolo blog

**File:** `docs/news-articolo-template.html`

### Struttura della pagina

```
[NAV] (identica alle altre pagine, con hamburger)
[ARTICLE HERO] — dark bg, categoria + data, titolo H1 grande, autore + tempo lettura
[ARTICLE BODY] — 2-column layout: corpo (2fr) + sidebar (1fr)
  Corpo:
    - Lead paragraph (Cormorant 20px, colore scuro, prima intro)
    - Paragrafi corpo (DM Sans 16px, line-height 1.8)
    - Quote block (bordo sinistro 3px rosso, testo Cormorant italic 20px)
    - Immagine inline placeholder (aspect-ratio 16/9, bg scuro con label)
    - Paragrafi continua
  Sidebar:
    - Box "Articoli correlati" (3 link con categoria+titolo)
    - Box "Prodotti citati" (link a pagina prodotto)
    - Box newsletter signup (email input + submit)
[CTA SECTION] — rosso, testo "Hai domande sul prodotto?" → link consulenza
[FOOTER] (identico alle altre pagine)
```

### Responsività
- Desktop `>1024px`: 2 colonne (corpo + sidebar)
- Tablet/Mobile `≤1024px`: colonna singola, sidebar va sotto il corpo

### Stile articolo
- Max-width corpo: 680px centrato nella sua colonna
- Font: Cormorant Garamond 18px/line-height 1.85 per i paragrafi
- H2 articolo: Bebas Neue 36px, rosso
- H3 articolo: Bebas Neue 24px, terracotta
- Quote block: `border-left: 3px solid var(--rosso); padding: 20px 28px; background: rgba(138,36,50,.04);`
- Categorie: stesso `.label` style del sito (11px uppercase terracotta)

---

## 4. File da modificare / creare

| File | Tipo modifica |
|---|---|
| `docs/basi.html` | CSS responsive + hamburger menu |
| `docs/panetti.html` | Hamburger menu |
| `docs/contatti.html` | Hamburger menu |
| `docs/news.html` | Hamburger menu |
| `docs/news-articolo-template.html` | Nuovo file |

---

## 5. Cosa NON cambia

- Design system (variabili CSS, font, colori)
- Struttura HTML delle sezioni esistenti (solo aggiunta hamburger HTML)
- Contenuti e copy
- Footer (identico in tutte le pagine)
- Sticky contact buttons
