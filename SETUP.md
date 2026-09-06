# Setup Guide — How to Run Binance Sentinel-OS on Antigravity

This guide shows you how to install and run the Binance Sentinel-OS agent on your own machine in under 10 minutes.

---

## Prerequisites

| Requirement | Where to Get It |
| :--- | :--- |
| **Google Antigravity (AGY)** CLI installed | [antigravity.dev](https://antigravity.dev) |
| **Binance Account** with Agent OS access | [binance.com/agent-os](https://www.binance.com/agent-os) |
| **Binance Agentic Sub-Account** funded | [Transfer funds here](https://www.binance.com/en/my/sub-account/asset-management/transfer) |

---

## Step 1: Clone the Repository

```bash
git clone https://github.com/favorian1/binance-sentinel-os.git
cd binance-sentinel-os
```

---

## Step 2: Install the Binance Sentinel Skills into Antigravity

Copy the agent skill suite into your Antigravity skills directory:

```bash
cp -r .agents/skills/* ~/.agents/skills/
```

This installs all 5 specialized skills into AGY:
- `binance-sentinel` — Master operating mandate
- `binance-market-intelligence` — Technical analysis across all products
- `binance-risk-guardrails` — Pre-trade safety engine
- `binance-portfolio-rebalancer` — Balance audit & DCA planning
- `binance-derivatives-engine` — Futures, Margin & Convert execution

---

## Step 3: Connect Binance Agent OS MCP to Antigravity

In your terminal, authenticate the Binance Agent OS MCP server:

```bash
agy mcp auth binance-agent-os
```

This opens a browser window to authorize your Binance account via OAuth. Log in and approve the Agentic Sub-Account scope.

> ⚠️ **Important**: Use an isolated Binance Sub-Account, not your main account.
> Fund it at [binance.com/en/my/sub-account/asset-management/transfer](https://www.binance.com/en/my/sub-account/asset-management/transfer).

---

## Step 4: Verify Connection

Open Antigravity and test the connection with a simple prompt:

```
Sentinel: Fetch real-time price for BNBUSDT.
```

AGY should respond by calling `spot.tickerPrice` via the Binance Agent OS MCP and return the live price.

---

## Step 5: Run the Full Demo

Follow the step-by-step prompt guide in [`DEMO_WALKTHROUGH.md`](./DEMO_WALKTHROUGH.md) to experience the full agent capabilities:

- Market intelligence across Spot & Futures
- Sub-account balance audit
- Risk guardrail interception
- Derivatives position setup (3x leverage, ISOLATED)
- Zero-slippage Convert
- Emergency stop across all products

---

## How It Works (For Judges)

```
You type a prompt in AGY
        ↓
AGY reads the Binance Sentinel skill rules
        ↓
AGY decides which Binance MCP tool to call
        ↓
Binance Agent OS MCP executes against your sub-account
        ↓
AGY synthesizes the result and responds
```

No external servers. No API keys in code. No bots running in the background.
**The agent IS Antigravity, guided by the skills in this repository.**

---

## Troubleshooting

| Issue | Solution |
| :--- | :--- |
| `Unauthorized` error on MCP | Re-run `agy mcp auth binance-agent-os` |
| Sub-account balance is 0 | Transfer funds via Binance sub-account management |
| Tool returns permission error | Enable Spot/Futures Trading permissions on your Binance API key |
| Rate limit error (-1003) | Wait 30–60 seconds before retrying |
