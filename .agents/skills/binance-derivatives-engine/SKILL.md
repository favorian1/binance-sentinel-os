---
name: binance-derivatives-engine
description: "Advanced Futures & Margin trading execution skill for Binance Agent OS. Manages USDⓈ-M & COIN-M leverage, position risk, liquidation buffers, cross-margin collateral ratios, dynamic minimum contract lot size enforcement, and alternative pair substitution recommendations."
---

# Binance Derivatives, Margin & Convert Skill

Enables Antigravity to operate seamlessly across Binance's entire suite of financial products: USDⓈ-M Futures, COIN-M Futures, Margin Trading, and Binance Convert via official Agent OS MCP tools.

---

## 1. Dynamic Minimum Contract Thresholds & Pair Substitution

Different futures contracts on Binance have significantly different minimum order requirements (`minQty` * price = minimum entry cost):
- **High-Minimum Contracts**: For example, BTCUSDT or ETHUSDT contracts require a larger minimum dollar commitment to open 1 minimum lot (e.g., minimum 0.001 BTC is ~$65 USDT, which may exceed a small trader's available budget).
- **Micro-Budget Alternative Substitution**:
  - When a user requests a futures trade on a pair where their available margin is below the contract's minimum order requirement:
    1. Query `futures_usds.exchangeInformation` to detect the minimum contract notional / lot size for the requested symbol.
    2. If the user's budget is insufficient, **do not reject with a dead-end error**.
    3. Calculate the shortfall and immediately recommend **liquid, low-minimum alternative pairs** (e.g. `SOLUSDT`, `DOGEUSDT`, `ADAUSDT`, or `XRPUSDT`) where minimum order requirements start at $5.00 – $10.00 USDT.
    4. Provide the exact alternative trade specification ready for 1-click confirmation.

---

## 2. USDⓈ-M & COIN-M Futures Operations
- **Leverage & Margin Setting**:
  - Always verify and set leverage via `futures_usds.changeInitialLeverage` or `futures_coin.changeInitialLeverage` before placing derivative orders (max recommended for agent: 3x-5x).
  - Set margin mode via `futures_usds.changeMarginType` (ISOLATED preferred for risk containment).
- **Position Monitoring**:
  - Continuously track open positions and unPnl with `futures_usds.positionInformationV2`.
  - Calculate liquidation distance to ensure dynamic liquidation protection.
- **Contract Klines & Mark Price**:
  - Query continuous contract data via `futures_usds.continuousContractKlineCandlestickData` and `futures_usds.markPriceKlineCandlestickData`.

---

## 3. Margin & Collateral Management
- **Collateral Ratio**: Query `margin.crossMarginCollateralRatio` to ensure health level remains well above margin call threshold (> 2.0).
- **Borrow & Repay**: Execute controlled borrowing/repayment using `margin.marginAccountBorrowRepay`.
- **Order Routing**: Manage margin positions with `margin.marginAccountNewOrder` and `margin.marginAccountCancelAllOpenOrdersOnASymbol`.

---

## 4. Zero-Slippage Convert Workflows
- **Instant Swaps**:
  1. Call `convert.sendQuoteRequest` with fromAsset, toAsset, and amount.
  2. Parse quote ID and valid time window.
  3. Call `convert.acceptQuote` to settle instantly without orderbook slippage.
