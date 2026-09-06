# 🎯 Agent Interaction Examples

Real examples of Binance Sentinel-OS in action inside an AI client connected to Binance Agent OS MCP, showcasing smart micro-trade routing, exchange filter validation, and institutional execution.

---

## 1. Smart Micro-Trade Routing (< $5 USDT Auto-Convert)

**Prompt:**
```
Sentinel: Buy $2.50 of BNB.
```

**Agent Response:**
```
🔄 Smart Micro-Trade Routing Triggered
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Target Amount: $2.50 USDT

Exchange Rule Check:
  • Spot matching engine requires MIN_NOTIONAL of 5.00 USDT.
  • Auto-routing to Binance Convert (`convert.sendQuoteRequest`) to bypass orderbook limit.

Conversion Execution:
  • From: 2.50 USDT
  • To:   0.0042 BNB
  • Rate: 1 BNB = $595.20 USDT
  • Fees: 0 USDT (Zero-slippage conversion)

Quote accepted via `convert.acceptQuote`!
0.0042 BNB credited to sub-account instantly. ✅
```

---

## 2. Real-Time Market Intelligence Across Spot & Futures

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

## 5. Emergency Stop — All Products

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

---

## 7. Futures Minimum Contract Enforcement & Alternative Pair Substitution

**Prompt:**
```
Sentinel: Open a 3x long position on BTCUSDT futures with $15 USDT margin.
```

**Agent Response:**
```
⚠️ Futures Minimum Contract Requirement Detected
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Target Contract: BTCUSDT Perpetual (3x Leverage)
User Margin: $15.00 USDT (Total Purchasing Power: $45.00 USDT)

Exchange Rule Check (`futures_usds.exchangeInformation`):
  • BTCUSDT Minimum Lot Size: 0.001 BTC
  • At current BTC price ($67,500), minimum notional is $67.50 USDT.
  • Required Margin: ~$22.50 USDT.
  • Shortfall: $7.50 USDT.

💡 Intelligent Alternative Pair Recommendations:
Your $15 USDT margin easily satisfies lower-minimum liquid perpetual pairs:

1. **SOLUSDT** (Current Price: $135.20)
   • Min order: 0.1 SOL (~$13.52 notional → requires only $4.51 margin at 3x)
   • 24h Trend: +3.2% | RSI-14: 48.2 (Neutral Accumulation)

2. **DOGEUSDT** (Current Price: $0.125)
   • Min order: 100 DOGE (~$12.50 notional → requires only $4.17 margin at 3x)
   • 24h Trend: +1.8% | RSI-14: 51.0

Would you like me to execute a 3x long on **SOLUSDT** instead? [yes/no]
```

---

## 8. Delta-Neutral Funding Rate Arbitrage (Cash & Carry)

**Prompt:**
```
Sentinel: Scan for high funding rates and propose a delta-neutral cash-and-carry position.
```

**Agent Response:**
```
🌾 Delta-Neutral Funding Rate Scan
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Scanning `futures_usds.premiumIndexKlineData` across active perpetuals...

Top Opportunity:
  • Symbol: PEPEUSDT Perp
  • 8h Funding Rate: +0.048% (~52.5% Annualized APR)
  • Next Settlement: In 2 hours 14 mins

Strategy Execution Plan (Zero Directional Risk):
  1. Buy $50.00 PEPE on Spot (`spot.newOrder`)
  2. Open $50.00 1x Short on PEPEUSDT Perp (`futures_usds.newOrder`)
  3. Net Delta: 0.00 (Price changes cancel out perfectly)
  4. Estimated Yield: ~$0.072 USDT every 8 hours ($0.216/day) passive yield.

Confirm Delta-Neutral Execution? [yes/no]
```
