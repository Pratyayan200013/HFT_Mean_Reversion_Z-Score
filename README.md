Quantitative Trading Strategy: Mean Reversion with Z-Score Optimization  
Objective: Develop a robust mean-reversion strategy using statistical Z-scores to identify overbought/oversold conditions, optimized for high-frequency trading (HFT) with strict risk management.

Key Components  
1. Data & Features:
   - Data Source: Historical OHLCV data for Reliance (RELIANCE.NS) fetched via yfinance.
   - Typical Price: (High + Low + Close) / 3 to reduce noise.
   - Z-Score: (Typical Price − Rolling Mean) / Rolling Std to quantify deviations.

2. Strategy Logic:
   - Entry Signals:
     - Long: Z-score < -entry_threshold (oversold).
     - Short: Z-score > entry_threshold (overbought).
   - Exit Signals:
     - Z-score reverts to [-exit_threshold, exit_threshold].
     - Stop-Loss: stop_loss% to limit losses.
     - Profit Target: profit_target% to lock gains (1:20 risk-reward in final params).

3. Risk Management:
   - Concurrent Positions: Multiple trades allowed, each managed independently.
   - Transaction Costs: ₹0.1% per trade to simulate slippage and fees.

---

 Parameter Optimization  
- Grid Search: Tested 2,520 combinations over 7 parameters:
  param_grid = {
      'window': [10, 15, ..., 40],
      'entry_threshold': [1.0, 1.5, 2.0],
      'profit_target': [0.01, 0.02, ..., 0.1],
      'stop_loss': [0.005, 0.01, ..., 0.03]
  }
    
- Best Parameters:
  - window=40, entry_threshold=1.0, profit_target=0.1 (10%), stop_loss=0.005 (0.5%).
  - Risk-Reward: 1:20 (0.5% loss vs. 10% gain).

---

 Performance Results  
| Metric               | Training (70% Data)      | Testing (30% Data)       | Overall          | 
|----------------------|--------------------------|--------------------------|------------------|  
| Total Return     | 17.19%                  | 36.62%                  | 199.24%                | 
| Sharpe Ratio     | 0.41                    | 0.72                    | 0.78                   | 
| Max Drawdown     | -55.61%                 | -40.98%                 | -80.73%                | 

---

 Key Insights  
1. Overfitting in Training:
   - Training data showed lower returns (-55% drawdown) due to aggressive parameter tuning.
   - Testing data performed better (36% return), indicating generalization potential.

2. High Risk-Reward Tradeoff:
   - Profit Target (10%) drove high returns but increased exposure to tail risks (80% drawdown).
   - Smaller stop_loss (0.5%) amplified volatility but improved risk-adjusted returns (Sharpe 0.78).

3. Z-Score Dynamics:
   - Larger window=40 captured longer-term trends, reducing false signals.
   - Lowering entry_threshold=1.0 increased trade frequency but improved responsiveness.

---

 Challenges & Solutions  
- Challenge: Extreme drawdowns due to aggressive profit targets.
  - Solution: Add position sizing (e.g., risk 1% capital per trade).
- Challenge: Transaction costs eroding gains.
  - Solution: Optimize cost_per_trade and use limit orders.

---

 Conclusion  
- Achievements:
  - Built a scalable mean-reversion framework with dynamic parameter tuning.
  - Achieved 199% total return but highlighted critical risks.
- Future Work:
  - Incorporate volatility filters (e.g., ATR) to avoid trending markets.
  - Test on diverse assets (commodities, FX) and timeframes.

 Final Parameters (Best Performance)
best_params = {
    'window': 40,
    'entry_threshold': 1.0,
    'exit_threshold': 0.5,
    'stop_loss': 0.005,
    'profit_target': 0.1,
    'cost_per_trade': 0.001
}
  

This project demonstrates the power of systematic optimization in quantitative trading while underscoring the importance of balancing risk and reward.
