# 🎯 Agent Interaction Examples

Real examples of Binance Sentinel-OS in action inside an AI client connected to Binance Agent OS MCP.

---

## 1. Real-Time Market Intelligence

**Prompt:**
```
Sentinel: Analyze BNBUSDT across spot and futures. Show price, 24h change, mark price, and funding rate.
```

**Agent Response:**
```
📊 BNBUSDT Market Brief
━━━━━━━━━━━━━━━━━━━━━━
Spot Price:     $594.30  (+2.1% 24h)
Futures Mark:   $594.80  (+$0.50 premium to spot)
Funding Rate:   +0.010%  (neutral — no crowding)

Technical:
  EMA-9:  $591.40
  EMA-21: $588.70
  RSI-14: 52.3 (neutral)

Regime: Mild bullish momentum. Spot and futures aligned.
No strong entry signal — monitoring.
```

---

## 2. Sub-Account Portfolio Audit

**Prompt:**
```
Sentinel: Check my sub-account balance and show all holdings.
```

**Agent Response:**
```
🔒 Agent Sub-Account Holdings
━━━━━━━━━━━━━━━━━━━━━━━━━━━
  USDT   →  120.00  (free)
  BNB    →    0.05  (free) ≈ $29.72
  BTC    →    0.00  (none)

Total Value: ~$149.72 USDT
Active Open Orders: 0
```

---

## 3. Risk Guardrail — Oversized Order Blocked

**Prompt:**
```
Sentinel: Buy 50 BNB right now.
```

**Agent Response:**
```
🚨 Guardrail Violation — Execution Halted
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Order: BUY 50 BNB
Estimated Notional: ~$29,715 USDT
Policy Cap: $50 USDT per trade

Reason: Notional exceeds maximum per-trade ceiling.
Action: Order blocked before reaching Binance.

Suggestion: Split into smaller DCA entries within the $50 cap.
```

---

## 4. Safe Derivatives Position — 3x Leveraged Long

**Prompt:**
```
Sentinel: Set up a 3x isolated long position on BNBUSDT futures with a small test entry.
```

**Agent Response:**
```
📈 Futures Setup — BNBUSDT
━━━━━━━━━━━━━━━━━━━━━━━━━
✅ Margin type set: ISOLATED
✅ Leverage set: 3x (within 5x safety cap)
✅ No conflicting positions open

Placing limit long order...
✅ Order placed: BUY 0.02 BNB @ $590.00 LIMIT GTC
   Order ID: #1048391
   Status: NEW

Estimated liquidation price: $399.80 (32% buffer ✅)
```

---

## 5. Zero-Slippage Convert

**Prompt:**
```
Sentinel: Get a quote to convert 30 USDT to BNB instantly.
```

**Agent Response:**
```
🔄 Binance Convert Quote
━━━━━━━━━━━━━━━━━━━━━━
From: 30.00 USDT
To:   0.0504 BNB
Rate: 1 BNB = $595.24
Valid for: 15 seconds

Confirm conversion? (yes/no)
```

---

## 6. Emergency Stop — All Products

**Prompt:**
```
Sentinel: /stop — cancel all open orders immediately.
```

**Agent Response:**
```
🚨 Emergency Stop Triggered
━━━━━━━━━━━━━━━━━━━━━━━━━
Cancelling all open orders...

✅ Spot: 0 orders cancelled
✅ Futures (USDⓈ-M): 1 order cancelled (BNBUSDT #1048391)
✅ Margin: 0 orders cancelled

All clear. Agent standing by.
Total time: 1.2 seconds
```
