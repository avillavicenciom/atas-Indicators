# Data Capabilities

## Goal
The bot must detect what data is actually available and adapt safely.

## Capability model
Suggested capability flags:

```text
HasCandles
HasVolume
HasBidAsk
HasFootprintLevels
HasTrades
HasCumulativeTrades
HasDomL2
HasMboL3
HasOpenInterest
HasReplay
CanSendOrders
```

## Degradation hierarchy

### Full
`MBO/L3 + L2 + trades + footprint + bars`

Use advanced liquidity analysis.

### L2
`L2 + trades + footprint + bars`

Disable L3-specific features.

### Footprint
`trades/footprint + bars`

Use delta/CVD/imbalance/cluster logic, no DOM assumptions.

### Bars only
`OHLC/volume`

Regime analysis may remain possible, but order-flow setups should normally be disabled.

### Insufficient
Return `NO_TRADE`.

## Why this matters
Availability can vary by:
- instrument;
- exchange;
- connector;
- data package;
- ATAS tariff;
- replay/live context.

The system must never crash or silently substitute fabricated values when a data type is missing.

## Initial development scope
V1:
- bars;
- volume;
- VWAP;
- ATR;
- ADX;
- relative volume;
- footprint Bid/Ask/Delta where available.

V2:
- L2 DOM.

V3:
- MBO/L3 and advanced liquidity interpretation.

## Runtime reporting
The journal should record active capabilities for each signal so later statistics can distinguish results from different feed qualities.
