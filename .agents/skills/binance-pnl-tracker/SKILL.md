---
name: binance-pnl-tracker
description: "Real-time performance audit, trade attribution, and P&L analytics engine for Binance Agent OS. Pulls spot and margin trade histories, daily account snapshots, and mark-to-market positions to compute net ROI and win rate."
---

# Binance P&L Tracker & Performance Attribution Skill

Provides Antigravity with quantitative auditing capabilities to evaluate historical trade performance, calculate realized vs. unrealized P&L, and track portfolio drawdowns.

## Integrated MCP Tools
- `spot.myTrades`: Granular trade execution logs (commission, realized price, timestamp, order ID).
- `margin.queryMarginAccountsTradeList`: Margin trade executions and borrowing costs.
- `wallet.dailyAccountSnapshot`: Historical daily sub-account equity snapshots (30-day tracking).
- `futures_usds.positionInformationV2`: Unrealized P&L (unPnl) and entry prices for open derivative positions.

## Analytical Protocols
1. **Realized Trade Audit**:
   - Query `spot.myTrades` for active pairs over the target evaluation window.
   - Aggregate buy volume vs. sell volume to compute net realized profit in USDT.
   - Calculate trade fees paid in BNB/USDT to derive net post-fee returns.
2. **Equity Curve & Max Drawdown**:
   - Fetch `wallet.dailyAccountSnapshot` to map the 7-day and 30-day sub-account equity trajectory.
   - Compute Peak-to-Trough drawdown: `(Peak - Current) / Peak * 100%`.
   - Alert the risk engine if drawdown breaches the 8% caution threshold.
3. **Executive Performance Report**:
   - Synthesize Total Realized P&L, Open Position unPnl, Win Rate %, and Profit Factor into a clear user summary.
