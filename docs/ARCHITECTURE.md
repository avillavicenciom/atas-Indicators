# Architecture

## High-level flow

```text
ATAS DATA
   |
   +-- Candles / OHLC / sessions
   +-- Volume / VWAP / profile
   +-- Footprint / PriceVolumeInfo
   +-- Trades / cumulative trades
   +-- DOM L2
   +-- MBO L3 (optional)
          |
          v
    MarketDataLayer
          |
    +-----+----------------+
    |                      |
    v                      v
MarketRegimeDetector   OrderFlowEngine
    |                      |
    +----------+-----------+
               v
        MarketLocationEngine
               |
               v
         StrategySelector
               |
               v
           SignalEngine
               |
               v
           RiskManager
               |
               v
          TradeJournal
               |
               v
        ExecutionAdapter
```

## Separation of responsibilities

### MarketDataLayer
Normalizes ATAS data access and exposes capabilities. It should hide whether a particular source is:
- bar-based;
- trade-by-trade;
- L2;
- MBO/L3.

### MarketRegimeDetector
Classifies market context. It should not open trades.

Candidate features:
- swing structure;
- VWAP slope/location;
- ADX/trend strength;
- ATR percentile;
- directional efficiency;
- relative volume;
- compression/expansion;
- CVD persistence as context, not sole trigger.

### MarketLocationEngine
Determines whether price is at a meaningful area:
- VWAP;
- VWAP deviations;
- POC;
- VAH/VAL;
- prior high/low;
- volume nodes/zones;
- range extremes;
- breakout/retest zones.

### OrderFlowEngine
Combines:
- footprint;
- Bid/Ask;
- delta/CVD;
- imbalance;
- absorption candidates;
- exhaustion;
- trades;
- optional DOM/MBO information.

### StrategySelector
Enables only the strategy family compatible with the active regime.

Examples:
- `TREND_UP` -> long pullback/continuation candidates;
- `TREND_DOWN` -> short pullback/continuation candidates;
- `RANGE` -> mean-reversion only near range extremes;
- `BREAKOUT_CANDIDATE` -> wait for acceptance/retest;
- `COMPRESSION`, `HIGH_VOLATILITY`, `UNKNOWN` -> normally no new trade.

### SignalEngine
Combines regime + location + strategy + order flow into:
- `BUY_CANDIDATE`
- `SELL_CANDIDATE`
- `WAIT`
- later: confirmed signals.

### RiskManager
Independent from signal generation. Even if regime classification changes, it must continue to control any open position safely.

### TradeJournal
Stores every decision and its later outcome for statistical validation.

### ExecutionAdapter
Late-phase component. Prefer ATAS strategy APIs if they satisfy requirements; otherwise evaluate an external executor. Must not be coupled to analysis modules.

## State stability
The regime detector must use hysteresis/stability logic. A one-bar threshold crossing must not cause immediate strategy flipping.

Possible state flow:
```text
RANGE
  -> COMPRESSION
  -> BREAKOUT_CANDIDATE
  -> TRANSITION
  -> TREND_UP / TREND_DOWN
```

## Performance principle
Keep hot paths allocation-light. Heavy analytics, historical statistics, and logging should not block ATAS real-time callbacks.
