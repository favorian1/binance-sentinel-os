---
name: binance-market-intelligence
description: "Real-time technical and quantitative analysis across Binance Spot, USDⓈ-M Futures, COIN-M Futures, Margin, and Tokenized Stocks via Agent OS MCP. Understands exact Binance pair taxonomy (Base vs Quote, USDT vs USDC vs FDUSD, Perps vs Delivery, Coin-Margined, bstock)."
---

# Binance Market Intelligence & Pair Taxonomy Engine

Enables Antigravity to perform deep technical, momentum, and quantitative market scans across ALL Binance product lines, with complete awareness of **Binance pair structures and quote asset differences**.

---

## 1. Complete Binance Pair Taxonomy & Structure

The agent treats every pair according to Binance's exact architectural taxonomy:

| Product Class | Symbol Convention | Settlement Asset | Behavior & Characteristics |
| :--- | :--- | :--- | :--- |
| **Spot Major Stable** | `BASE + USDT` (e.g. `BNBUSDT`, `BTCUSDT`) | USDT | Deepest liquidity orderbooks. Subject to `spot.exchangeInfo` filters. |
| **Spot Alternative Stables** | `BASE + USDC`, `BASE + FDUSD` | USDC / FDUSD | Zero or reduced maker fees on Binance (promotional). Ideal for low-cost rebalancing. |
| **USDⓈ-M Perpetual** | `BASE + USDT` (e.g. `BTCUSDT`) | USDT / Multi-Asset | No expiration date. Incurs/receives funding rate every 8h via `futures_usds.premiumIndexKlineData`. |
| **USDⓈ-M Delivery** | `BASE + USDT_YYMMDD` (e.g. `BTCUSDT_260925`) | USDT | Quarterly expiry. Settles at expiration date, zero funding fees. |
| **COIN-M Perpetual (Inverse)** | `BASE + USD_PERP` (e.g. `BTCUSD_PERP`) | Base Asset (e.g. BTC) | P&L and margin denominated in cryptocurrency. Ideal for accumulating native tokens. |
| **Margin Pairs** | `BASE + QUOTE` | Borrowed asset | Cross (collateral pooled) vs. Isolated (risk segregated per pair). |
| **Tokenized Stocks (bstock)** | `TICKER + BUSD / USDT` (e.g. `TSLAbstock`) | USDT | Fractional tokenized equity traded on Binance Spot rails. |

---

## 2. Dynamic Pair Verification Protocol

1. **Quote Asset Disambiguation**:
   - If user asks *"buy BNB"*, check available sub-account assets. If user holds USDC, route to `BNBUSDC`; if USDT, route to `BNBUSDT`; if holding dust, auto-route via `convert.sendQuoteRequest`.
2. **Product Distinction (Spot vs. Perp vs. Coin-M)**:
   - When asked to trade futures:
     - If user specifies collateral in crypto (e.g. *"use my BTC as margin"*), route to **COIN-M Futures** (`futures_coin.*`).
     - If user specifies USDT/USDC, route to **USDⓈ-M Futures** (`futures_usds.*`).
3. **Cross-Product Price Verification**:
   - Compare `spot.tickerPrice` vs `futures_usds.symbolPriceTicker` to detect basis spreads (contango vs backwardation).

---

## 3. Supported MCP Tools by Product

### Spot Intelligence
- `spot.tickerPrice`: Current mid-market pricing.
- `spot.ticker24hr`: 24h price change %, volume, high/low spread.
- `spot.klines`: Spot historical candlestick arrays (1m, 5m, 15m, 1h, 4h, 1d).
- `spot.depth`: Orderbook bid/ask liquidity structure and spread estimation.

### Futures Intelligence (USDⓈ-M & COIN-M)
- `futures_usds.klineCandlestickData`: USDⓈ-M Futures price candles.
- `futures_usds.markPriceKlineCandlestickData`: Mark price data (used for liquidation detection).
- `futures_usds.continuousContractKlineCandlestickData`: Continuous contract charts for premium analysis.
- `futures_usds.premiumIndexKlineData`: Funding rate and premium index dynamics.
- `futures_usds.symbolPriceTicker`: Real-time futures best price ticker.
- `futures_coin.klineCandlestickData`: Same for COIN-M contracts.

### Margin Intelligence
- `margin.queryCrossMarginAccountDetails`: Cross-margin net assets and liability status.
- `margin.crossMarginCollateralRatio`: Real-time collateral health ratio.

### AI Reports
- `analysis.getTokenAiReport`: Token fundamental intelligence and market sentiment score.
