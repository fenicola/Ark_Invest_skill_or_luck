# ARK Invest: Skill or Luck?

> Personal research project — *N.F.* — **Work in Progress**
> All rights reserved. See `LICENSE` for details.

---

## Overview

This project investigates whether ARK Invest's actively managed ETFs generate **alpha through genuine manager skill**, or whether their historical performance is better explained by systematic factor exposure and market conditions.

The analysis covers five ARK ETFs — ARKK, ARKW, ARKG, ARKF, ARKQ — benchmarked against the S&P 500 (SPY) over the period **January 2019 – December 2024**, using monthly return data.

> ⚠️ **This project is a work in progress.** The data pipeline and initial structure are in place; analysis sections are being developed.

---

## Research Question

ARK Invest gained significant attention during 2020–2021 for extraordinary returns, only to suffer steep drawdowns in subsequent years. This raises a fundamental question in asset management:

*Is outperformance evidence of skill — the ability to identify mispriced innovation stocks — or is it the product of factor tilts, momentum, and favorable market conditions that eventually reversed?*

---

## Data

| Source | Content |
|--------|---------|
| `yfinance` | Monthly adjusted closing prices for ARKK, ARKW, ARKG, ARKF, ARKQ, SPY |
| `fmfinance` | Fama-French 4-factor data (Mkt-RF, SMB, HML, MOM) |

**Sample period:** 2019-01-01 to 2024-12-31 (monthly frequency)

---

## Planned Analysis

### Phase 1 — Return Analysis
Descriptive statistics of raw and excess returns (over the risk-free rate) for each ETF and the benchmark. Cumulative performance charts, drawdown analysis.

### Phase 2 — Factor Model Estimation
Regression of excess returns on the **Fama-French 4-factor model** (market, size, value, momentum) to decompose performance into:
- **Alpha** (risk-adjusted excess return, if any)
- **Factor loadings** (systematic exposures)

**Rolling OLS** regressions to track how factor exposures evolved over time — particularly relevant given ARK's style drift across market regimes.

### Phase 3 — Portfolio Optimization
Construction of mean-variance efficient portfolios using `PyPortfolioOpt` (Efficient Frontier), comparing ARK allocations against optimized benchmarks on risk/return grounds.

### Phase 4 — Additional Analysis
- Web scraping of supplementary fund data
- Interactive performance dashboard (via `ipywidgets` and `plotly`)
- Statistical tests for persistence of alpha

---

## Project Structure

```
.
└── progetto.ipynb    # Main Jupyter notebook (all analysis)
```

---

## Requirements

```bash
pip install numpy pandas matplotlib seaborn yfinance fmfinance quantstats \
            statsmodels scipy pypfopt ipywidgets requests beautifulsoup4 plotly
```

Python 3.9+ recommended. Run in Jupyter Notebook or JupyterLab.

---

## License

This project is protected by copyright. See the [`LICENSE`](LICENSE) file for full terms.
**Reusing, copying, modifying, or redistributing** the code without explicit written permission from the author is not permitted.
