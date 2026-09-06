# Binance Sentinel-OS — Antigravity Demo Walkthrough

This guide provides the exact prompts to showcase the agent in action during your video recording.

---

### Phase 1: Setup & Overview (0:00 - 0:30)
- **Visual**: Show Antigravity (AGY) open with the active conversation.
- **Explain**:
  > *"This is Binance Sentinel-OS. We built an autonomous trading sentinel directly inside Antigravity (AGY) using Binance's official Agent OS MCP Server, without writing external bot code."*

---

### Phase 2: Live Market Intelligence (0:30 - 1:15)
- **Prompt to AGY**:
  > `"Sentinel: Fetch real-time market data for BNBUSDT and analyze the trend."`
- **What Happens**:
  - AGY calls `spot.tickerPrice` and `spot.klines` via `binance-agent-os`.
  - AGY reports current price, 24h trend, and technical levels.

---

### Phase 3: Sub-Account Status (1:15 - 1:45)
- **Prompt to AGY**:
  > `"Sentinel: Check my sub-account balances and report available USDT."`
- **What Happens**:
  - AGY calls `spot.getAccount` via `binance-agent-os`.
  - AGY displays sub-account assets safely without exposing private data.

---

### Phase 4: Risk Guardrail & Order Safety (1:45 - 2:30)
- **Prompt to AGY**:
  > `"Sentinel: Place a buy order for 50 BNB."`
- **What Happens**:
  - AGY evaluates the request against the pre-trade risk policy in `SKILL.md`.
  - AGY rejects the oversized order:
    > *"Guardrail Violation: Order exceeds the $50 USDT notional cap. Action halted."*
- **Follow-up Prompt**:
  > `"Sentinel: Place a safe test limit buy for 0.02 BNB at $500, then cancel it."`
- **What Happens**:
  - AGY calls `spot.newOrder` to place the limit order.
  - AGY calls `spot.deleteOpenOrders` to cancel it.

---

### Phase 5: Wrap-up (2:30 - 3:00)
- **Explain**:
  > *"Binance Sentinel-OS showcases how Antigravity plus Binance Agent OS creates a secure, autonomous agentic workflow for CeFi trading. The code and rules are open-sourced on GitHub."*
