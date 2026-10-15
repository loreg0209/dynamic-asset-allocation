# Dynamic Asset Allocation with Markowitz, Ledoit–Wolf and Michaud Resampling

This project develops and evaluates a **dynamic multi-asset allocation framework** using classical and robust portfolio-optimization techniques.

The analysis compares:

- Classical Markowitz maximum-Sharpe portfolios
- Markowitz portfolios with **Ledoit–Wolf covariance shrinkage**
- A **Michaud-style Resampled Efficient Frontier**
- A traditional **60/40 portfolio**
- A naive **1/N equally weighted portfolio**

The strategy is evaluated fully **out of sample** using a **60-month rolling estimation window** and **quarterly rebalancing**.

---

## Objective

The goal is to study how different portfolio-construction methods behave when applied dynamically through time, with particular attention to:

- expected return
- volatility
- Sharpe and Sortino ratios
- maximum drawdown
- turnover
- diversification
- sensitivity to portfolio constraints
- robustness to estimation error

Rather than searching for a single strategy that dominates every metric, the project focuses on the **risk-return trade-off** between alternative allocation approaches.

---

## Investment Universe

The investable universe contains **17 ETFs** covering multiple asset classes:

### Equities

- SPY — US large-cap equity
- EFA — Developed equity ex-US
- IWM — US small-cap equity
- VWO — Emerging-market equity
- XLK — US technology sector

### Fixed Income

- SHY — US Treasury 1–3Y
- IEF — US Treasury 7–10Y
- TLT — US Treasury 20Y+
- LQD — Investment-grade corporate credit
- HYG — High-yield corporate credit
- EMB — Emerging-market debt
- TIP — Inflation-linked Treasuries
- MBB — Agency mortgage-backed securities

### Alternative and Defensive Assets

- GLD — Gold
- DBC — Broad commodities
- VNQ — US REITs
- BIL — Cash proxy / Treasury bills

---

## Data

Market prices are downloaded through **Yahoo Finance** using `yfinance`.

The analysis covers the period from **January 2008 to August 2026**.

Daily adjusted prices are converted into **monthly simple returns** for portfolio estimation and backtesting.

The historical risk-free rate is taken from the **3-month US Treasury rate (DGS3MO)** from FRED and is aligned with each rebalance date.

---

## Backtest Setup

The dynamic allocation framework uses:

- **60-month rolling estimation window**
- **Quarterly rebalancing**
- **Monthly returns**
- **Long-only portfolios**
- **Fully invested portfolios**
- **No leverage**
- Historical arithmetic mean returns
- Historical risk-free rate available at each rebalance

The main optimized portfolios use a **40% maximum weight per asset**.

---

## Exploratory Analysis

The notebook includes:

- annualized mean returns
- annualized volatility
- minimum and maximum monthly returns
- correlation matrix
- normalized historical price performance

These diagnostics provide an overview of the risk, return and diversification characteristics of the ETF universe before optimization.

---

## Portfolio Strategies

### 1. Classical Markowitz

A rolling maximum-Sharpe portfolio is estimated using:

- historical arithmetic expected returns
- sample covariance matrix

Three weight-constraint specifications are compared:

- no cap
- 25% maximum weight
- 40% maximum weight

The analysis shows that unconstrained Markowitz portfolios can become highly concentrated, motivating the use of a position cap in the main comparison.

---

### 2. Ledoit–Wolf Covariance Shrinkage

The second approach keeps the same expected-return estimator and optimization framework but replaces the sample covariance matrix with a **Ledoit–Wolf shrinkage estimator**.

This aims to reduce covariance-estimation noise while preserving the same rolling out-of-sample framework.

---

### 3. Michaud Resampled Efficient Frontier

The third approach implements a **Michaud-style resampling procedure**.

At every rebalance:

1. Estimate expected returns and covariance from the previous 60 months
2. Generate **500 synthetic samples**
3. Re-estimate the return vector and covariance matrix for every sample
4. Build a **15-point efficient frontier** for each simulation
5. Average corresponding portfolio weights across simulated frontiers
6. Select the maximum-Sharpe portfolio from the resulting resampled frontier

The purpose is to explicitly account for **estimation uncertainty** and obtain more diversified allocations.

---

## Robustness Check

A dedicated sensitivity analysis removes **BIL**, the cash proxy, and reruns the Michaud strategy.

This tests whether the defensive behaviour of the resampled portfolio is driven exclusively by cash or whether it persists through other lower-risk assets.

---

## Benchmarks

Two naive benchmarks are included:

### 60/40 Portfolio

- 60% SPY
- 40% IEF

### 1/N Portfolio

Equal allocation across all 17 ETFs.

Both benchmarks are rebalanced quarterly.

---

## Performance Metrics

Each strategy is evaluated using:

- Annualized return
- CAGR
- Annualized volatility
- Sharpe ratio
- Sortino ratio
- Maximum drawdown
- Average quarterly turnover
- Average maximum asset weight
- Average number of active ETFs
- Final value of an initial investment of 100

---

## Main Out-of-Sample Results

| Strategy | CAGR | Volatility | Sharpe | Max Drawdown |
|---|---:|---:|---:|---:|
| Markowitz — Sample Covariance | 9.92% | 8.40% | 0.974 | -14.13% |
| Markowitz — Ledoit–Wolf | **11.40%** | 9.39% | **1.020** | -21.45% |
| Michaud — Resampled Efficient Frontier | 8.25% | 7.73% | 0.847 | **-13.76%** |
| 60/40 Benchmark | 9.37% | 9.07% | 0.839 | -20.70% |
| 1/N ETF Benchmark | 5.92% | **7.53%** | 0.566 | -17.63% |

The main findings are:

- **Ledoit–Wolf Markowitz** achieves the highest CAGR and Sharpe ratio.
- **Michaud resampling** produces the smallest maximum drawdown among the optimized strategies and a much more diversified portfolio.
- Classical Markowitz with sample covariance performs well but remains relatively concentrated.
- The sophisticated strategies do not dominate every metric, highlighting the importance of evaluating portfolio construction as a multidimensional risk-return trade-off.

---

## Diversification

Average numbers of active ETFs:

- Markowitz — Sample Covariance: about **4.7**
- Markowitz — Ledoit–Wolf: about **6.0**
- Michaud Resampled Frontier: about **12.7**
- 1/N Benchmark: 17

The Michaud procedure therefore produces a substantially more diversified allocation than classical Markowitz optimization.

---

## Stress Analysis

The notebook also evaluates portfolio behaviour during two important market episodes:

- **COVID market shock (February–April 2020)**
- **2022 broad market stress**

For each strategy, the analysis reports:

- cumulative return
- annualized volatility
- maximum drawdown

Maximum drawdown timing is also decomposed into:

- previous peak
- drawdown trough
- recovery date

---

## Visual Analysis

The notebook includes:

- ETF correlation matrix
- historical normalized performance
- efficient-frontier visualization
- cumulative out-of-sample wealth curves
- drawdown curves
- rolling 12-month volatility
- Michaud portfolio-weight heatmap
- portfolio-allocation comparisons

---

## Technologies

- Python
- Jupyter Notebook
- pandas
- NumPy
- matplotlib
- yfinance
- PyPortfolioOpt
- CVXPY
- FRED data
- Portfolio Optimization
- Backtesting
- Risk Analysis

---

## Limitations and Possible Extensions

Main limitations include:

- transaction costs are not included
- taxes are not modeled
- the analysis is based on a predefined ETF universe

A natural extension would be to introduce a **Black–Litterman model**, combining market-implied equilibrium returns with investor views.

---

## Author

**Lorenzo Gulizia**

Master's Degree in Data Science  
University of Milano-Bicocca
