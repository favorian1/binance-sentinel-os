# Binance Sentinel-OS — Hackathon Video Demo Script

> Record 1–3 minutes showing the agent live in Antigravity.

---

### Phase 1: Intro (0:00 – 0:20)
**Screen**: Show AGY + the GitHub repo open side by side.

> *"This is Binance Sentinel-OS — an autonomous full-stack trading agent built directly inside Google Antigravity using the official Binance Agent OS MCP Server. No external bot code, no API wrappers — just AI and MCP."*

---

### Phase 2: Multi-Product Market Intelligence (0:20 – 1:00)
**Prompt to AGY:**
> `"Sentinel: Analyze BNBUSDT across spot and futures. Show mark price, funding rate, and RSI."`

**What AGY does:**
- Calls `spot.ticker24hr` → Spot price + 24h change.
- Calls `futures_usds.symbolPriceTicker` → Perp mark price.
- Calls `futures_usds.premiumIndexKlineData` → Funding rate.
- Calls `spot.klines` → Computes RSI-14 and EMA 9/21 trend.
- Synthesizes into a unified market brief.

---

### Phase 3: Sub-Account & Portfolio Audit (1:00 – 1:20)
**Prompt to AGY:**
> `"Sentinel: Show my sub-account balances and margin health."`

**What AGY does:**
- Calls `spot.getAccount` → Available USDT and assets.
- Calls `margin.crossMarginCollateralRatio` → Margin health ratio.
- Calls `wallet.queryUserWalletBalance` → Full wallet picture.

---

### Phase 4: Risk Guardrail Demonstration (1:20 – 1:45)
**Prompt to AGY:**
> `"Sentinel: Open a 20x BNB futures long."`

**What AGY does:**
- Evaluates leverage against hard 5x cap.
- **Rejects** with message:
  > *"🚨 Guardrail Violation: 20x exceeds maximum 5x leverage cap. Execution halted."*

---

### Phase 5: Safe Futures Execution (1:45 – 2:15)
**Prompt to AGY:**
> `"Sentinel: Set 3x isolated leverage on BNBUSDT futures and place a small limit long, then cancel it."`

**What AGY does:**
- Calls `futures_usds.changeMarginType` → ISOLATED.
- Calls `futures_usds.changeInitialLeverage` → 3x.
- Calls `futures_usds.newOrder` → Limit long placed.
- Calls `futures_usds.cancelOrder` → Order cancelled.

---

### Phase 6: Zero-Slippage Convert (2:15 – 2:35)
**Prompt to AGY:**
> `"Sentinel: Get a quote to convert 20 USDT to BNB instantly."`

**What AGY does:**
- Calls `convert.sendQuoteRequest` → Quote returned with rate and expiry.
- Shows user the quote for approval.

---

### Phase 7: Emergency Stop (2:35 – 2:50)
**Prompt to AGY:**
> `"Sentinel: /stop — emergency cancel all orders."`

**What AGY does:**
- Calls `spot.deleteOpenOrders` → Spot cleared.
- Calls `futures_usds.currentAllOpenOrders` + `futures_usds.cancelOrder` → Futures cleared.
- Calls `margin.marginAccountCancelAllOpenOrdersOnASymbol` → Margin cleared.
- Reports: *"All open orders across Spot, Futures, and Margin cancelled. Agent standing down."*

---

### Phase 8: Wrap-up (2:50 – 3:00)
> *"Binance Sentinel-OS showcases the full power of Binance Agent OS: one AI agent managing Spot, Futures, Margin, Convert, and Wallet — all in real-time, all safely bounded. Code is open-source on GitHub. Thank you!"*
