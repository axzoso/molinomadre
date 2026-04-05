# Molino Madre — Sito Web

Demo del nuovo sito istituzionale per **Molino Madre**, storico molino calabrese fondato nel 1939. Il progetto comprende il design system, il prototipo HTML/CSS di tutte le pagine e la documentazione dei contenuti.

---

## Panoramica del progetto

Molino Madre è il **primo molino italiano produttore di basi per pizza artigianali** e offre tre linee di prodotto destinate esclusivamente al mercato B2B (pizzaioli, ristoratori, catene Ho.Re.Ca., buyer GDO):

- **Farine professionali** — macinazione lenta in 24 passaggi con preservazione del germe di grano
- **Basi per pizza** — precotte nel forno a legna, lievitazione 48h con lievito madre, surgelate senza additivi
- **Panetti surgelati** — impasti tecnici con pasta madre fresca, grammature personalizzabili 180–600 g

---

## Struttura del repository

```
molinomadre/
│
├── docs/                            # ★ Sito deployato su GitHub Pages
│   ├── index.html                   # Homepage
│   ├── il-molino.html               # Storia e valori aziendali
│   ├── farine.html                  # Catalogo farine
│   ├── basi.html                    # Basi per pizza
│   ├── panetti.html                 # Panetti surgelati
│   ├── il-forno-del-mulino.html     # Hub professionale B2B
│   ├── consulenza.html              # Servizi di consulenza + form
│   ├── news.html                    # News e articoli
│   ├── contatti.html                # Contatti + form
│   └── images/                      # Tutte le immagini del sito
│
├── Demo sito/                       # File di lavoro originali (archivio)
│   └── html/                        # Versioni precedenti con struttura a sottocartelle
│
├── Logo/                            # Loghi e brand guidelines (PDF)
├── Materiali ricevuti/              # Asset forniti dal cliente
│   ├── Foto/                        # Fotografie (shooting, ambassador, storiche)
│   ├── Sezione PRODOTTI_basi/       # Copy e foto basi
│   ├── Sezione PRODOTTI_panetti/    # Copy e foto panetti
│   └── Site map e testi/            # Brief, alberatura, testi approvati
│
├── sitemap.md                       # Alberatura completa del sito
├── sezioni-pagine.md                # Sezioni interne di ogni pagina
├── catalogo-prodotti.md             # Tutti i prodotti con schede descrittive
├── CLAUDE.md                        # Guida per AI (Claude Code)
└── README.md                        # Questo file
```

---

## Pagine del sito

Tutte le pagine si trovano in `docs/` — struttura piatta, nessuna sottocartella per pagina.

| Pagina | File | Stato |
|--------|------|-------|
| Homepage | `docs/index.html` | ✅ Completata |
| Il Molino | `docs/il-molino.html` | ✅ Completata |
| Farine | `docs/farine.html` | ✅ Completata |
| Basi per Pizza | `docs/basi.html` | ✅ Completata |
| Panetti Surgelati | `docs/panetti.html` | ✅ Completata |
| Il Forno del Mulino | `docs/il-forno-del-mulino.html` | ✅ Completata |
| Consulenza | `docs/consulenza.html` | ✅ Completata |
| News | `docs/news.html` | ✅ Completata |
| Contatti | `docs/contatti.html` | ✅ Completata |

---

## Design system

Il prototipo è **statico, senza dipendenze esterne** (nessun framework, nessun bundler). Ogni pagina è un file HTML autonomo con CSS inline.

### Palette colori

| Token | Valore | Uso |
|-------|--------|-----|
| `--rosso` | `#8a2432` | Colore primario, titoli, CTA |
| `--terracotta` | `#a9483d` | Accenti, label, hover |
| `--grano` | `#ddc9a2` | Testo su sfondo scuro, dettagli warm |
| `--lavanda` | `#e3e0e5` | Sfondi sezioni secondarie |
| `--oliva` | `#888f64` | Accenti naturali |
| `--bianco` | `#f5f2ec` | Background principale |
| `--nero` | `#1a1410` | Sfondi dark, testo |

### Tipografia

| Ruolo | Font | Uso |
|-------|------|-----|
| Display | **Bebas Neue** | Titoli di sezione, numeri grandi, nav logo |
| Serif | **Cormorant Garamond** | Testi editoriali, citazioni, descrizioni prodotto |
| Body | **DM Sans** | Corpo testo, UI, form, label |

Fonte: Google Fonts (caricamento preconnect in ogni pagina).

### Componenti ricorrenti

- **Nav** — fixed, trasparente su hero scuro, diventa opaca su sfondo chiaro con `.scrolled`
- **Breadcrumb** — navigazione contestuale sotto la nav
- **Page Hero** — layout a due colonne con titolo in Bebas Neue, stats bar, visual panel
- **Ticker** — striscia animata con claim di brand (`@keyframes marquee`)
- **Category strip** — separatore `--grano` tra gruppi di prodotti
- **Reveal** — `IntersectionObserver` che aggiunge `.visible` all'entrata in viewport
- **Footer** — griglia 4 colonne su sfondo `--nero`, uniforme su tutte le pagine
- **Sticky bar** — bottoni WhatsApp + Contatti fissi in basso a destra su mobile e desktop

---

## Come visualizzare il sito

Il sito è composto da file HTML statici in `docs/`.

**Apertura locale:** apri `docs/index.html` nel browser o usa Live Server puntando alla cartella `docs/`.

**GitHub Pages:** attiva nelle impostazioni del repo → Pages → Source: `main` branch, cartella `/docs`. Il sito sarà disponibile all'URL fornito da GitHub.

Tutti i link interni sono già relativi e funzionano sia in locale che su GitHub Pages senza configurazioni aggiuntive.

---

## Documentazione di progetto

- [`sitemap.md`](sitemap.md) — alberatura completa del sito con note strategiche B2B
- [`sezioni-pagine.md`](sezioni-pagine.md) — mappa dettagliata di ogni sezione per pagina, con ID/anchor HTML
- [`catalogo-prodotti.md`](catalogo-prodotti.md) — schede complete di tutti i prodotti (farine, basi, panetti) con usi, lievitazioni, istruzioni

---

## Brand e posizionamento

- **Fondazione:** 1939, Calabria — tre generazioni di famiglia (nonno Amodio → Amodio e Vittoria)
- **Posizionamento:** *"Il Forno del Mulino"* — unico molino italiano che affianca il ristoratore dall'impasto alla tavola
- **Target:** esclusivamente B2B — farine a pizzaioli/panifici, basi e panetti a catene di distribuzione anche estero
- **Certificazioni:** IFS Food, BRC Food
- **Elemento distintivo:** lievito madre originale conservato dal 1939, macinazione lenta in 24 passaggi con preservazione del germe di grano
