# Decisions Log

## 2026-10-23 — Adaptive architecture
Decision:
Build an adaptive regime-based system instead of a single footprint bounce indicator.

Reason:
A footprint pattern can mean different things in trend, range, breakout or extreme volatility.

## 2026-10-23 — Footprint is confirmation, not regime detection
Decision:
Use footprint/order flow after regime and location.

Reason:
Avoid treating a local imbalance or delta pattern as a universal entry signal.

## 2026-10-23 — Prefer zones over “exact reversal prices”
Decision:
Represent support/resistance/reversal opportunities as short zones bounded by tick resolution.

Reason:
Exact-price claims create false precision.

## 2026-10-23 — Statistical validation required
Decision:
Journal every hypothesis and later outcome.

Reason:
Patterns must be validated empirically and out of sample.

## 2026-10-23 — Reuse ATAS source as implementation reference
Decision:
Study existing official/current indicator implementations before writing equivalent logic.

Primary references:
- StackedImbalance
- ClusterSearch
- Delta / CumulativeDelta / MarketPower
- VWAP
- RelativeVolume
- ADX / ATR
- DOM / DomV3

## 2026-10-23 — Keep execution separate
Decision:
Do not add real order execution during early development.

Reason:
Analysis errors must not become broker orders.

## 2026-10-23 — Evaluate ChartStrategy before NinjaTrader bridge
Decision:
When execution phase begins, first evaluate ATAS native strategy/order APIs.

Reason:
Fewer moving parts if ATAS can satisfy execution requirements.

## 2026-10-23 — Data capability degradation
Decision:
MBO is optional. The system must degrade:
`MBO -> L2 -> footprint -> bars -> NO_TRADE`.

## 2026-10-23 — Relative thresholds
Decision:
Prefer percentiles/z-scores/relative volume and instrument-aware thresholds over fixed values like “Delta > 200”.

## 2026-10-23 — Regime hysteresis
Decision:
Use transition states and different enter/exit thresholds.

Reason:
Prevent strategy changes on every marginal candle.

## 2026-10-23 — Initial account
Decision:
Begin development using the ATAS Start/free tier where sufficient. Re-evaluate paid plans when futures real-time, replay, MBO or execution become required.

## 2026-10-23 — Codex context strategy
Decision:
Persist project context in this repository through `AGENTS.md` and `docs/`, while using NotebookLM MCP for curated ATAS research.

Reason:
Codex chats are not the durable source of project knowledge.
