# token-launch-radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/clmthy/token-launch-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/clmthy/token-launch-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A token launch radar: it reads a sealed DefiLlama snapshot of every protocol, finds the ones that record no token yet, ranks them by an implied value, and shows a reader how to farm each one — with an early radar that finds future airdrops before they exist.

## What it does

- **Board:** 178 pre-token protocols ranked by implied value — the 40-protocol watchlist (every protocol with ≥ $50M TVL and no token on record) plus 138 early protocols below that floor, held to the watchlist's own exclusion rule (no Bridge, Canonical Bridge, Liquid Staking, Liquid Restaking, Restaked BTC or CEX, no versioned names, no wrapped / staked / staking products, no exchange products).
- **Breakout signal:** young protocols already growing fast are flagged and ranked up — the radar's answer to "find future airdrops early". Nine breakout rows on this snapshot, led by a protocol up 4,448% in seven days at 45 days old.
- **Farm plans:** eight concrete steps per protocol, each with a cost and a time, derived from the protocol's own record (category, chains, TVL, age) — category-specific actions, capital sized from TVL, hold windows from age, a points/rewards check and a safety note.
- **Dashboard:** a static dark page, no framework and no network. Board view with search, category and chain filters and All / Watchlist / Early / Breakout scopes; a detail view per protocol with the implied-value breakdown and the full farm plan; a live-token view that keeps the protocols whose token already trades apart with their market caps; the model stated in full on the page.

## The model (radar-2)

```
implied = round( tvl x 0.25
  x (1 + 0.8 x clamp(change_7d, -50, 200) / 100)
  x breakout          (1 + 0.5 x min(change_7d, 200) / 200, only when age < 365d and change_7d > 10%, else 1)
  x (0.6 + min(age, 1095) / 1095 x 0.4)
  x (1 + 0.2 x tanh(log2(tvl / category median TVL) / 2))
  x (1 + 0.04 x log2(1 + chains)) )
```

Every factor follows from the snapshot's own figures; no protocol name is special-cased and no row is hand-picked. The page recomputes each row from those figures and checks the number `board.json` carries. Implied value is this entry's own model claim, never a price and never a prediction that a token is issued.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ../input/snapshot.json
```

Node 22, standard library only, no install. Writes `out/board.json`, `out/data.js`, `out/dashboard/`, `out/screenshots/` and `out/dossier.json`. Then open `out/dashboard/index.html` in a browser.

## Layout

- `main.ts` — reads the snapshot, scores the board, writes the outputs and the dossier
- `lib/model.ts` — the radar-2 model, the farm plans and the snapshot parser
- `vendor/makefun-board.ts` — make.fun's dossier library, byte for byte as the kit ships it
- `dashboard/` — the static page (`index.html`, `app.css`, `app.js`)
- `screenshots/` — the board, a protocol detail view and a filtered (breakout) view

---

<sub>Entered on [make.fun](https://make.fun/?ref=bxw2msg9&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards. superbot.gg is a free uncensored model that can help run this radar.</sub>
