# FinOptix — Algorithmic Trading & Backtesting System

> A rules-based quantitative trading engine built from scratch, featuring Walk-Forward Optimization, ATR-based risk management, and dual market regime detection — tested across 5 crypto assets over 5 years of live market data.

---

## Performance Summary (Out-of-Sample, 2018–2023)

| Asset | Sharpe (OOS) | CAGR | Total Return | Max Drawdown | Win Rate | Trades |
|-------|-------------|------|-------------|-------------|----------|--------|
| BTC-USD | 1.20 | 7.70% | 53.74% | -1.84% | 40.0% | 15 |
| ETH-USD | 1.27 | 7.58% | 52.76% | -2.54% | 40.0% | 20 |
| LTC-USD | 1.35 | 7.52% | 52.24% | -1.94% | 38.1% | 21 |
| XRP-USD | **1.54** | **12.88%** | **101.85%** | -1.50% | **52.0%** | 25 |
| BCH-USD | 1.34 | 8.51% | 60.54% | -1.91% | 36.4% | 22 |

> **XRP outperformed its buy-and-hold benchmark by 116% on a cumulative basis.**  
> All results are strictly out-of-sample with 0.1% transaction costs applied per trade.

---
## Strategy Tearsheet (XRP-USD)
<img width="1740" height="711" alt="XRP-USD_OOS_vs_Benchmark" src="https://github.com/user-attachments/assets/4951c4b3-84f8-4767-87b8-2f6856d3c1f7" />



## System Architecture

```
Raw Price Data (yfinance)
        │
        ▼
Market Regime Detection
  ├── Trend Regime (200-day SMA → Bull / Bear)
  └── Volatility Regime (30-day rolling STD → Low / Medium / High)
        │
        ▼
Signal Generation
  ├── MACD (12/26/9 EMA crossover)
  ├── ADX (14-period directional strength filter)
  └── ATR (14-period true range)
        │
        ▼
Walk-Forward Optimization (WFO)
  ├── 12-month rolling train window
  ├── 3-month rolling test window
  └── 36-combination grid search per fold
        │
        ▼
Backtesting Engine
  ├── ATR-based position sizing (1% risk per trade)
  ├── Trailing stop-loss
  ├── Take-profit targets
  └── 25% max per-trade drawdown guard
        │
        ▼
Performance Tearsheet
  ├── OOS Equity Curve vs Buy-and-Hold
  ├── Drawdown Profile
  └── Trade PnL Distribution
```

---

## Key Technical Components

### 1. Signal Generation (`generate_combined_signals`)
Combines three indicators for high-conviction entry signals:
- **MACD** — momentum confirmation (12/26/9 EMA)
- **ADX** — trend strength filter (threshold: 20/25/30, grid-searched)
- **ATR** — volatility-normalized trailing stops

Long entry requires: DI+ crosses above DI−, ADX above threshold, and MACD above signal line — all simultaneously.

### 2. Walk-Forward Optimization (`walk_forward_optimization`)
- Rolls a **12-month train / 3-month test** window across the full 5-year history
- Runs **36 parameter combinations** per fold via grid search
- Selects best params by in-sample Sharpe, applies them strictly out-of-sample
- Stitches OOS windows into a single continuous equity curve

### 3. Risk Management (`run_backtest`)
- **ATR-based position sizing**: risk amount = 1% of capital / ATR-scaled stop distance
- **Trailing stop**: dynamically adjusts as trade moves in favour
- **Max per-trade drawdown**: hard cap at 25% before forced exit
- **Signal reversal exit**: closes position on opposing signal

### 4. Regime Detection (`add_regime_filters`)
- **Trend**: classifies each day as Bull (price > 200 SMA) or Bear
- **Volatility**: buckets rolling 30-day return STD into Low / Medium / High tertiles

---

## Hyperparameter Grid

| Parameter | Values Searched |
|-----------|----------------|
| `adx_threshold` | 20, 25, 30 |
| `stop_loss_pct` | 0.10, 0.15 |
| `take_profit_pct` | 0.15, 0.20 |
| `atr_trail_mult` | 5, 7, 9 |

**Total combinations per fold: 36**

---

## How to Run

```bash
# Install dependencies
pip install numpy pandas matplotlib yfinance plotly python-dateutil

# Run the notebook
jupyter notebook FinOptix-Algo_Trading_System.ipynb
```

The notebook runs end-to-end — fetches live data, runs WFO across all 5 assets, and generates tearsheet plots automatically.

---

## Assets & Data

- **Universe**: BTC-USD, ETH-USD, LTC-USD, XRP-USD, BCH-USD
- **Period**: January 2018 – January 2023 (5 years, 1,826 rows per asset)
- **Source**: Yahoo Finance via `yfinance`
- **Initial Capital**: $1,000,000
- **Transaction Cost**: 0.1% per trade (applied at entry and exit)

---

## Tech Stack

`Python` · `NumPy` · `Pandas` · `Plotly` · `yfinance` · `itertools`
