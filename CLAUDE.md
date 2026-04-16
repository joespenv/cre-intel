# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture

Single-file React app — everything lives in `index.html`. No build step, no bundler.

- **React 18** loaded via CDN (`unpkg`), compiled in-browser with `@babel/standalone`
- **All data** is hardcoded as JS constants at the top of the `<script type="text/babel">` block
- **All styles** are inline JS objects in the `S` constant, plus a minimal global `<style>` block in `<head>`
- **Tabs** are driven by the `TABS` array — each entry maps an `id` to a label and a component function

## Data constants

| Constant | Contents |
|----------|----------|
| `LEIE` | Rental prices per Oslo district (kr/kvm/år + vacancy) |
| `SENTIMENT` | Market signal per district (good/warn/bad/info) |
| `YIELD` | Prime and secondary yields per segment |
| `ENTRA` | Entra consensus report history (Q4 2023–Q2 2025) |
| `RENTE` | Interest rates — historical, actual, forecast |
| `BYGG` | SSB construction cost index (2019=100) |
| `BORS` | Listed CRE companies on Oslo Børs |
| `TRANS` | Transaction volumes Norway 2021–2025E |
| `KPI` | 6 headline KPIs shown in the top bar |

## Updating data

Edit the relevant constant directly in `index.html`. No imports, no separate data files — everything is inline. After editing:

```bash
git add index.html
git commit -m "Oppdater [LEIE/YIELD/RENTE/etc] per [kilde] [dato]"
git push origin main
```

Vercel auto-deploys on push once GitHub is connected at vercel.com/greencap/cre-intel-app/settings/git. Until then, deploy manually:

```bash
npx vercel --prod --yes --scope greencap
```

## Adding a new tab

1. Create a `function TabNavn() { return (...) }` component
2. Add `{ id:'navn', label:'Visningsnavn', comp: TabNavn }` to the `TABS` array

## Badge types

`good` (green) · `warn` (orange) · `bad` (red) · `info` (blue) · `neutral` (grey)

Used in `<Badge value="..." type="..." />` and `<Delta v={number} />` (auto-colors ±0.5pp threshold).

## Deployment

- **Live:** https://cre-intel-app.vercel.app
- **GitHub:** https://github.com/joespenv/cre-intel
- **Vercel project:** greencap/cre-intel-app
- **Scope:** always pass `--scope greencap` to Vercel CLI
