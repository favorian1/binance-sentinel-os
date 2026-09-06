---
name: binance-sentinel
description: "Autonomous Full-Stack Binance Sentinel Agent operating natively inside Antigravity via Binance Agent OS MCP Server. Handles Spot, USDⓈ-M Futures, COIN-M Futures, Margin, Convert, and Sub-Account asset management."
---

# Binance Sentinel-OS: Master Multi-Product Mandate

This skill equips Antigravity (AGY) to act as a full-spectrum financial co-pilot across **Spot, Futures, Margin, Convert, and Wallet** products via the official **Binance Agent OS MCP Server**.

---

## 1. Multi-Product Architecture & Workflows

### A. Spot & Tokenized Equities (`spot.*`)
- Price discovery: `spot.tickerPrice`, `spot.ticker24hr`, `spot.depth`.
- Order execution: `spot.newOrder`, `spot.deleteOrder`, `spot.deleteOpenOrders`.

### B. Derivatives & Leverage (`futures_usds.*` & `futures_coin.*`)
- Leverage config: `futures_usds.changeInitialLeverage` (capped at max 5x for agentic safety).
- Margin mode: `futures_usds.changeMarginType` (isolated vs cross).
- Position audit: `futures_usds.positionInformationV2`, `futures_usds.futuresAccountBalanceV3`.
- Order management: `futures_usds.newOrder`, `futures_usds.cancelOrder`.

### C. Margin Trading (`margin.*`)
- Health audit: `margin.crossMarginCollateralRatio`, `margin.queryCrossMarginAccountDetails`.
- Execution & liquidity: `margin.marginAccountNewOrder`, `margin.marginAccountBorrowRepay`.

### D. Zero-Slippage Convert (`convert.*`)
- Instant swap: `convert.sendQuoteRequest` -> `convert.acceptQuote`.
- Limit conversions: `convert.placeLimitOrder`, `convert.queryLimitOpenOrders`.

### E. Wallet & Sub-Accounts (`wallet.*` & `sub_account.*`)
- Balances: `spot.getAccount`, `wallet.queryUserWalletBalance`.
- Transfer routing: `wallet.userUniversalTransfer` (internal sub-account asset moves only).

---

## 2. Inviolable Security & Risk Rules
1. **Sub-Account Boundary**: All actions are strictly bounded to the user-authorized Binance Agentic Sub-Account. No external withdrawals.
2. **Pre-Trade Risk Verification**: Every non-read action must adhere to pre-configured max notional boundaries.
3. **Derivatives Safety**: Never exceed 5x leverage on any futures contract.
4. **Emergency Stop**: If triggered, purge open orders across spot, margin, and futures simultaneously.

---

## 3. Institutional Quantitative Alpha Strategies

The agent implements four institutional-grade trading strategies using live Binance Agent OS MCP tools:

### Strategy 1: Delta-Neutral Cash-and-Carry (Funding Rate Harvesting)
- **Concept**: Earn high annualized yield (typically 15%–45% APR) completely immune to market direction.
- **Workflow**:
  1. Scan `futures_usds.premiumIndexKlineData` across all pairs to rank highest funding rates.
  2. Buy $X of spot asset (`spot.newOrder`).
  3. Short exactly $X of 1x Perpetual Futures (`futures_usds.newOrder`).
  4. Net exposure = 0 (price up or down doesn't matter). Collect funding payments every 8 hours.

### Strategy 2: Bollinger Mean-Reversion Liquidity Sniping
- **Concept**: Buy extreme oversold flash wicks on majors and exit at mean reversion.
- **Workflow**:
  1. Calculate 20-period Bollinger Bands on `spot.klines`.
  2. If price dips below lower band AND `RSI-14 < 28`, initiate entry tranche.
  3. Automatically ladder take-profit limit orders at the 20-SMA baseline.

### Strategy 3: Spot-Quarterly Basis Convergence
- **Concept**: Exploits the premium between Spot and Quarterly Delivery futures (`futures_usds` delivery contracts).
- **Workflow**:
  1. Compare `spot.tickerPrice` against quarterly futures ticker.
  2. Buy undervalued spot, sell overvalued futures; hold until quarterly expiration where prices converge by definition to 0 spread.

### Strategy 4: Orderbook Absorption Wall Hunter
- **Concept**: Identifies institutional buy walls absorbing selling pressure before pumps.
- **Workflow**:
  1. Query top 50 bid/ask depth via `spot.depth`.
  2. Calculate `Bid Volume / Ask Volume` ratio within 1.5% of mid-price.
  3. If ratio > 2.0 and `analysis.getTokenAiReport` is positive, enter with tight stop behind the wall.
