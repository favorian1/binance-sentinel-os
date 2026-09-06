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
