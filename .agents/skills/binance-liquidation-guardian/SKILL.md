---
name: binance-liquidation-guardian
description: "Active liquidation monitoring and defensive deleveraging engine for Binance Agent OS. Tracks liquidation price proximity, collateral margin buffers, and automatically trims open exposure before danger thresholds."
---

# Binance Liquidation Guardian Skill

Acts as a continuous safety monitor protecting agentic futures and margin accounts from cascading liquidations or forced closures.

## Integrated MCP Tools
- `futures_usds.positionInformationV2`: Real-time position sizing, entry price, mark price, and liquidation price.
- `futures_coin.positionInformation`: Coin-margined contract liquidation parameters.
- `margin.crossMarginCollateralRatio`: Margin collateral ratio and margin call warning thresholds.
- `margin.queryMaxBorrow`: Maximum borrowable headroom before risk escalation.
- `futures_usds.newOrder` / `futures_coin.newOrder`: Defensive reduction orders (Reduce-Only).

## Defensive Protocols
1. **Liquidation Distance Monitoring**:
   - For every open contract, calculate the Liquidation Distance:
     `Distance % = abs(Mark Price - Liquidation Price) / Mark Price * 100%`
   - **Green Zone (> 25%)**: Safe. Normal operations permitted.
   - **Yellow Alert (15% - 25%)**: Caution. Halt new position sizing on the symbol.
   - **Red Alert (< 15%)**: Immediate defensive reduction.
2. **Autonomous Deleverage Trigger**:
   - If Distance % drops below 15%, emit a defensive market or limit order with `reduceOnly: true` to trim 50% of the position size.
   - Prevent margin liquidations before exchange liquidators trigger liquidation penalty fees.
3. **Cross-Margin Health Floor**:
   - If `margin.crossMarginCollateralRatio` drops below 1.8 (caution zone), initiate automated repayment using `margin.marginAccountBorrowRepay`.
