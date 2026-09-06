# Binance Sentinel-OS on Antigravity (AGY)

[![Binance Agent OS](https://img.shields.io/badge/Binance-Agent%20OS-F0B90B?style=for-the-badge&logo=binance&logoColor=black)](https://binance.com)
[![Antigravity](https://img.shields.io/badge/Antigravity-AGY%20Native-blue?style=for-the-badge)](https://antigravity.dev)
[![Model Context Protocol](https://img.shields.io/badge/MCP-Protocol-4B0082?style=for-the-badge)](https://modelcontextprotocol.io)
[![Hackathon Track](https://img.shields.io/badge/Hackathon-Track%20A%20Submission-00C087?style=for-the-badge)](#)

> **Autonomous, Risk-Guarded AI Trading Agent built directly inside Google Antigravity (AGY) utilizing the native Binance Agent OS MCP Server.**
> 
> *Submitted for the Binance Agent OS Mini Hackathon (Track A: Build an AI Agent).*

---

## 💡 Concept & Vision

Instead of writing another fragile custom bot or external script, **Binance Sentinel-OS** turns **Antigravity (AGY)** itself into an autonomous financial co-pilot.

By connecting AGY's advanced reasoning capabilities directly to Binance's official **Agent OS Model Context Protocol (MCP)** endpoint (`https://agent.binance.com/mcp/agentic`), the agent gains native access to 82+ Binance exchange tools:
- **Spot & Margin Trading**
- **Live Candlestick (Kline) & Price Feeds**
- **Isolated Sub-Account Asset Management**
- **Convert & AI Research Reports**

All actions are governed by an AGY **Agent Skill & Persona Mandate** with strict pre-trade guardrails.

---

## 🏛️ Architecture: Pure AGY Native Agent

```
┌─────────────────────────────────────────────────────────────┐
│                    USER INTERACTION IN AGY                  │
│       "Sentinel: Scan BNB 1h structure and check balance"   │
└──────────────────────────────┬──────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                  ANTIGRAVITY (AGY) RUNTIME                  │
│   • Multi-Turn Reasoning & Goal Execution                   │
│   • Custom Skill: `binance-sentinel` (Rules & Boundaries)   │
│   • Pre-Trade Guardrails (Max notional, isolated account)   │
└──────────────────────────────┬──────────────────────────────┘
                               │ JSON-RPC over MCP
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                 BINANCE AGENT OS MCP SERVER                 │
│   Official Endpoint: https://agent.binance.com/mcp/agentic  │
│   ├─ spot.tickerPrice                                       │
│   ├─ spot.klines                                            │
│   ├─ spot.getAccount                                        │
│   └─ spot.newOrder / deleteOpenOrders                       │
└──────────────────────────────┬──────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                     BINANCE SUB-ACCOUNT                     │
│   • Dedicated Agentic Sub-Account (Funded by User)          │
│   • No Withdrawal Permissions (100% Secure)                 │
└─────────────────────────────────────────────────────────────┘
```

---

## 📁 Repository Structure

```
binance-agent-os-agy/
├── .agents/
│   └── skills/
│       └── binance-sentinel/
│           └── SKILL.md       # Custom AGY Skill defining Sentinel behaviors
├── AGENTS.md                  # Operating Mandate & Risk Boundaries for AGY
├── DEMO_WALKTHROUGH.md        # Script and prompt flow for recording the demo
└── README.md                  # Hackathon submission documentation
```

---

## 🛡️ Risk & Security Model

1. **Zero Credential Leaks**: Antigravity uses OAuth/session authorization for the Binance Agent OS MCP server. No API keys or private keys are committed.
2. **Sub-Account Boundaries**: The agent operates exclusively inside an isolated Binance sub-account.
3. **Hard Trade Caps**: Orders are evaluated against maximum notional limits before execution.
4. **Emergency Stop**: One-command purge of all open orders via `spot.deleteOpenOrders`.

---

## 🎥 Video Demo Flow (For Judges)

1. **Activate Agent**: Open Antigravity and activate the `binance-sentinel` skill.
2. **Market Intelligence**: Prompt AGY to scan `BNBUSDT` ticker and 1h klines via `spot.tickerPrice` and `spot.klines`.
3. **Sub-Account Inspection**: Prompt AGY to check available balance via `spot.getAccount`.
4. **Guardrail Demonstration**: Attempt an order exceeding the trade cap to demonstrate safety rejection.
5. **Execution**: Place a safe, guarded limit order and cancel it to demonstrate the full lifecycle.

---

## 📜 License
MIT © 2026. Built for the Binance Agent OS Mini Hackathon.
