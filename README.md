# 🃏 Poker Night — Dashboard

Public standings page for our poker sessions. It reads our Google Sheet live and
renders the leaderboard, charts, and a per-session balance check.

**Live:** https://yordi23.github.io/poker-dashboard/

## What it shows

- **Leaderboard** — every player ranked by all-time net profit, with biggest
  winner / biggest loser highlight cards.
- **Net profit by player** — bar chart.
- **Cumulative standings over time** — line chart across each (date, session).
- **Session check** — each sitting's total vs. the expected `$975 × players`,
  flagged OK / OFF (catches counting mistakes).

Auto-refreshes every 60s. Fully responsive on mobile.

## How it works

It's a single self-contained [`index.html`](index.html) — no build step, no backend.
On load it fetches the responses tab as CSV from Google's public endpoint:

```
https://docs.google.com/spreadsheets/d/<SPREADSHEET_ID>/gviz/tq?tqx=out:csv&sheet=Form%20Responses%201
```

…parses it, and computes the leaderboard, sessions, and charts in the browser with
[Chart.js](https://www.chartjs.org/) (loaded from a CDN). The Google Sheet is shared
*Anyone with the link → Viewer*, which is what makes the read work.

## Configuration

Two constants at the top of [`index.html`](index.html):

```js
const SPREADSHEET_ID = "1sdli0MIdzyLiwStvG1soNHKoFrf4xspQl_MUw6_8BJs";
const BUY_IN = 975;   // must match the Config tab in the sheet
```

The spreadsheet ID is a **view-only locator**, not a secret — it can't edit
anything. The trade-off of a public dashboard is that the poker numbers (names +
amounts) are publicly viewable by anyone with the URL.

## Hosting (GitHub Pages)

Served from `main` / root. To update: edit `index.html`, commit, push — Pages
rebuilds in about a minute.

```
Settings → Pages → Build and deployment → Source: Deploy from a branch → main /(root)
```

## Where the data comes from

The Form, Sheet, and validation are built by the Apps Script in the separate
**Poker** repo. Players submit a Google Form after each session; this dashboard
just visualizes what lands in the sheet.
