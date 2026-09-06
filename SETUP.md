# 🚀 Full Setup Guide — Works with All 5 Supported Platforms

Binance Sentinel-OS runs on the **Model Context Protocol (MCP)** standard.
Choose your preferred AI platform below to go from zero to live trading in under 10 minutes.

---

## 🧭 Step 0: Set Up Your Binance Account (All Platforms)

1. **Log in to Binance**: Go to [binance.com](https://www.binance.com).
2. **Enable Agent OS**: Visit [binance.com/agent-os](https://www.binance.com/agent-os).
3. **Create an Agentic Sub-Account**: Create an isolated sub-account at [Binance Sub-Account Management](https://www.binance.com/en/my/sub-account/management).
   > 🔒 *Security Invariant: The agent is sandboxed inside this sub-account with zero external withdrawal permissions.*
4. **Fund the Sub-Account**: Transfer USDT into the sub-account at [Sub-Account Transfer](https://www.binance.com/en/my/sub-account/asset-management/transfer?asset=USDT) ($10–$25 USDT is plenty for initial live trading).
5. **Clone this repository**:
   ```bash
   git clone https://github.com/favorian1/binance-sentinel-os.git
   cd binance-sentinel-os
   ```

---

## 🛠️ Setup for Your Specific AI Platform

Pick the client you are using:

---

### 1️⃣ Google Antigravity (AGY) ⭐ Recommended

1. **Install Skills**:
   ```bash
   cp -r .agents/skills/* ~/.agents/skills/
   ```
2. **Register the Binance Agent OS MCP Server**:
   ```bash
   agy mcp add --type http binance-agent-os https://agent.binance.com/mcp/agentic
   ```
3. **Launch AGY & Authenticate**:
   ```bash
   agy
   ```
   Type: `"Sentinel: Check my balance"`.
   On the first tool call, AGY opens your browser to log into Binance and approve your sub-account. The rules in `AGENTS.md` and the 9 skills load automatically.

---

### 2️⃣ Claude Code (Anthropic)

1. **Open your Claude MCP configuration**:
   - macOS / Linux: `~/.config/claude/claude_desktop_config.json`
   - Windows: `%APPDATA%\Claude\claude_desktop_config.json`
2. **Add the Binance Agent OS server**:
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
3. **Launch Claude Code**:
   ```bash
   claude
   ```
   Claude Code opens inside the `binance-sentinel-os` folder and **automatically reads `CLAUDE.md`**.
   Authenticate via the browser prompt when prompted.

---

### 3️⃣ Cursor AI

1. **Open the repository in Cursor**:
   ```bash
   cursor .
   ```
   Cursor **automatically reads `.cursorrules`** from the root of this project.
2. **Add the MCP Server**:
   - Go to **Cursor Settings (⚙️) → Features → MCP Servers → Add New Server**.
   - Set Name: `binance-agent-os`.
   - Set URL: `https://agent.binance.com/mcp/agentic`.
3. **Connect**:
   Click Connect, complete the Binance OAuth approval in your browser, and begin chatting in Cursor.

---

### 4️⃣ Windsurf / VS Code (Cascade & Copilot)

1. **Open the repository**:
   ```bash
   windsurf .
   # or
   code .
   ```
   - **Windsurf** automatically loads [`.windsurfrules`](./.windsurfrules).
   - **VS Code** automatically detects the pre-configured [`.vscode/settings.json`](./.vscode/settings.json) containing the Binance MCP endpoint.
2. **Authenticate**:
   Click the MCP status prompt in the bottom bar to authorize Binance OAuth.

---

### 5️⃣ ChatGPT / OpenAI Custom GPT

1. **Create a Custom GPT or Assistant** at [chatgpt.com/create](https://chatgpt.com/create).
2. **Set System Instructions**:
   Copy and paste the full contents of [`AGENT_INSTRUCTIONS.md`](./AGENT_INSTRUCTIONS.md) into the Instructions box.
3. **Add Action / MCP Tool**:
   Connect the schema pointing to `https://agent.binance.com/mcp/agentic`.

---

## ✅ Live Connection Test (All Platforms)

In your AI chat window, prompt the agent:

```text
Sentinel: Get the current price of BNBUSDT and check my sub-account balance.
```

**Expected Response**:
- Real-time BNB/USDT market price via `spot.tickerPrice`.
- Sub-account balances and free USDT via `spot.getAccount`.

Once both return real data, your agent is live and operational!

---

## 🎯 Sample Workflows

Refer to **[EXAMPLES.md](./EXAMPLES.md)** for production-tested workflows:
- **Smart Micro-Trade Routing**: Automatically routes orders $< \$5.00$ USDT to Binance Convert to bypass spot minimum limits.
- **Dynamic Exchange Compliance**: Uses `spot.exchangeInfo` to enforce official LOT_SIZE and MIN_NOTIONAL filters.
- **Institutional Whale Execution**: Slices orders $> \$100,000$ USDT using orderbook depth checks.
- **Derivatives Position Setup**: Configures isolated margin and leverage up to 5x.
- **Emergency Circuit Breaker**: Purges all open orders across Spot, Futures, and Margin in under 1 second.
