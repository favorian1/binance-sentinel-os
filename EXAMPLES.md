# 🎯 Agent Interaction Examples

Real examples of Binance Sentinel-OS in action inside an AI client connected to Binance Agent OS MCP, utilizing dynamic Binance exchange filter validation.

---

## 1. Real-Time Market Intelligence Across Spot & Futures

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
Exchange Filters: minNotional = 5.00 USDT | maxNotional = 5,000,000 USDT
```

---

## 2. Dynamic Exchange Filter Validation (Below Minimum Notional)

**Prompt:**
```
Sentinel: Buy 0.001 BNB on spot.
```

**Agent Response:**
```
⚠️ Order Below Binance Minimum Notional
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Order: BUY 0.001 BNB @ ~$594.30
Estimated Notional: $0.59 USDT

Exchange Rule: `spot.exchangeInfo` requires MIN_NOTIONAL of 5.00 USDT.
Action: Order held locally to prevent Binance rejection code -1013 (FILTER_FAILURE: MIN_NOTIONAL).

Recommendation: Increase quantity to at least 0.01 BNB (~$5.94 USDT).
```

---

## 3. High-Notional / Institutional Execution ($1,000,000 Order)

**Prompt:**
```
Sentinel: Buy $1,000,000 of BTC on spot.
```

**Agent Response:**
```
🐋 High-Notional Execution Audit
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Target Notional: $1,000,000 USDT (~14.85 BTC @ $67,300)

Exchange Rule Compliance:
  ✅ BTCUSDT MAX_NOTIONAL: $10,000,000 USDT (Pass)
  ✅ Available Sub-Account Balance: $1,250,000 USDT (Pass)

Orderbook Liquidity Audit (`spot.depth`):
  • Top 20 ask liquidity: $2,400,000 USDT
  • Single clip market execution would incur ~0.42% slippage ($4,200).

Execution Plan:
  To minimize slippage, slicing into 5 TWAP tranches of $200,000 over 10 minutes.
  Confirm execution of Tranche 1? [yes/no]
```

---

## 4. Safe Derivatives Position — 3x Leveraged Long

**Prompt:**
```
Sentinel: Set up a 3x isolated long position on BNBUSDT futures with an initial live entry.
```

**Agent Response:**
```
📈 Futures Setup — BNBUSDT
━━━━━━━━━━━━━━━━━━━━━━━━━
✅ Margin type set: ISOLATED
✅ Leverage set: 3x (within exchange bracket)
✅ Contract Filters (`futures_usds.exchangeInformation`):
   • minNotional: 5.00 USDT
   • stepSize: 0.01 BNB

Placing limit long order...
✅ Order placed: BUY 0.05 BNB @ $590.00 LIMIT GTC
   Order ID: #1048391
   Status: NEW

Estimated liquidation price: $399.80 (32% buffer ✅)
```

---

## 5. Zero-Slippage Convert

**Prompt:**
```
Sentinel: Get a quote to convert 500 USDT to BNB instantly.
```

**Agent Response:**
```
🔄 Binance Convert Quote
━━━━━━━━━━━━━━━━━━━━━━
From: 500.00 USDT
To:   0.8403 BNB
Rate: 1 BNB = $595.02
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
Total time: 0.8 seconds
```
