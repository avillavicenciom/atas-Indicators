# AGENTS.md — ATAS Adaptive Trading Project

## Purpose
This repository is the technical base for an adaptive trading system built around ATAS. The goal is not to create a single “buy/sell indicator”, but a modular engine that first classifies the market regime, then enables only compatible strategies, validates setups with order flow, and only later evaluates execution.

## Primary sources of truth
Before implementing or changing ATAS-specific code, use this priority order:

1. Official ATAS SDK documentation: https://docs.atas.net/en/index.html
2. Official ATAS Help Center: https://help.atas.net/es/support/home
3. Current source code in this repository
4. NotebookLM notebook: **Integración ATAS MenthorQ**
   - Notebook ID: `32f0118c-cf5e-47d8-afbb-debc4ef93374`
   - Access through the configured NotebookLM MCP
5. External/community references only as secondary evidence

If sources disagree, do not guess. Document the discrepancy and prefer the highest-priority current official source.

## Mandatory rules for Codex
- Do not invent ATAS classes, methods, events, properties, enum values, order APIs, or file paths.
- Before using an ATAS API, verify it in official docs or current repository code.
- Reuse ATAS indicator implementations as references, but do not blindly copy full classes into the new architecture.
- Keep market analysis, strategy selection, risk, and execution decoupled.
- `WAIT` / `NO_TRADE` is a valid and expected system decision.
- Do not introduce real-money execution in early phases.
- Prefer relative/adaptive thresholds over fixed absolute values where instrument/feed scale differs.
- Do not treat a single delta divergence, imbalance, or footprint pattern as proof of reversal.
- MBO/L3 logic must degrade safely to L2, footprint, or no-trade when unavailable.
- Avoid heavy allocations/LINQ in hot paths such as `OnCalculate` and real-time event handlers unless justified.
- Preserve existing ATAS source files unless a task explicitly requires modifying them.

## Project architecture
Read these documents before coding:
- `docs/PROJECT_CONTEXT.md`
- `docs/ARCHITECTURE.md`
- `docs/ATAS_SDK_NOTES.md`
- `docs/MARKET_REGIMES.md`
- `docs/ORDER_FLOW.md`
- `docs/DATA_CAPABILITIES.md`
- `docs/DEVELOPMENT_PLAN.md`
- `docs/DECISIONS.md`

## Initial module targets
The intended modular design is:

- `MarketDataLayer`
- `MarketRegimeDetector`
- `MarketLocationEngine`
- `FootprintAnalyzer`
- `DeltaCvdAnalyzer`
- `LiquidityAnalyzer` (later, when feed supports it)
- `StrategySelector`
- `SignalEngine`
- `RiskManager`
- `TradeJournal`
- `ExecutionAdapter` / ATAS `ChartStrategy` layer (late phase)

## Current phase
The project is in research/bootstrap. No real-order logic should be added yet.

The next implementation milestone is:
1. capability discovery / market-data abstraction;
2. market-regime detector;
3. validation/logging;
4. only then order-flow confirmation modules.

## Build context
The current repository targets the installed ATAS platform. Existing project files show current builds around .NET 10. Verify the exact local ATAS installation and framework before changing target frameworks.

## Useful existing ATAS reference files
Especially relevant:
- `Technical/StackedImbalance.cs`
- `Technical/ClusterSearch.cs`
- `Technical/Delta.cs`
- `Technical/CumulativeDelta.cs`
- `Technical/MarketPower.cs`
- `Technical/VWAP.cs`
- `Technical/RelativeVolume.cs`
- `Technical/ADX.cs`
- `Technical/ATR.cs`
- `Technical/BollingerSqueezeV3.cs`
- `Technical/VolumeSupResZones.cs`
- `Technical/DOM.cs`
- `Technical/DomV3/MainIndicator.cs`
- `Technical/DomV3/MboGridController.cs`

## Start-of-task protocol
For any substantial implementation task:
1. Read this file and the relevant document under `docs/`.
2. Query NotebookLM if the task depends on ATAS SDK behavior not obvious from the repo.
3. Check the official ATAS docs for the exact API.
4. Search this repository for a known-good implementation pattern.
5. State assumptions and data prerequisites.
6. Implement the smallest testable unit.
7. Add logging/tests or a validation path before moving to execution.

## Safety boundary
This project may eventually support automated trading. Early development must remain analysis-only, alert-only, or simulated/paper mode. Any future real execution must have explicit risk limits, failure handling, order-state handling, disconnect behavior, and post-trade audit logging.
