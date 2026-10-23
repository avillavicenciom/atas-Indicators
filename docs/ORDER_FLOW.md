# Order Flow Design

## Principle
Order flow confirms context; it does not define the whole market regime.

Preferred flow:
`regime -> location -> setup -> order-flow confirmation -> risk gate`.

## Footprint
Primary price-level data:
- Bid;
- Ask;
- Volume;
- Delta;
- concentration / POC-like behavior.

Reference implementations:
- `Technical/ClusterSearch.cs`
- `Technical/StackedImbalance.cs`

## Diagonal imbalance
Use the same conceptual model as ATAS Stacked Imbalance:
- compare adjacent price levels diagonally;
- require a ratio threshold;
- require a minimum volume filter;
- optionally require N consecutive levels.

Do not use whole-candle Ask/Bid ratios as a substitute for true footprint imbalance.

## Absorption
Treat absorption as a hypothesis, not a guaranteed reversal.

A candidate pattern may combine:
- strong aggressive volume into an area;
- limited price progress;
- rejection or failure to continue;
- subsequent opposing response;
- optional CVD/delta confirmation.

A single large negative/positive delta is not enough.

## Exhaustion
Possible exhaustion may involve:
- diminishing aggressive volume at the extreme;
- poor continuation;
- rejection/failed auction behavior.

It should be location-aware and validated statistically.

## Delta and CVD
Use:
- candle delta;
- MinDelta / MaxDelta;
- session CVD;
- CVD slope/persistence;
- price/CVD divergence as warning or confirmation.

Do not interpret divergence alone as an entry.

## Cluster zones
Prefer short zones over false “exact reversal prices”.

Possible zone output:
- price range;
- total volume;
- Bid;
- Ask;
- Delta;
- local POC;
- location relative to VWAP/value/range;
- evidence;
- required confirmation;
- invalidation.

## Trades
For real-time flow, prefer event-driven trade handlers where appropriate:
- new trades;
- cumulative trades;
- updates.

## DOM / L2
Later phase.
Potential features:
- total depth;
- bid/ask imbalance;
- stacking;
- pulling;
- net liquidity change;
- distance to best bid/ask;
- persistence vs cancellation.

Visible liquidity can be cancelled, so it must not be treated as committed intent.

## MBO / L3
Optional advanced phase.
Potential features:
- order count;
- order size distribution;
- priority;
- refill behavior;
- native iceberg clues when the feed makes them observable.

Do not infer institutional identity.

## Data quality labels
Every order-flow conclusion should know its data quality:
- `L3_MBO`
- `L2_DOM`
- `FOOTPRINT_ONLY`
- `BAR_ONLY`
- `INSUFFICIENT`

The signal engine should become more conservative as data quality decreases.
