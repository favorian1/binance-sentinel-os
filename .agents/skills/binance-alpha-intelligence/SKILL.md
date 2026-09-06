---
name: binance-alpha-intelligence
description: "AI-powered market scanner and alpha discovery engine for Binance Agent OS. Evaluates 24-hour volume surges, token AI intelligence reports, and orderbook microstructure imbalance to identify high-probability setups."
---

# Binance Alpha Intelligence & AI Token Discovery Skill

Empowers Antigravity to conduct autonomous opportunity discovery across Binance markets, filtering noise to surface institutional-grade token intelligence.

## Integrated MCP Tools
- `analysis.getTokenAiReport`: Binance native AI token intelligence reports, social momentum, and developer sentiment.
- `spot.ticker24hr`: Rolling 24-hour volume, price volatility, and turnover ranking.
- `spot.depth`: Bid/Ask book depth, imbalance ratios, and wall detection.
- `futures_usds.symbolPriceTicker`: Futures premium/discount tracking across candidate tokens.

## Alpha Discovery Protocols
1. **Momentum & Volume Outlier Scan**:
   - Screen `spot.ticker24hr` for symbols exhibiting volume > 2x their 7-day average accompanied by positive price action (+3% to +8%).
   - Filter out low-liquidity pairs (minimum 24h volume threshold: $5,000,000 USDT).
2. **AI Fundamental Verification**:
   - For identified momentum candidates, query `analysis.getTokenAiReport` to verify positive ecosystem sentiment and avoid honeypots or negative regulatory catalysts.
3. **Orderbook Imbalance Ratio**:
   - Query top 20 levels via `spot.depth`.
   - Calculate Bid/Ask Volume Ratio: `Total Bid Vol / Total Ask Vol`.
   - Ratio > 1.5 indicates strong buy-side absorption support.
