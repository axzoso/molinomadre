# Design Spec: Template Pagina Prodotto Singolo — Basi

**Data:** 2026-04-28  
**File di output:** `docs/basi-prodotto-template.html`  
**Stato:** Approvato dall'utente

---

## Obiettivo

Creare un template HTML statico per le pagine dei singoli prodotti della linea **Basi per Pizza** di Molino Madre. Il layout è ispirato alle schede prodotto e-commerce, integrato nel design system esistente del sito. Deve essere semplice da replicare in Elementor.

---

## Design System di Riferimento

Stesso CSS tokens, font e componenti già presenti in `docs/basi.html`:

- **Colori:** `--rosso` #8a2432 · `--terracotta` #a9483d · `--grano` #ddc9a2 · `--bianco` #f5f2ec · `--nero` #1a1410
- **Font:** Bebas Neue (display) · Cormorant Garamond (serif/body lungo) · DM Sans (body/UI)
- **Componenti riusati:** nav, footer, sticky-contact, breadcrumb, `.label`, `.divider`, `.btn`, `.reveal`, hamburger mobile

---

## Struttura della Pagina

### 1. Nav + Breadcrumb
Identica agli altri file. Breadcrumb: `Home → Basi → [Nome Prodotto]`

### 2. Zona Hero — Split 2 colonne (above the fold)

**Layout desktop:** griglia `5fr 7fr`, gap 60–80px, padding-top 120px (sotto nav fissa).

**Colonna sinistra (info):**
- `.label` — categoria (es. "Piccoli Formati · Al piatto")
- `.divider` terracotta
- `<h1>` in Bebas Neue, `clamp(48px, 6vw, 88px)`, `--rosso`
- Sottotitolo breve: Cormorant Garamond italic, 17–18px, colore #666, max 3 righe
- Tags: `.bc-tag` (riutilizzati da basi.html)
- `<hr>` sottile terracotta (opacity .15)
- **Griglia info rapide:** 4 righe (Formato / Peso / Cottura / Scadenza). Label a sinistra 11px uppercase grigio, valore a destra 13px scuro.
- CTA `.btn`: "Richiedi Scheda Tecnica →"

**Colonna destra (galleria):**
- Immagine principale: aspect-ratio 4/3, border-radius 2px, `object-fit: cover`
- Sotto: riga di 3 thumbnail quadrate (1:1), cliccabili via JS per swap immagine principale
- Thumbnail attiva: border 2px `--terracotta`

### 3. Zona Descrizione Lunga

Sfondo `--bianco`, padding `80px 0`.
- `.label` + `.divider` + titolo sezione h2 in Bebas Neue (es. "IL PRODOTTO")
- Corpo testo in Cormorant Garamond, 17px, max-width 720px
- `.reveal` animation

### 4. Zona Tabella Tecnica

Sfondo bianco puro `#fff`, padding `60px 0`.
- `.label` + `.divider` + h2 "SCHEDA TECNICA"
- Tabella a 3 colonne: **Parametro / Valore / Note**
  - Header row: background `--rosso`, testo bianco, font DM Sans 11px uppercase
  - Righe alternate: `--bianco` / `#fff`
  - Colonna parametro: grassetto 12px uppercase
  - Colonne valore/note: 13px, colore #555
- Max-width 800px

### 5. CTA Block

Blocco `--rosso` riusato da basi.html: "Hai bisogno di un partner?" con link a consulenza.html.

### 6. Footer + Sticky Contact

Identici agli altri file del sito.

---

## Responsive

### Tablet (max-width: 1024px)
- Hero: griglia `1fr 1fr` (colonne più strette)
- Thumbnail: 3 visibili

### Tablet piccolo / landscape mobile (max-width: 768px)
- Hero: colonna unica — **immagine prima**, poi info sotto
- Titolo h1: `clamp(44px, 8vw, 72px)`
- Info rapide: layout a 2 colonne (2 righe × 2 colonne)
- Thumbnail: 3 visibili, più piccole

### Mobile (max-width: 480px)
- Container padding: 0 20px
- Thumbnail: 2 visibili (terza nascosta o rimpicciolita)
- Info rapide: lista verticale
- Tabella: scroll orizzontale (`overflow-x: auto`)

---

## JavaScript

Solo un piccolo script inline (~15 righe):
1. **Thumbnail swap:** click su thumbnail → aggiorna `src` immagine principale + toggle classe `.active`
2. **Reveal on scroll:** `IntersectionObserver` (già pattern standard del sito)
3. **Hamburger menu:** identico agli altri file

---

## Note Elementor

Ogni zona = una **Section** Elementor:
- Hero split: Section con 2 colonne, widget Image + widget Text Editor/HTML
- Descrizione: Section colonna unica, widget Text Editor
- Tabella: Section colonna unica, widget HTML (tabella raw) o Table widget
- CTA: Section colonna unica, widget HTML

Nessun widget custom o shortcode complesso necessario.

---

## File da creare

| File | Descrizione |
|------|-------------|
| `docs/basi-prodotto-template.html` | Template completo con dati di esempio (Pizza Tonda) |

Nessun file esistente viene modificato.
