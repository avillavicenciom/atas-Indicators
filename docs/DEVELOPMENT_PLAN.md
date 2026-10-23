# Development Plan

## Phase 0 — SDK and capability discovery
Deliverables:
- verify build against local ATAS installation;
- document exact ATAS APIs used;
- implement capability detection;
- create minimal diagnostic indicator/tool;
- prove access to bars, footprint and real-time events.

Exit criteria:
- no invented API usage;
- diagnostics show what the feed exposes.

## Phase 1 — MarketDataLayer
Deliverables:
- normalized candle snapshot;
- session information;
- footprint level snapshot;
- trade event normalization;
- optional DOM/MBO adapters behind interfaces.

Exit criteria:
- deterministic snapshots/logging;
- missing data handled safely.

## Phase 2 — MarketRegimeDetector
Deliverables:
- `TREND_UP`;
- `TREND_DOWN`;
- `RANGE`;
- `COMPRESSION`;
- `BREAKOUT_CANDIDATE`;
- `TRANSITION`;
- `HIGH_VOLATILITY`;
- `UNKNOWN`.

Initial features:
- structure;
- VWAP slope/location;
- ADX;
- ATR percentile;
- relative volume;
- compression/expansion.

Exit criteria:
- stable state transitions;
- regime journal;
- visual/debug output.

## Phase 3 — MarketLocationEngine
Deliverables:
- VWAP/deviations;
- POC/value zones where available;
- prior high/low;
- range boundaries;
- volume support/resistance zones.

## Phase 4 — OrderFlowEngine
Deliverables:
- diagonal imbalance;
- stacked imbalance;
- delta/CVD context;
- cluster concentration;
- absorption candidate;
- exhaustion candidate.

Exit criteria:
- output evidence objects, not direct trades.

## Phase 5 — StrategySelector
Rules:
- trend -> pullback/continuation;
- range -> edge reversion;
- breakout -> acceptance/retest;
- compression/unknown/high-volatility -> mostly no-trade.

## Phase 6 — SignalEngine + Risk model
Combine:
- regime;
- location;
- strategy;
- order-flow confirmation;
- invalidation;
- expected execution cost.

No real execution yet.

## Phase 7 — TradeJournal and validation
Capture:
- all inputs;
- regime;
- strategy;
- evidence;
- decision;
- MFE/MAE;
- result;
- costs;
- capability level.

Evaluate:
- sample size;
- hit rate;
- expectancy;
- drawdown;
- performance by regime;
- performance by setup;
- out-of-sample behavior.

## Phase 8 — Paper / simulation
Only after earlier modules are stable.

## Phase 9 — Execution architecture
Evaluate ATAS `ChartStrategy` first.
If it satisfies requirements, prefer in-platform execution.
Otherwise evaluate a separate execution adapter.

Must handle:
- order registration;
- partial fills;
- rejection;
- modification;
- cancellation;
- disconnect/reconnect;
- duplicated signals;
- max loss/exposure;
- emergency stop.

## Phase 10 — Real execution
Only after explicit review of simulation evidence and risk controls.
