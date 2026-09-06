---
name: binance-portfolio-rebalancer
description: "Portfolio audit and algorithmic rebalancing skill for Binance Agent OS. Evaluates sub-account asset distribution, calculates target weights, and plans guarded dollar-cost averaging (DCA) allocations."
---

# Binance Portfolio & Rebalancing Skill

Enables Antigravity to conduct automated sub-account balance audits and structure disciplined asset rebalancing without manual spreadsheet calculations.

## Supported MCP Tools
- `spot.getAccount`: Sub-account asset balances, free vs locked funds.
- `wallet.queryUserWalletBalance`: Comprehensive wallet breakdown.
- `convert.listAllConvertPairs`: Low-friction zero-slippage conversion options.
- `convert.sendQuoteRequest` & `convert.acceptQuote`: Automated token conversions.

## Rebalancing Protocol
1. **Holdings Audit**:
   - Query `spot.getAccount` and filter for assets with non-zero balances.
   - Price all balances to USDT using `spot.tickerPrice`.
2. **Allocation Divergence Check**:
   - Target Portfolio: e.g. 50% BNB, 30% BTC, 20% USDT Cash Reserve.
   - Calculate divergence from target weights.
3. **Execution Routing**:
   - For small divergences: Plan micro-DCA limit orders via `spot.newOrder`.
   - For direct conversions: Utilize Binance Convert tools (`convert.sendQuoteRequest`).
