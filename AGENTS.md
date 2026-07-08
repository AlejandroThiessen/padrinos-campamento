# Sponsor Transparency Board — Agent Instructions

This repo is a **public fundraising transparency ledger** for a youth camp tournament.
Goal: **$30,000 MXN**. Live site (GitHub Pages, deploys ~1 min after push to `main`):

**https://alejandrothiessen.github.io/padrinos-campamento/**

Everything is one file: `index.html`. The only thing that normally changes is the
data at the top of its `<script>` block. Do **not** redesign or restructure the page
when asked to record a donation or expense.

## Recording a donation

The organizer will say something like *"Ferretería López gave 2,500, name public"*
or *"someone gave 800, anonymous"*. Add one object to the `PATROCINADORES` array:

```js
{ folio: "F-003", fecha: "2026-07-21", nombre: "Ferretería López", publico: true, monto: 2500 },
```

If the organizer provides a sponsor logo, save the image under `assets/logos/`
using a lowercase/kebab-case filename, then add a `logo` field:

```js
{ folio: "F-003", fecha: "2026-07-21", nombre: "Ferretería López", publico: true, monto: 2500, logo: "assets/logos/ferreteria-lopez.png" },
```

Rules:
- **folio**: sequential receipt number — next unused `F-###` (F-001, F-002, …).
  Tell the organizer the folio so they can write it on the sponsor's paper receipt.
- **fecha**: date received, `YYYY-MM-DD`. Use today unless told otherwise.
- **nombre**: the business name exactly as given. For **anonymous** sponsors, invent
  a code name instead: an uppercase Spanish word + 2-digit number (e.g. `ÁGUILA-07`,
  `NOPAL-03`), unique within the ledger. Tell the organizer the code — it goes on the
  sponsor's paper receipt so they can verify their own row later.
- **publico**: `true` if the business wants its name shown, `false` for anonymous.
- **monto**: whole pesos (number, no quotes, no commas).
- **logo**: optional path to a public sponsor logo image, only for public sponsors.
  Keep files in `assets/logos/`; prefer PNG/WebP/JPG; do not use logos for anonymous sponsors.
  The wall shows the logo on a white plaque up to ~88px tall, so ask for an image at
  least ~400px wide; transparent or white background looks best. Crop away excess
  whitespace around the mark before committing.
- Donations of **$1,000+ with publico:true** automatically get the ★ camp-board badge;
  the page handles this — nothing extra to do.

## Recording an expense

Add to the `GASTOS` array: `{ fecha: "2026-08-01", concepto: "Premios — ronda final", monto: 4000 },`

## Integrity rules (this is a trust ledger)

- **Never** remove or alter existing entries unless the organizer explicitly confirms
  a correction — then note the correction in the commit message.
- Never hardcode totals anywhere; the page computes all sums from the arrays.
- Do not touch `DEMO_PATROCINADORES` / `DEMO_GASTOS` (watermarked sample data for
  pitching) or `CONFIG` unless asked.
- The page is bilingual (ES default / EN toggle); if you add UI text, add it to both
  dictionaries in `STR`.

## Publishing

Two different computers maintain this ledger — **always `git pull` before editing.**

```
git pull
git add index.html
git commit -m "Donativo F-003: Ferretería López $2,500"
git push
```

Optionally verify after ~1 minute: `curl -s -o /dev/null -w "%{http_code}" https://alejandrothiessen.github.io/padrinos-campamento/` → `200`.
Confirm to the organizer: folio, name/code shown, new total raised.
