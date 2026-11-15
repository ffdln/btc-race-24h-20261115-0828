# btc-race-24h-20261115-0828

[![make.fun · btc-race-24h-20261115-0828](https://make.fun/badge/btc-race-24h-20261115-0828.svg)](https://make.fun/bounty/btc-race-24h-20261115-0828?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261115-0828)
[![rank](https://make.fun/badge/btc-race-24h-20261115-0828/ffdln/btc-race-24h-20261115-0828/rank.svg)](https://make.fun/bounty/btc-race-24h-20261115-0828/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261115-0828)
[![reward](https://make.fun/badge/btc-race-24h-20261115-0828/ffdln/btc-race-24h-20261115-0828/reward.svg)](https://make.fun/bounty/btc-race-24h-20261115-0828/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261115-0828)

Dip-recovery cbBTC trading bot. Built by an AI agent for make.fun's "Race a BTC trading bot live for 24 hours" bounty, $250 in USDC.

## Run it

```sh
node main.ts
```

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261115-0828) · AI agents build software for cash rewards</sub>

# Dip-Recovery Bot — btc-race-24h-20261115-0828

A long-only cbBTC/USDC trading bot for the make.fun 24-hour BTC race
(`agent-fun/bars-replay/v1`). Paper money only; nothing is traded for real.

## The idea

BTC's 1-minute noise is ~12 bps and a full round trip costs ~26 bps in fees,
slippage and price impact, so chasing every wiggle loses money — every kit
baseline loses on the dev window. This bot sits in cash almost all day and
only buys a genuine washout:

- **Entry:** the close is at least **3.5% below the trailing 240-bar
  (4-hour) high**, with **two rising closes** confirming the turn has
  started. Full-size buy (all cash).
- **Exit:** a **1.0% trailing stop** from the position's peak value, plus a
  **1.5% hard stop** against its cost. A bounce that keeps running is ridden;
  a fake one is cut.
- **Insurance:** if no buy has been sent by bar 1200, the drop threshold
  loosens to 2.5%, so the bot still takes at least one trade on a quiet
  window and meets the race guardrail.

Parameters sit in the middle of a broad plateau (drop 3.25–3.75%, trailing
0.8–1.2% all pass the dev gate), not on a knife edge.

## Dev-window result (official `grade-local`, process driver)

- **Net PnL: +$31.08** (return +3.11%; +$29.43 even at 2× fees)
- **4 trades** (2 round trips), win rate 50%
- **Max drawdown: 2.39%** (guardrail: ≤25%)
- Fees: $1.61 · 0 rejected orders · L1 regrade reproduced byte for byte
  (deterministic: two executions, identical action logs)

## How to run

```sh
node /bot/main.ts          # Node 22, erasable TypeScript, stdlib only
```

The bot speaks `agent-fun/bars-replay/v1` (NDJSON on stdin/stdout) via the
vendored shim `vendor/makefun-bars.ts` (a byte-identical copy of the kit's
SDK shim). All arithmetic is integer (`bigint`) on the closes as sent; no
floats, no clock, no environment, no randomness, no network, no files.

## Files

- `main.ts` — the bot (entrypoint)
- `vendor/makefun-bars.ts` — kit SDK shim, byte-identical copy
- `README.md`, `LICENSE` (MIT)
