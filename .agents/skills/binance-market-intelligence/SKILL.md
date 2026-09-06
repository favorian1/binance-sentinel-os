---
name: binance-market-intelligence
description: "Real-time technical and quantitative analysis on Binance via Agent OS MCP. Fetches orderbook depth, computes EMA and RSI momentum indicators, scans 24h ticker metrics, and queries AI token reports."
---

# Binance Market Intelligence Skill

Enables Antigravity to perform deep technical, momentum, and quantitative market scans across Binance markets using official Agent OS MCP tools.

## Supported MCP Tools
- `spot.tickerPrice`: Current mid-market pricing.
- `spot.ticker24hr`: 24-hour price change percentage, volume, high/low spread.
- `spot.klines`: Historical candlestick arrays (1m, 5m, 15m, 1h, 4h, 1d).
- `spot.depth`: Orderbook bid/ask liquidity structure and slippage estimation.
- `analysis.getTokenAiReport`: Token-specific fundamental intelligence and sentiment score.

## Analytical Protocols
1. **Trend Identification**:
   - Query 30 periods of 1h klines via `spot.klines`.
   - Calculate EMA-9 and EMA-21 to determine short-term vs medium-term momentum.
2. **Overbought / Oversold Detection**:
   - Evaluate RSI-14.
   - RSI < 30 indicates accumulation zone (potential DCA entry).
   - RSI > 70 indicates exhaustion / profit-taking zone.
3. **Liquidity Depth Assessment**:
   - Query `spot.depth` with limit=20 before routing larger trades to verify tight bid-ask spreads.
