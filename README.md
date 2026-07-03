# FinOptix - Algo Trading System

This project is my attempt at building a complete quantitative trading pipeline instead of just backtesting a trading strategy.

I implemented a systematic trend-following strategy using **MACD, ADX, and ATR**, along with **walk-forward optimization** to evaluate the strategy on unseen market data. The framework also includes dynamic position sizing, transaction costs, Buy & Hold benchmarking, and performance analysis using metrics like Sharpe Ratio, CAGR, Maximum Drawdown, and Win Rate.

The goal was not to build a profitable trading bot, but to understand how systematic trading strategies are developed, optimized, and evaluated in quantitative finance.

## Features

- Multi-asset backtesting (BTC, ETH, LTC, XRP, BCH)
- Walk-forward optimization (12-month train, 3-month test)
- ATR-based risk management and position sizing
- Hyperparameter optimization using grid search
- Buy & Hold benchmark comparison
- Performance metrics (Sharpe, CAGR, Drawdown, Win Rate, Profit Factor)
- Interactive Plotly visualizations

## Tech Stack

- Python
- Pandas
- NumPy
- SciPy
- Plotly
- yFinance

## Running the Project

Install the required libraries and run the notebook from top to bottom.

```bash
pip install numpy pandas scipy plotly yfinance matplotlib
```

Then open:

```
FinOptix-Algo_Trading_System.ipynb
```

and execute all cells sequentially.

---
