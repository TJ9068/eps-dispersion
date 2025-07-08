# EPS Dispersion Strategy  
*A Long–Short Equity Signal Based on Realized Fundamental Uncertainty*  

This repository implements a quantitative trading strategy that exploits persistent **earnings volatility** as a priced source of risk or mispricing. The strategy uses a **five-year trailing dispersion in EPS** (Earnings Per Share) as a proxy for fundamental uncertainty, constructing a long–short portfolio that consistently outperforms the market on both absolute and risk-adjusted bases.

---

## Background & Motivation

Traditional literature (e.g., Diether, Malloy & Scherbina, 2002) focuses on *analyst forecast dispersion* as a measure of uncertainty, often associated with overvaluation. In contrast, this strategy uses **realized EPS dispersion** — a backward-looking, accounting-based signal — to identify firms that are hard to value due to volatile fundamentals.

Inspired by findings from Zhang (2006), Pastor & Veronesi (2003, 2009), and Bali, Cakici & Whitelaw (2014), this project shows that realized uncertainty may reflect real economic risk, not just disagreement — and thus commands a premium.

---

## Strategy Overview

### Signal Construction:
- **EPS Calculation**: Annual EPS = Net Income (NIQ) / Shares Outstanding (CSHOQ), from Compustat  
- **Dispersion Metric**: 5-year trailing coefficient of variation (CV) of annual EPS  
- **Filters**:
  - Minimum 5 valid EPS observations
  - Exclude firms with near-zero mean EPS
  - Price filter ≥ $5 to reduce microcap noise

### Portfolio Formation:
- **Universe**: U.S. equities from CRSP/Compustat, 1966–2024
- **Monthly Sorting**: Stocks sorted into quintiles based on 5-year EPS dispersion
- **Long–Short Portfolio**: Long Q5 (high dispersion), Short Q1 (low dispersion)
- **Rebalancing**: Monthly
- **Volatility Targeting**: 20% annualized volatility on each leg
- **Transaction Costs**: 20 bps/month subtracted from gross returns

---

## Key Results (2019–2024 Backtest)

| Metric                        | Value          |
|------------------------------|----------------|
| Net Compounded Annual Return | **23.80%**     |
| Sharpe Ratio (net)           | **1.13**       |
| FF3 Alpha (net)              | **16.2%** (t = 2.00) |
| Skewness                     | 0.72           |
| Kurtosis                     | 0.26           |
| Sector Tilt (Long)           | Biotech, Public Admin, Holding Co. |
| Sector Tilt (Short)          | Banks, Utilities, Insurance |

**Alpha persists across size deciles and market conditions, with low correlation to standard risk factors.**

---

## Interpretation

### Risk-Based View:
- High dispersion reflects real risk: cash flow instability, macro sensitivity, operating leverage  
- Investors require compensation → priced risk premium  

### Behavioral View:
- Complex firms are avoided due to ambiguity aversion or institutional frictions  
- Mispricing persists due to neglect → alpha from behavioral underreaction  

---

## Implementation Details

### Code Pipeline (`EPS_Dispersion.ipynb`):
1. **Data Cleaning**: Load Compustat quarterly NIQ & CSHOQ, fill with CRSP SHROUT
2. **Signal Generation**: Compute 5-year EPS dispersion for each firm-year
3. **Portfolio Construction**: Merge with CRSP returns, apply price filter, quintile sort
4. **Performance Evaluation**:
   - CAPM & FF3 regressions (with HAC robust t-stats)
   - Rolling Sharpe and Alpha plots
   - Skew/Kurtosis diagnostics
5. **Robustness**:
   - Volatility targeting & cost modeling
   - Out-of-sample validation (2019–2024)
   - Sector & size exposure analysis

---

## Requirements

Tested in Python 3.8+. Required libraries:
```bash


