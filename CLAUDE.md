# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **static HTML/CSS website demo** for **Molino Madre**, a Calabrian flour mill founded in 1939 by the Amodio family. The company produces professional flours, pre-cooked pizza bases ("basi"), and frozen dough balls ("panetti") for the food service / HoReCa sector. The site is in Italian.

There is no build system, no package manager, no server framework — open the HTML files directly in a browser.

## Site Structure

Pages live under `Demo sito/html/`:
- `Home.html` — homepage with hero slider, product overview, certifications, consulting form
- `il molino/il-molino.html` — company history, milling process, brand values
- `farine/farine-def.html` — flour product catalogue (multiple variants exist; `farine-def.html` is the current reference)
- Product pages for basi and panetti are referenced in content docs but not yet in HTML form

Supporting assets:
- `Materiali ricevuti/` — all client-supplied content: photos (organized by subject), text documents, site map, brief
- `Logo/` — brand logo files and guidelines

## Design System

Every HTML page embeds the same CSS design tokens (no external stylesheet yet — CSS is inline `<style>` per file):

```css
--rosso:      #8a2432   /* primary brand red */
--terracotta: #a9483d   /* secondary warm red */
--grano:      #ddc9a2   /* wheat / warm gold */
--lavanda:    #e3e0e5   /* light lavender */
--oliva:      #888f64   /* olive green */
--bianco:     #f5f2ec   /* off-white background */
--nero:       #1a1410   /* near-black text */
```

Typography (Google Fonts, loaded in every page):
- `Bebas Neue` — display/headings (`--font-display` / `--font-d`)
- `Cormorant Garamond` — serif accent (`--font-serif` / `--font-s`)
- `DM Sans` — body text (`--font-body` / `--font-b`)

Recurring UI patterns shared across pages:
- `.label` — 11px uppercase tracked terracotta label
- `.section-title` — `clamp(44px,6vw,88px)` Bebas Neue in rosso
- `.divider` — 48px × 1px terracotta horizontal rule
- `.btn` / `.btn-light` — text link with animated gap arrow
- `nav` — fixed navbar that gains `.scrolled` class on scroll (transparent → bianco background)
- `.container` — `max-width: 1280px`, `padding: 0 40px`

## Key Content Facts

- Founded 1939 by "nonno Amodio e nonna Angelina"; today run by siblings **Amodio** and **Vittoria**
- Unique technique: slow milling in 24 passes preserving the wheat germ
- Products ship frozen (12-month shelf life); bases are hand-stretched and pre-cooked in a wood-fired stone-based oven
- Certifications: **IFS Food** and **BRC Food**
- Target audience: professional chefs, pizzaioli, bakers, pastry chefs, food service chains

## Content Source Documents

All authoritative copy lives in `Materiali ricevuti/Site map e testi/`:
- `Testi per home molino madre V3 23.O1 (1).txt` — full copy for all site sections (homepage, products, consulting)
- `Brief Sito Molino Madre 3 per Lucio.docx (1).pdf` — design brief
- `Farine.pdf` — flour product sheets
- `appunti importanti.pdf` — important client notes
- `Alberatura sito Molino Madre v2 per Gabriele.png` — site map / information architecture
