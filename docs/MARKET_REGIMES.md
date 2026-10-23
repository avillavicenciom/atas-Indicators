# Market Regimes

## Purpose
The regime detector answers: “What kind of market is this right now?” It does not decide entries by itself.

## States

### TREND_UP
Characteristics may include:
- higher highs / higher lows;
- positive VWAP slope;
- price acceptance above VWAP;
- directional efficiency;
- trend strength;
- supportive CVD/delta persistence;
- healthy or expanding participation.

Allowed strategy family:
- pullback / continuation long.

### TREND_DOWN
Mirror of `TREND_UP`.

Allowed strategy family:
- pullback / continuation short.

### RANGE
Characteristics may include:
- flat/slower VWAP;
- stable POC;
- repeated rotation between value-area/range extremes;
- weak directional efficiency;
- rejected breakouts;
- contained volatility.

Allowed strategy family:
- reversion near validated extremes.
Avoid center-of-range entries.

### COMPRESSION
Characteristics may include:
- falling ATR percentile;
- Bollinger/ATR squeeze;
- low relative volume;
- contracting range.

Default behavior:
- wait.
- transition to breakout candidate only after expansion evidence.

### BREAKOUT_CANDIDATE
Characteristics may include:
- volatility expansion;
- relative-volume expansion;
- price leaving a prior balance area;
- directional order-flow confirmation.

Default behavior:
- do not chase blindly;
- wait for acceptance or a valid retest.

### TRANSITION
Used to prevent abrupt regime flipping.

Examples:
- range -> breakout -> transition -> trend;
- trend -> weakening -> transition -> range.

### HIGH_VOLATILITY
Used when movement/volatility is too extreme for normal thresholds or execution assumptions.

Default behavior:
- reduce activity or no trade.

### UNKNOWN
Used when evidence conflicts or data quality is insufficient.

Default behavior:
- no trade.

## Candidate feature groups

### Direction
- swing structure;
- VWAP slope;
- price vs VWAP;
- trend indicator direction where useful.

### Strength
- ADX;
- directional efficiency;
- persistence across bars.

### Volatility
- ATR percentile;
- range percentile;
- squeeze/expansion.

### Participation
- relative volume;
- volume expansion/contraction.

### Order-flow context
- CVD slope;
- delta persistence;
- divergence warning.

## Scoring
Use a multidimensional score rather than one binary indicator.

Example concept:
```text
Trend score
- structure
- VWAP slope
- location relative to VWAP
- directional efficiency
- ADX/trend strength
- volume participation
- CVD persistence
```

A “confidence” score means confidence in regime classification, not probability that a trade will win.

## Hysteresis
Use different thresholds for entering and leaving a regime. Require persistence or confirmation before switching state.
