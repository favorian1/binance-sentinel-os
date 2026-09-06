---
name: binance-risk-guardrails
description: "Pre-trade mathematical guardrail engine for Binance Agent OS. Enforces per-trade notional ceilings, daily aggregate spend caps, allowlisted trading universes, and immediate emergency order cancellation."
---

# Binance Pre-Trade Risk Guardrails Skill

Acts as the automated compliance officer for Antigravity before any non-idempotent action is dispatched to the Binance Agent OS endpoint.

## Deterministic Rules & Ceilings
- **Max Notional Per Order**: Strict cap of **$50.00 USDT** per single transaction.
- **24-Hour Max Spend**: Maximum cumulative execution volume of **$250.00 USDT** across any 24-hour rolling window.
- **Allowed Symbol Universe**: Restricted exclusively to liquid, major pairs: `BNBUSDT`, `BTCUSDT`, `ETHUSDT`, `SOLUSDT`.
- **Sub-Account Invariant**: All operations must execute strictly within the designated Binance Agentic Sub-Account. Withdrawals and external transfers are strictly prohibited.

## Emergency Protocols
- **Trigger**: Anomaly detection, user `/stop` command, or market volatility spikes > 10% in 15 minutes.
- **Action**: Immediately invoke `spot.deleteOpenOrders` for all active symbols to cancel all pending limit and stop orders.
