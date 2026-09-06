# 🚀 Full Setup Guide — From Zero to Running Binance Sentinel-OS

This guide starts from scratch. Even if you have never used an AI coding tool or MCP before, follow these steps and you will have a live Binance AI agent running in under 15 minutes.

---

## 🧭 Overview — What You Are Setting Up

```
Binance Account  +  AI Client (AGY / Claude Code / Cursor)  +  This Repo
        ↓                        ↓                                  ↓
  Your sub-account         The AI brain                    The trading rules
  where trades happen      that talks to Binance            & strategies
```

You need **three things**:
1. A Binance account with an isolated sub-account (where your funds live safely)
2. An AI client installed on your computer (pick one: AGY, Claude Code, or Cursor)
3. This repository cloned to your machine (the 9-skill suite and rules)

---

## PART 1 — Set Up Your Binance Account (5 Minutes)

### Step 1.1 — Create or Log In to Binance
Go to [binance.com](https://www.binance.com) and create an account if you don't have one.
Complete identity verification (KYC) if required.

### Step 1.2 — Enable Binance Agent OS
Go to [binance.com/agent-os](https://www.binance.com/agent-os) and enable Agent OS on your account.
This unlocks the ability for AI clients to connect to your Binance exchange.

### Step 1.3 — Create an Agentic Sub-Account
Go to [Binance Sub-Account Management](https://www.binance.com/en/my/sub-account/management) and create a new sub-account dedicated to the AI agent.

> ⚠️ **Important**: This sub-account is isolated from your main account. The AI agent can ONLY trade within it — it cannot withdraw funds or touch your main account.

### Step 1.4 — Fund the Sub-Account
Transfer some USDT into the sub-account at [Sub-Account Transfer](https://www.binance.com/en/my/sub-account/asset-management/transfer?asset=USDT).
Start with $10–$20 USDT for initial live agent trading.

✅ **Binance is ready.**

---

## PART 2 — Install Your AI Client (5 Minutes)

Pick ONE of the following. All three work with this project.

---

### Option A — Google Antigravity (AGY) ⭐ Recommended

**Step 2A.1 — Download and install AGY**

Go to [antigravity.dev](https://antigravity.dev), download the installer for your OS (Windows / macOS / Linux) and install it.

**Step 2A.2 — Create an AGY account**

Open AGY after installation. Sign in with your Google account when prompted.

**Step 2A.3 — Confirm AGY is working**
```bash
agy --version
```
You should see a version number printed. ✅

---

### Option B — Claude Code (Anthropic)

**Step 2B.1 — Create an Anthropic account**

Go to [claude.ai](https://claude.ai) and sign up for an account.

**Step 2B.2 — Install Claude Code**

```bash
# macOS / Linux
npm install -g @anthropic-ai/claude-code

# Then log in
claude login
```

**Step 2B.3 — Confirm Claude Code is working**
```bash
claude --version
```
You should see a version number printed. ✅

---

### Option C — Cursor AI

**Step 2C.1 — Download and install Cursor**

Go to [cursor.com](https://cursor.com), download the installer for your OS and install it.

**Step 2C.2 — Create a Cursor account**

Open Cursor and sign up with your email or GitHub account when prompted.

**Step 2C.3 — Confirm Cursor is working**

Cursor opens as a code editor. You should see the AI chat panel on the right side. ✅

---

## PART 3 — Clone This Repository (1 Minute)

Open your terminal (or Cursor's built-in terminal) and run:

```bash
git clone https://github.com/favorian1/binance-sentinel-os.git
cd binance-sentinel-os
```

Confirm it worked:
```bash
ls
```
You should see: `README.md`, `SETUP.md`, `EXAMPLES.md`, `AGENTS.md`, `.agents/`  ✅

---

## PART 4 — Connect Binance Agent OS MCP to Your AI Client

> MCP (Model Context Protocol) is the bridge that lets your AI client talk directly to Binance.
> You do this once and it stays connected.

---

### If you chose AGY:

**Step 4A.1 — Install the 9 Binance Sentinel skills**
```bash
cp -r .agents/skills/* ~/.agents/skills/
```

Confirm:
```bash
ls ~/.agents/skills/ | grep binance
```
Should list all 9 `binance-*` skills. ✅

**Step 4A.2 — Register the Binance Agent OS MCP Server in AGY**
```bash
agy mcp add --transport http binance-agent-os https://agent.binance.com/mcp/agentic
```

Confirm it is registered:
```bash
agy mcp list
```
You should see `binance-agent-os` listed as `enabled`. ✅

**Step 4A.3 — Authenticate with Binance**

Open AGY:
```bash
agy
```
Ask AGY any Binance question (e.g. *"Sentinel: Check my balance"*).
On the very first tool call, AGY will automatically open a browser window:
1. Log in with your Binance account.
2. Select your isolated **Agentic Sub-Account**.
3. Click **Approve**.

The session token is securely saved by AGY. You are now live! ✅

---

### If you chose Claude Code:

**Step 4B.1 — Open your Claude MCP config file**

- macOS / Linux: `~/.config/claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

If the file does not exist, create it.

**Step 4B.2 — Add the Binance MCP server**

Paste this into the file:
```json
{
  "mcpServers": {
    "binance-agent-os": {
      "type": "remote",
      "url": "https://agent.binance.com/mcp/agentic",
      "auth": { "type": "oauth" }
    }
  }
}
```

**Step 4B.3 — Restart Claude Code and authenticate**
```bash
claude
```
Claude Code will prompt you to authorize Binance in your browser → log in → approve. ✅

**Step 4B.4 — Load the Sentinel rules**

Start a new Claude Code session and paste the full contents of `.agents/skills/binance-sentinel/SKILL.md` as your system prompt or project instruction. ✅

---

### If you chose Cursor:

**Step 4C.1 — Open Cursor Settings → MCP**

In Cursor: go to **Settings (⚙️) → Features → MCP Servers → Add New Server**

**Step 4C.2 — Add the Binance MCP server**
```json
{
  "binance-agent-os": {
    "type": "remote",
    "url": "https://agent.binance.com/mcp/agentic",
    "auth": { "type": "oauth" }
  }
}
```
Save and restart Cursor. Authorize with your Binance account when prompted. ✅

**Step 4C.3 — Load the Sentinel rules**

Create a `.cursorrules` file in the project root:
```bash
cp .agents/skills/binance-sentinel/SKILL.md .cursorrules
```
✅

---

## PART 5 — Verify Everything is Working (All Clients)

Open your AI client and type this exact prompt:

```
Sentinel: Get the current price of BNBUSDT and show my sub-account balance.
```

**What you should see:**
- Live BNB price pulled from `spot.tickerPrice`
- Your sub-account USDT balance from `spot.getAccount`

If both appear — **your Binance AI agent is fully live!** 🎉

---

## PART 6 — Explore Agent Capabilities

Check out **[EXAMPLES.md](./EXAMPLES.md)** for sample prompts and outputs:

- 📊 Multi-market technical analysis (Spot, Futures, and funding rates)
- 🔄 Smart micro-trade routing (< $5 USDT auto-routes to Binance Convert)
- 🛡️ Dynamic exchange filter validation (LOT_SIZE, stepSize, minNotional)
- 📈 Safe derivatives position with isolated leverage
- 🚨 Emergency stop cancelling all open orders across products instantly

---

## 🔧 Troubleshooting

| Problem | Fix |
| :--- | :--- |
| `agy: command not found` | Install AGY from [antigravity.dev](https://antigravity.dev) |
| `claude: command not found` | Run `npm install -g @anthropic-ai/claude-code` |
| `binance-agent-os` not in `agy mcp list` | Run `agy mcp add --transport http binance-agent-os https://agent.binance.com/mcp/agentic` |
| Browser does not open during auth | Copy the auth URL from terminal and open manually in browser |
| Sub-account balance shows 0 | Transfer USDT at [Binance Sub-Account Transfer](https://www.binance.com/en/my/sub-account/asset-management/transfer?asset=USDT) |
| Rate limit error `-1003` | Wait 30–60 seconds and retry the prompt |
| Skills not appearing in AGY | Restart AGY after running the `cp -r` command |
