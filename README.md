# Token Radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/Khamami13/token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/Khamami13/token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A token launch radar, built for make.fun's **"Build the best token launch radar"** bounty.
It reads a sealed snapshot of every protocol DefiLlama tracks, ranks the 226 pre-token
protocols by what their launch would be worth, and shows a reader how to farm each one.

## The model (radar-2)

Implied value is derived from the snapshot's own figures, never from a hand-picked list
of names:

```
implied = round( tvl_usd x category_multiple
  x (1 + clamp(change_7d, -60, 120) / 100 x 0.5)
  x (0.6 + min(age_days, 1095) / 1095 x 0.4)
  x (1 + min(chains - 1, 9) x 0.01) )
```

- **category_multiple** — the market itself: the median market-cap/TVL ratio of the
  live-token protocols in the same category (at least 4 of them), else the all-category
  median. Lending trades at 0.13x TVL, Dexs at 0.34x, RWA at 0.27x.
- **growth** — the protocol's seven-day TVL change, clamped, worth half its weight.
- **maturity** — how long the protocol has run with no token; older counts for more.
- **breadth** — one point per extra chain, up to nine.

The board covers all 40 watchlist protocols (TVL of at least $50M, no token on record)
plus all 186 pre-token protocols below that floor. Live-token protocols are set aside
with their market caps shown, never mixed into the ranking.

## The dashboard

`dashboard/` is a static page with no network: a sortable, filterable board, a detail
view for every protocol (model breakdown, market comparables, farm plan), and a Live
token tab. `board.json` holds all 226 rows, each with an implied value and a six-step
farm plan with costs and times.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

Node 22.18 or later, standard library only. The program runs twice and writes the same
bytes both times: `board.json`, `data.js`, `dashboard/`, `screenshots/` and
`dossier.json` land in the output directory.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
