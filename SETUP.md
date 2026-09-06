# 🚀 Setup Guide — Run Binance Sentinel-OS in Under 10 Minutes

Follow these steps exactly. Each step has a confirmation check so you know it worked before moving to the next.

---

## What You Need Before Starting

| Requirement | Where to Get It |
| :--- | :--- |
| **Google Antigravity (AGY) CLI** | Download at [antigravity.dev](https://antigravity.dev) |
| **Binance Account** with Agent OS enabled | Enable at [binance.com/agent-os](https://www.binance.com/agent-os) |
| **Binance Agentic Sub-Account** | Create one at [Binance Sub-Accounts](https://www.binance.com/en/my/sub-account/management) |
| **Funded Sub-Account** (any amount of USDT) | Transfer at [binance.com Sub-Account Transfer](https://www.binance.com/en/my/sub-account/asset-management/transfer) |

---

## ✅ Step 1 — Clone the Repository

Open your terminal and run:

```bash
git clone https://github.com/favorian1/binance-sentinel-os.git
cd binance-sentinel-os
```

**How to confirm it worked:**
```bash
ls
```
You should see: `README.md`, `SETUP.md`, `DEMO_WALKTHROUGH.md`, `.agents/`

---

## ✅ Step 2 — Install the 5 Binance Sentinel Skills into Antigravity

```bash
cp -r .agents/skills/* ~/.agents/skills/
```

**How to confirm it worked:**
```bash
ls ~/.agents/skills/ | grep binance
```
You should see all 5 skills listed:
```
binance-derivatives-engine
binance-market-intelligence
binance-portfolio-rebalancer
binance-risk-guardrails
binance-sentinel
```

---

## ✅ Step 3 — Authenticate Binance Agent OS MCP

Make sure you are **logged into your Binance account** in your browser first, then run:

```bash
agy mcp auth binance-agent-os
```

**What happens:**
1. A browser window or authorization URL opens.
2. Log in with the Binance account that has an Agentic Sub-Account.
3. Approve the **Agentic Sub-Account** scope when prompted.
4. You will see a success confirmation in the terminal.

> ⚠️ **Use an isolated Binance Sub-Account — never your main account.**
> The agent has NO withdrawal permissions by design.

---

## ✅ Step 4 — Open Antigravity and Verify the Connection

Open AGY in your terminal:

```bash
agy
```

Then type this prompt to verify the MCP is live:

```
Sentinel: Get the current price of BNBUSDT.
```

**What to expect:**
AGY calls `spot.tickerPrice` via Binance Agent OS and returns the live BNB price — something like:
```
BNB/USDT: $594.30
```

If you see a real price — **the agent is live and connected!** 🎉

---

## ✅ Step 5 — Check Your Sub-Account Balance

```
Sentinel: Check my sub-account balance and show available funds.
```

**What to expect:**
AGY calls `spot.getAccount` and returns your sub-account assets and free USDT balance.

---

## ✅ Step 6 — Run the Full Demo

Follow the complete prompt-by-prompt script in **[DEMO_WALKTHROUGH.md](./DEMO_WALKTHROUGH.md)** to experience:

- 📊 Market intelligence across Spot, Futures & funding rates
- 🛡️ Risk guardrail interception (watch AGY reject a 20x leverage request)
- 📈 Derivatives setup (3x isolated leverage on BNB futures)
- 🔄 Zero-slippage Convert (USDT → BNB instantly)
- 🚨 Emergency stop across Spot, Futures & Margin simultaneously

---

## How It Works Under The Hood

```
You type a prompt in AGY
         ↓
AGY reads the Binance Sentinel skill rules (from .agents/skills/)
         ↓
AGY reasons about which Binance tool to call and in what order
         ↓
Binance Agent OS MCP executes the tool against your sub-account
         ↓
AGY synthesizes the result and responds in plain language
```

> No external servers. No API keys in code. No bots running in the background.
> **The agent IS Antigravity, guided by the skill files in this repository.**

---

## 🔧 Troubleshooting

| Problem | Fix |
| :--- | :--- |
| `Unauthorized` on first tool call | Re-run `agy mcp auth binance-agent-os` and re-approve |
| Browser does not open during auth | Copy the URL from the terminal and open it manually |
| Sub-account balance shows 0 | Transfer funds at [Sub-Account Transfer](https://www.binance.com/en/my/sub-account/asset-management/transfer) |
| `Permission denied` on a tool | Enable Spot/Futures/Margin Trading on your Binance API key settings |
| Rate limit error code `-1003` | Wait 30–60 seconds and retry the prompt |
| Skills not appearing in AGY | Restart AGY after copying the skill files |
