# 🚀 Setup Guide — Works with AGY, Claude Code, Cursor, VS Code & More

Binance Sentinel-OS is built on the **Model Context Protocol (MCP)** — an open standard.
This means it works with **any MCP-compatible AI client**, not just Antigravity.

---

## What You Need (For Any Client)

| Requirement | Where to Get It |
| :--- | :--- |
| **Binance Account** with Agent OS enabled | [binance.com/agent-os](https://www.binance.com/agent-os) |
| **Binance Agentic Sub-Account** (isolated) | [Create one here](https://www.binance.com/en/my/sub-account/management) |
| **Sub-Account funded** (any USDT amount) | [Transfer here](https://www.binance.com/en/my/sub-account/asset-management/transfer?asset=USDT) |
| **Your preferred AI client** (see below) | AGY / Claude Code / Cursor / VS Code |

---

## Choose Your AI Client

### Option A — Google Antigravity (AGY)

**Step 1: Clone the repo**
```bash
git clone https://github.com/favorian1/binance-sentinel-os.git
cd binance-sentinel-os
```

**Step 2: Install the Binance Sentinel skills**
```bash
cp -r .agents/skills/* ~/.agents/skills/
```
Confirm it worked:
```bash
ls ~/.agents/skills/ | grep binance
# Should list all 5 binance-* skills
```

**Step 3: Authenticate Binance Agent OS MCP**
```bash
agy mcp auth binance-agent-os
```
A browser window opens → Log in to your Binance sub-account → Approve Agent OS scope.

**Step 4: Verify it's live**
Open AGY and type:
```
Sentinel: Get the current price of BNBUSDT.
```
You should see a live BNB price returned. ✅

---

### Option B — Claude Code (Anthropic)

**Step 1: Clone the repo**
```bash
git clone https://github.com/favorian1/binance-sentinel-os.git
cd binance-sentinel-os
```

**Step 2: Add Binance Agent OS MCP to Claude Code**

Edit your Claude Code MCP config file:
- **macOS/Linux**: `~/.config/claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

Add this entry:
```json
{
  "mcpServers": {
    "binance-agent-os": {
      "type": "remote",
      "url": "https://agent.binance.com/mcp/agentic",
      "auth": {
        "type": "oauth"
      }
    }
  }
}
```

**Step 3: Restart Claude Code and authenticate**

When Claude Code restarts, it will prompt you to authorize the Binance Agent OS OAuth connection. Log in with your Binance sub-account.

**Step 4: Load the Sentinel system prompt**

Copy the contents of `.agents/skills/binance-sentinel/SKILL.md` and paste it as a **System Prompt** or **Project Instruction** in your Claude Code project settings.

**Step 5: Verify**
In Claude Code, type:
```
Sentinel: Get the current price of BNBUSDT.
```
You should see a live BNB price returned. ✅

---

### Option C — Cursor AI

**Step 1: Clone the repo**
```bash
git clone https://github.com/favorian1/binance-sentinel-os.git
cd binance-sentinel-os
```

**Step 2: Add Binance Agent OS MCP to Cursor**

Go to **Cursor Settings → MCP → Add Server** and enter:

```json
{
  "binance-agent-os": {
    "type": "remote",
    "url": "https://agent.binance.com/mcp/agentic",
    "auth": { "type": "oauth" }
  }
}
```

**Step 3: Authenticate**

Cursor will prompt you to authorize via Binance OAuth. Use your Binance sub-account.

**Step 4: Add the Sentinel rules as a Cursor Rule**

Copy the content of `.agents/skills/binance-sentinel/SKILL.md` into a `.cursorrules` file in the project root:
```bash
cp .agents/skills/binance-sentinel/SKILL.md .cursorrules
```

**Step 5: Verify**
In Cursor chat:
```
Sentinel: Get the current price of BNBUSDT.
```
✅

---

### Option D — VS Code (with MCP Extension)

**Step 1: Install the MCP extension for VS Code**

Search for **"MCP Client"** or **"Model Context Protocol"** in the VS Code Extensions marketplace and install it.

**Step 2: Configure the Binance Agent OS server**

Open `.vscode/settings.json` in your project and add:
```json
{
  "mcp.servers": {
    "binance-agent-os": {
      "type": "remote",
      "url": "https://agent.binance.com/mcp/agentic",
      "auth": { "type": "oauth" }
    }
  }
}
```

**Step 3: Authenticate**

Follow the OAuth popup to connect your Binance sub-account.

**Step 4: Load Sentinel instructions**

Open the MCP extension panel and paste the contents of `.agents/skills/binance-sentinel/SKILL.md` as the system context.

---

### Option E — Any Other MCP-Compatible Client

The Binance Agent OS MCP server endpoint is:
```
https://agent.binance.com/mcp/agentic
```
Authentication: **OAuth 2.0** (log in with your Binance account)

1. Add the above URL as a **remote MCP server** in your client settings.
2. Authenticate with your Binance Agentic Sub-Account.
3. Load the strategy rules from `.agents/skills/binance-sentinel/SKILL.md` as a system prompt.
4. Start prompting!

---

## ✅ Quick Connection Test (All Clients)

Once connected, run this prompt regardless of which client you use:

```
Sentinel: Get the current price of BNBUSDT and check my sub-account balance.
```

**Expected response:**
- Live BNB price from `spot.tickerPrice`
- Sub-account balance from `spot.getAccount`

If both return real data — **you are fully connected and ready.** 🎉

---

## 🔧 Troubleshooting

| Problem | Fix |
| :--- | :--- |
| `Unauthorized` / auth failed | Re-authenticate Binance OAuth in your client |
| Browser does not open | Copy the auth URL from terminal and open manually |
| Sub-account balance is 0 | [Transfer USDT to sub-account](https://www.binance.com/en/my/sub-account/asset-management/transfer) |
| `Permission denied` on a tool | Enable Spot/Futures/Margin permissions in Binance API settings |
| Rate limit error `-1003` | Wait 30–60 seconds and retry |
| Skills not loading in AGY | Restart AGY after running `cp -r .agents/skills/* ~/.agents/skills/` |

---

## 📖 Next Step

Follow the **[DEMO_WALKTHROUGH.md](./DEMO_WALKTHROUGH.md)** for the full 8-phase interaction demo across Spot, Futures, Margin, Convert, and emergency stop — works with any of the clients above.
