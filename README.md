# token-radar-azykrie

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/azykrie/token-radar-azykrie/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/azykrie/token-radar-azykrie/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token Radar (model radar-2): a pre-token protocol board, ranked by implied value. It reads a sealed
snapshot of every protocol DefiLlama tracks, works out which ones still record no token and are not a
wrapped or staked product of one that trades, ranks them by what their launch would be worth, and hands
in a web dashboard with the board, a detail view for each protocol and the steps to farm it.

## What it scores

- **758 protocols** in the sealed snapshot (2026-11-14), **40** on the watchlist, **342** pre-token.
- The board ranks **198** protocols the tokenless test passes: all 40 watchlist rows plus 158 early
  rows the list leaves out. Live-token protocols (24) are set aside with their market caps shown.
- **Implied value** (radar-2, from the snapshot's own figures only, no name ever special-cased):
  `implied = round(tvl x 0.30 x growthMult x maturityMult x breadthMult)`, where growth is the 7-day
  change clamped to [-80, +150] with declines weighing 1.5x gains, maturity is `0.55 + 0.45 x
  (1 - exp(-age / 540))`, and breadth is `1 + 0.03 x min(chains - 1, 5)`.
- Every row carries an **8-step farm plan** derived from its own record (category, chains, TVL, age).

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json|directory>
```

`main.ts` (Node 22, standard library only) writes `board.json` (198 rows), `data.js`, copies
`dashboard/` and `screenshots/` to the output, and writes `dossier.json` with the vendored
`vendor/makefun-board.ts`. The dashboard is a static page that renders with no network.

## Layout

- `main.ts` — the entrypoint the grader runs.
- `lib/model.ts` — the radar-2 model: snapshot parsing, the tokenless test, implied value, farm plans.
- `dashboard/` — the board: search, category and scope filters, sortable table, per-protocol detail
  dialog with the implied-value breakdown and farm plan, and the model panel.
- `screenshots/` — the board, a protocol's detail view, and a filtered view (1440x900).
- `vendor/makefun-board.ts` — make.fun's dossier library, copied unchanged from the kit.

---

<sub>Entered on [make.fun](https://make.fun/?ref=azykrie&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
