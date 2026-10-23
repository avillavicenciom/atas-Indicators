# ATAS SDK Notes

This file records confirmed implementation patterns discovered from official ATAS documentation and the current repository. Re-verify exact API signatures before coding.

## Indicator basics
Existing ATAS indicators inherit from `Indicator` and implement `OnCalculate(int bar, decimal value)`.

Typical candle access:
```csharp
var candle = GetCandle(bar);
```

Useful candle-level values visible in the current source include:
- `Open`
- `High`
- `Low`
- `Close`
- `Volume`
- `Bid`
- `Ask`
- `Delta`
- `MinDelta`
- `MaxDelta`
- `OI`
- `VWAP` where applicable

## Footprint / price-level access
The repository confirms price-level access through:
```csharp
var info = candle.GetPriceVolumeInfo(price);
```

Relevant members used by ATAS source include:
- `Bid`
- `Ask`
- `Volume`
- delta-derived values

Reference:
- `Technical/StackedImbalance.cs`
- `Technical/ClusterSearch.cs`

## Stacked imbalance
`Technical/StackedImbalance.cs` scans the candle from low to high using `InstrumentInfo.TickSize`, reads Bid/Ask per level, and compares adjacent levels diagonally.

This is the preferred reference for implementing our footprint imbalance detector.

## Delta / CVD
References:
- `Technical/Delta.cs`
- `Technical/CumulativeDelta.cs`
- `Technical/MarketPower.cs`

The source demonstrates:
- candle delta;
- MinDelta/MaxDelta;
- session CVD;
- divergence-style logic;
- trade/cumulative-trade processing.

## Real-time trade events
The repository confirms usage of:
- `OnNewTrade(MarketDataArg trade)`
- `OnNewTrades(IEnumerable<MarketDataArg> trades)`
- `OnCumulativeTrade(CumulativeTrade trade)`
- `OnUpdateCumulativeTrade(CumulativeTrade trade)`
- `RequestForCumulativeTrades(...)`

Do not force all order-flow logic into `OnCalculate` if an event-driven path is more appropriate.

## Sessions
Many indicators use:
- `IsNewSession(bar)`
- `IsNewWeek(bar)`
- `IsNewMonth(bar)`

Session-aware reset behavior is important for VWAP, CVD and profile-type analytics.

## VWAP
Reference: `Technical/VWAP.cs`.

The implementation supports session/period handling and different volume modes such as total, Bid and Ask.

## DOM / L2
Reference: `Technical/DOM.cs`.

The source contains concepts for:
- depth snapshots/updates;
- stacked volume;
- pulled volume;
- net volume;
- cumulative asks/bids.

Use this only after capability detection and feed validation.

## MBO / L3
References:
- `Technical/DomV3/MainIndicator.cs`
- `Technical/DomV3/MboGridController.cs`

The current code shows:
- MBO subscription;
- `OnMarketByOrdersChanged(...)`;
- `MarketByOrder`;
- order IDs / priority;
- L3-to-L2 fallback behavior.

MBO availability depends on connector/feed/tariff. Never assume it exists.

## Strategy / execution
ATAS documentation includes `ChartStrategy` and asynchronous order-management APIs. This repository is indicator-focused and is not the main source for strategy execution examples.

Before writing execution code, verify:
- order open/modify/cancel APIs;
- order-change callbacks;
- registration/modify/cancel failure callbacks;
- current position;
- fills/partial fills;
- reconnect behavior;
- simulated vs real account behavior.

## Framework/build
Current repository README and project files reference .NET 10 and current ATAS installations. Verify the installed version before changing project targets.

## Performance
ATAS documentation emphasizes performance in real-time calculation. Avoid unnecessary allocations, repeated full scans, and expensive LINQ inside hot handlers unless measured and justified.
