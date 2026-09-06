---
name: binance-risk-guardrails
description: "Pre-trade mathematical guardrail engine for Binance Agent OS. Enforces per-trade notional ceilings, daily aggregate spend caps, futures leverage limits, margin health floors, allowlisted trading universes, and immediate emergency order cancellation across Spot, Futures, and Margin products."
---

# Binance Pre-Trade Risk Guardrails (Multi-Product)

Acts as the automated compliance officer for Antigravity before any non-idempotent action is dispatched to the Binance Agent OS endpoint across ALL product verticals.

## Deterministic Rules & Ceilings

### Spot Rules
- **Max Notional Per Spot Order**: $50.00 USDT.
- **24h Spot Aggregate Spend Cap**: $250.00 USDT.
- **Allowed Spot Universe**: `BNBUSDT`, `BTCUSDT`, `ETHUSDT`, `SOLUSDT`.

### Futures Rules
- **Max Leverage**: Capped at 5x. Never set leverage above 5x via `futures_usds.changeInitialLeverage` or `futures_coin.changeInitialLeverage`.
- **Margin Mode**: ISOLATED margin required for all agentic futures positions.
- **Max Open Futures Positions**: 3 active contracts simultaneously.
- **Liquidation Buffer**: Position must maintain at least 20% buffer from liquidation price before entering. Query `futures_usds.positionInformationV2` to verify.

### Margin Rules
- **Margin Health Floor**: Cross-margin collateral ratio must be >= 2.0. Query `margin.crossMarginCollateralRatio` before any borrow or new margin order.
- **Max Borrow Cap**: $100.00 USDT equivalent per borrow action.

### Universal Emergency Protocol
- **Trigger**: Any of the following — user issues `/stop`, portfolio loss > 10% of sub-account in 1h, or API response anomaly.
- **Action**: Simultaneously invoke:
  - `spot.deleteOpenOrders` (all active spot orders)
  - `futures_usds.currentAllOpenOrders` -> cancel via `futures_usds.cancelOrder`
  - `margin.marginAccountCancelAllOpenOrdersOnASymbol` for active margin symbols
