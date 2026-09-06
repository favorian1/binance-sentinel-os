---
name: binance-risk-guardrails
description: "Exchange-native mathematical guardrail engine for Binance Agent OS. Dynamically queries Binance exchangeInfo filter rules (minNotional, maxNotional, LOT_SIZE, stepSize, maxPrice) and sub-account equity to enforce exact exchange compliance for any order size — from $5 to $1,000,000+."
---

# Binance Exchange-Native Risk Guardrails (Multi-Product)

Acts as the automated compliance officer for Antigravity, strictly validating every order against **official Binance exchange filters** via `spot.exchangeInfo` and `futures_usds.exchangeInformation` before dispatching signed requests.

---

## 1. Dynamic Binance Exchange Filter Auditing

Never hardcode arbitrary limits. Query the exchange rules directly:

### Spot Exchange Filters (`spot.exchangeInfo`):
- **`MIN_NOTIONAL` / `NOTIONAL`**: Minimum order value (typically 5.00 USDT or 10.00 USDT on Binance). Orders below this are rejected by Binance matching engines.
- **`MAX_NOTIONAL`**: Maximum allowable single order value published by Binance for that pair.
- **`LOT_SIZE`**:
  - `minQty`: Minimum tradeable base asset quantity.
  - `maxQty`: Maximum tradeable base asset quantity (millions for high-cap pairs).
  - `stepSize`: Valid decimal precision interval (e.g., 0.001 BNB).
- **`PRICE_FILTER`**:
  - `minPrice`: Minimum order price.
  - `maxPrice`: Maximum order price.
  - `tickSize`: Valid tick rounding increment.

### Futures Exchange Filters (`futures_usds.exchangeInformation` & `futures_coin.exchangeInformation`):
- **Contract Max Limits**: Symbol-specific leverage brackets and maximum position size tiers (e.g. tier 1 supports up to $5,000,000 notional; higher leverage reduces max position cap).
- **`MIN_NOTIONAL`**: Minimum contract notional (typically 5.00 USDT).
- **`MARKET_LOT_SIZE`**: Maximum market order clip size to prevent orderbook slippage.

---

## 2. Dynamic Capital Sizing & Whitelist Protocol

1. **Available Balance Check**:
   - Query `spot.getAccount` or `futures_usds.futuresAccountBalanceV3`.
   - Ensure `orderNotional <= availableBalance` (Binance Hard Limit).
2. **Whale / Large Order Execution**:
   - For orders with notional > $100,000 USDT:
     - Query `spot.depth` (limit: 50) to evaluate orderbook bid/ask depth and compute expected slippage.
     - Recommend TWAP (Time-Weighted Average Price) or iceberging rather than aggressive single-clip market buys.
3. **Margin Health Floor**:
   - Cross-margin collateral ratio must remain >= 1.5 before any borrow or margin order.
4. **Universal Emergency Protocol**:
   - **Trigger**: User issues `/stop`, abnormal price flash crash, or account anomaly.
   - **Action**: Immediately invoke:
     - `spot.deleteOpenOrders` (all active spot orders)
     - `futures_usds.currentAllOpenOrders` -> cancel via `futures_usds.cancelOrder`
     - `margin.marginAccountCancelAllOpenOrdersOnASymbol`
