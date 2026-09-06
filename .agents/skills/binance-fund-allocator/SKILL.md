---
name: binance-fund-allocator
description: "Autonomous capital orchestration and sub-account treasury management for Binance Agent OS. Evaluates main vs. sub-account liquidity, manages cross-wallet transfers, and balances cash reserves dynamically."
---

# Binance Fund Allocator & Treasury Skill

Enables Antigravity to act as an autonomous treasury manager, ensuring the agentic sub-account has optimal working capital without exposing excessive funds to market volatility.

## Integrated MCP Tools
- `sub_account.getMainAccountAsset`: Query assets residing in the primary master account.
- `spot.getAccount`: Inspect available working capital in the active agentic sub-account.
- `wallet.userUniversalTransfer`: Secure internal asset movement between spot, margin, and futures sub-wallets.
- `wallet.queryUserUniversalTransferHistory`: Audit trail of internal balance reallocations.

## Treasury & Capital Protocols
1. **Working Capital Right-Sizing**:
   - Optimal operating buffer: $100.00 - $300.00 USDT in active sub-account.
   - If sub-account balance < $50.00 USDT, alert user to top up or initiate authorized internal allocation.
2. **Cross-Pocket Segregation**:
   - Isolate capital between Spot (long-term DCA holdings), Margin (tactical leverage), and Futures (short-term momentum).
   - Use `wallet.userUniversalTransfer` to shift realized profits back to Spot wallet automatically after trade close.
3. **Audit Ledger**:
   - Periodically inspect `wallet.queryUserUniversalTransferHistory` to verify all past movements are accounted for and no unauthorized transfers occurred.
