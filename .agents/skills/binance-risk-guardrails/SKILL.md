---
name: binance-risk-guardrails
description: "Exchange-native mathematical guardrail and routing engine for Binance Agent OS. Dynamically audits Binance exchangeInfo filters (minNotional, maxNotional, LOT_SIZE) and automatically routes sub-$5 micro-trades to Binance Convert to prevent rejection."
---

# Binance Exchange-Native Risk Guardrails & Smart Routing

Acts as the automated compliance and routing engine for Antigravity, strictly validating every order against **official Binance exchange filters** and dynamically routing sub-minimum orders to **Binance Convert**.

---

## 1. Dynamic Order Routing (< $5 USDT Auto-Convert)

Binance's Spot Matching Engine strictly requires a `MIN_NOTIONAL` (typically 5.00 USDT). When an order is below this threshold:
- **Never fail or drop the trade**: Instead of throwing an error or letting Binance return `-1013 FILTER_FAILURE: MIN_NOTIONAL`, the agent automatically routes the trade through **Binance Convert** (`convert.sendQuoteRequest` & `convert.acceptQuote`).
- **Binance Convert supports small dust and micro-amounts**: Convert enables instant swaps with zero orderbook fees and lower minimum notional limits than the orderbook spot engine.

### Routing Decision Logic:
```
Target Trade Notional:
├─ Notional < MIN_NOTIONAL (< $5.00 USDT)
│    └─► Route to BINANCE CONVERT (convert.sendQuoteRequest -> convert.acceptQuote)
│        Zero orderbook rejection, instant execution, zero slippage.
│
├─ $5.00 USDT <= Notional <= $100,000 USDT
│    └─► Route to BINANCE SPOT (spot.newOrder)
│        Standard limit/market order with precise LOT_SIZE and tickSize compliance.
│
└─ Notional > $100,000 USDT (Whale Execution)
     └─► Audit spot.depth for liquidity, slice into TWAP/VWAP tranches to prevent market impact.
```

---

## 2. Dynamic Binance Exchange Filter Auditing

### Spot Exchange Filters (`spot.exchangeInfo`):
- **`MIN_NOTIONAL`**: Minimum order value (typically 5.00 USDT).
- **`MAX_NOTIONAL`**: Maximum allowable single order value published by Binance for that pair.
- **`LOT_SIZE`**: Validates `minQty`, `maxQty`, and `stepSize` precision.
- **`PRICE_FILTER`**: Validates `minPrice`, `maxPrice`, and `tickSize`.

### Futures Exchange Filters (`futures_usds.exchangeInformation`):
- **Contract Max Limits**: Symbol-specific leverage brackets and maximum position size tiers.
- **`MIN_NOTIONAL`**: Minimum contract notional (typically 5.00 USDT).

---

## 3. Margin & Emergency Protocols

1. **Available Balance Check**: Ensure `orderNotional <= availableBalance` (Binance Hard Limit).
2. **Margin Health Floor**: Cross-margin collateral ratio must remain >= 1.5 before any borrow or margin order.
3. **Universal Emergency Protocol**:
   - **Trigger**: User issues `/stop`, flash crash anomaly, or API error.
   - **Action**: Immediately invoke `spot.deleteOpenOrders` and `futures_usds.cancelOrder`.
