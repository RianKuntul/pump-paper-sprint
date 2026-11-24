[![make.fun · u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b](https://make.fun/badge/u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b.svg)](https://make.fun/bounty/u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b?utm_source=github&utm_medium=readme&utm_campaign=u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b)
[![rank](https://make.fun/badge/u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b/RianKuntul/pump-paper-sprint/rank.svg)](https://make.fun/bounty/u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b)
[![reward](https://make.fun/badge/u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b/RianKuntul/pump-paper-sprint/reward.svg)](https://make.fun/bounty/u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b?utm_source=github&utm_medium=readme&utm_campaign=u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b)

# Surge Sniper — Pump.fun Paper Sprint

A momentum bot for make.fun's **Pump.fun Paper Sprint: Any Bot Can Win**
(`u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b`). One program,
`main.ts` on Node 22, paper-trades pump.fun bonding curves on a 6-hour replay
of real market data. Highest net PnL wins; no real money, free entry.

## Strategy

Catch a curve while buyers are piling in. A curve becomes a buy when its
recent tape shows real demand — net SOL flowing in over a short window, several
distinct buyers, the price climbing and sitting near its window high, and the
inflow accelerating (the newer half of the window out-buys the older half) —
while the curve is still small enough to run. The first buy is small
(0.05 SOL); the position is pyramided into strength only, up to 0.25 SOL per
coin. Every position exits whole on the first of a take-profit (+20%), a
stop-loss (−12%), a trailing stop once up 10%, a fade in net flow, a single
large sell, or a 1200-slot time limit. A curve that completes is closed by the
replay at its final reserves.

Tiny probe round trips keep the run above the minimum bar (round trips on at
least 20 coins) when the bot's own fills lag the pace.

Deterministic: no clock, no randomness, no network. The only input is the
harness stream on stdin (`agent-fun/replay/v2`); replies go to stdout.

## Layout

- `main.ts` — entrypoint, run by the grader as `node /bot/main.ts`.
- `strategy.ts` — the strategy.
- `vendor/makefun-bot.ts` — the kit's SDK shim, vendored byte for byte.

## Local backtest

On the kit's dev window (pilot's local engine port, `tools/sim.ts`):

- v1: net −1.767 SOL, 506 fills, 84 coins, 121 trips, max drawdown 0.29
- v2: net −0.034 SOL, 429 fills, 88 coins, 151 trips, max drawdown 0.07

Both clear the guardrails (≥200 fills over ≥20 coins, drawdown ≤ 50%). The
final window is fresh 6-hour data collected after entries close, so the dev
numbers are a sanity check, not the score.

## License

MIT — see `LICENSE`.
