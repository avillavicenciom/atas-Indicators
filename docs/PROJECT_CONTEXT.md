# Project Context

## Objective
Build an adaptive ATAS-based trading system that analyzes the current market environment before deciding which strategy, if any, is allowed to operate.

The system should not behave as:
`footprint signal -> automatic trade`.

The intended flow is:
`market regime -> market location -> setup -> order-flow confirmation -> risk gate -> decision`.

## Core idea
Different market regimes require different behavior:
- strong trend: continuation / pullback logic;
- range: reversal near meaningful extremes;
- breakout: acceptance/retest logic;
- compression: usually wait for expansion;
- extreme volatility / ambiguity / poor liquidity: no trade.

## Initial market-regime states
- `TREND_UP`
- `TREND_DOWN`
- `RANGE`
- `COMPRESSION`
- `BREAKOUT_CANDIDATE`
- `TRANSITION`
- `HIGH_VOLATILITY`
- `UNKNOWN`

## Initial strategies
- Trend pullback / continuation
- Range reversion
- Breakout + acceptance / retest
- No-trade state

## Validation philosophy
The system must record what happened after every hypothesis. Each setup should preserve:
- timestamp;
- instrument;
- regime;
- regime score/confidence;
- location;
- order-flow evidence;
- decision;
- invalidation;
- entry/exit if simulated;
- MFE;
- MAE;
- costs/slippage when available;
- result.

The purpose is to discover which combinations remain useful out of sample instead of assuming a pattern works because it looks convincing on a chart.

## Data sources
The system may combine:
- OHLC;
- volume;
- VWAP;
- volume profile / POC / VAH / VAL;
- Bid/Ask;
- Delta;
- CVD;
- footprint by price level;
- stacked imbalances;
- relative volume;
- ATR / volatility;
- trend-strength measures;
- L2 DOM;
- MBO/L3 if available;
- open interest where supported.

Not every feed provides every data type. Capability detection is therefore part of the architecture.

## Development environment
Primary implementation language: C#.
Primary analytics platform: ATAS.
Repository: `avillavicenciom/atas-Indicators`.
Main branch inherited from repository: `Develop`.
Bootstrap branch: `agent/project-bootstrap`.

## NotebookLM knowledge source
Notebook: **Integración ATAS MenthorQ**
Notebook ID: `32f0118c-cf5e-47d8-afbb-debc4ef93374`

The NotebookLM MCP is intended to help Codex query the curated ATAS documentation and related sources during development.

## Account / rollout assumption
Initial development can begin on ATAS Start/free using features available there, especially for development and crypto/live analysis where supported. Futures real-time, replay, MBO/premium capabilities, and execution requirements should be verified again when those phases are reached.
