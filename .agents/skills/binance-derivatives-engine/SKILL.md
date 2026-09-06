---
name: binance-derivatives-engine
description: "Advanced Futures & Margin trading execution skill for Binance Agent OS. Manages USDⓈ-M & COIN-M leverage, position risk, liquidation buffers, cross-margin collateral ratios, and zero-slippage Convert."
---

# Binance Derivatives, Margin & Convert Skill

Enables Antigravity to operate seamlessly across Binance's entire suite of financial products: USDⓈ-M Futures, COIN-M Futures, Margin Trading, and Binance Convert via official Agent OS MCP tools.

## 1. USDⓈ-M & COIN-M Futures Operations
- **Leverage & Margin Setting**:
  - Always verify and set leverage via `futures_usds.changeInitialLeverage` or `futures_coin.changeInitialLeverage` before placing derivative orders (max recommended for agent: 3x-5x).
  - Set margin mode via `futures_usds.changeMarginType` (ISOLATED preferred for risk containment).
- **Position Monitoring**:
  - Continuously track open positions and unPnl with `futures_usds.positionInformationV2`.
  - Calculate liquidation distance to ensure dynamic liquidation protection.
- **Contract Klines & Mark Price**:
  - Query continuous contract data via `futures_usds.continuousContractKlineCandlestickData` and `futures_usds.markPriceKlineCandlestickData`.

## 2. Margin & Collateral Management
- **Collateral Ratio**: Query `margin.crossMarginCollateralRatio` to ensure health level remains well above margin call threshold (> 2.0).
- **Borrow & Repay**: Execute controlled borrowing/repayment using `margin.marginAccountBorrowRepay`.
- **Order Routing**: Manage margin positions with `margin.marginAccountNewOrder` and `margin.marginAccountCancelAllOpenOrdersOnASymbol`.

## 3. Zero-Slippage Convert Workflows
- **Instant Swaps**:
  1. Call `convert.sendQuoteRequest` with fromAsset, toAsset, and amount.
  2. Parse quote ID and valid time window.
  3. Call `convert.acceptQuote` to settle instantly without orderbook slippage.
