---
name: binance-market-intelligence
description: "Real-time technical and quantitative analysis across Binance Spot, USDⓈ-M Futures, COIN-M Futures, and Margin products via Agent OS MCP. Fetches orderbook depth, mark price klines, premium index klines, continuous contract data, computes EMA/RSI indicators, and queries AI token reports."
---

# Binance Market Intelligence (Multi-Product)

Enables Antigravity to perform deep technical, momentum, and quantitative market scans across ALL Binance product lines using official Agent OS MCP tools.

## Supported MCP Tools by Product

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
- `futures_coin.klineCandlestickData`, `futures_coin.markPriceKlineCandlestickData`: Same for COIN-M.

### Margin Intelligence
- `margin.queryCrossMarginAccountDetails`: Cross-margin net assets and liability status.
- `margin.crossMarginCollateralRatio`: Real-time collateral health ratio.

### AI Reports
- `analysis.getTokenAiReport`: Token fundamental intelligence and market sentiment score.

## Analytical Protocols

1. **Cross-Product Momentum Alignment**:
   - Check spot price trend via `spot.klines` (EMA-9 vs EMA-21).
   - Cross-reference with futures `futures_usds.markPriceKlineCandlestickData` to check premium/discount.
   - If spot and futures trends align → stronger signal conviction.

2. **Funding Rate Regime Check**:
   - Query `futures_usds.premiumIndexKlineData` to gauge whether funding rate is positive (bullish crowding) or negative (bearish crowding).
   - Persistently negative funding = potential spot accumulation zone.

3. **Orderbook Liquidity & Slippage Pre-check**:
   - Query `spot.depth` (limit=20) before any market buy/sell to verify tight bid-ask spreads.

4. **Overbought / Oversold Detection (RSI-14)**:
   - RSI < 30 → oversold accumulation zone (consider DCA entry across spot and futures).
   - RSI > 70 → exhaustion zone (scale out or tighten stop-loss).
