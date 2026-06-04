# ARK Invest: Skill or Luck?

> Personal research project — *N.F.*

---

## Overview

This project investigates whether ARK Invest's actively managed ETFs generate **alpha through genuine manager skill**, or whether their historical performance is better explained by systematic factor exposure and market conditions.

The analysis covers five ARK ETFs — ARKK, ARKW, ARKG, ARKF, ARKQ — benchmarked against the S&P 500 (SPY) over the period **January 2019 – December 2024**, using monthly return data and the Carhart four-factor model.

---

## Research Question

ARK Invest gained widespread attention during 2020–2021 for extraordinary returns, only to suffer steep drawdowns in subsequent years. This raises a fundamental question in asset management:

*Is outperformance evidence of skill — the ability to identify mispriced innovation stocks — or is it the product of factor tilts, momentum, and favorable market conditions that eventually reversed?*

---

## Data

| Source | Content |
|--------|---------|
| `yfinance` | Monthly adjusted closing prices for ARKK, ARKW, ARKG, ARKF, ARKQ, SPY (2019–2024) |
| `fmfinance` | Fama-French 4-factor data: Mkt-RF, SMB, HML, MOM and risk-free rate (French Data Library) |

**In-sample period:** January 2019 – December 2022  
**Out-of-sample period:** January 2023 – December 2024  
**Frequency:** monthly

---

## Analysis

### Phase 1 — Return Analysis

Descriptive statistics of monthly excess returns (over the risk-free rate RF) for all five ETFs and SPY: mean, standard deviation, skewness, excess kurtosis, min, max, CAGR, annualised volatility, Sharpe ratio, Sortino ratio, maximum drawdown, VaR 5% and CVaR 5%. Metrics are computed with `quantstats` using `periods_per_year=12`.

Cumulative excess return chart over the full 2019–2024 period, annotated with key market events (COVID crash Feb–Mar 2020, bull market rally 2020, ARK drawdown 2021–2022, Fed rate hike cycle).

Correlation matrix of excess returns across the five ETFs, showing that despite different declared themes (genomics, fintech, robotics) all funds share a very high pairwise correlation — suggesting they are largely interchangeable from a risk-exposure standpoint.

### Phase 2 — Carhart Four-Factor Model (OLS, in-sample)

OLS regression of each ETF's monthly excess returns on the four Carhart factors (Mkt-RF, SMB, HML, Mom) over the in-sample period, presented in a joint table via `summary_col`. Key findings:

- **Market beta (Mkt-RF):** all ETFs show betas between 1.13 and 1.52, amplifying market moves.
- **SMB:** all positive and significant — ARK funds behave like small-cap portfolios.
- **HML:** all negative and significant — confirming the structural growth tilt.
- **Momentum (Mom):** not statistically significant for any fund in-sample.
- **Alpha (Jensen's):** only ARKF shows a significant alpha (−1.49\*\*, ≈ −18% p.a.); no fund generates positive significant alpha.
- **R²:** around 0.81 for all funds — roughly 80% of monthly return variation is explained by the four factors.

### Phase 3 — Rolling Analysis

RollingOLS regressions with a 24-month window on the in-sample period, tracking the time evolution of all four factor loadings and alpha for each ETF. The analysis documents style drift across market regimes, in particular the variation in momentum exposure between the 2020 bull market and the 2021–2022 drawdown.

### Phase 4 — Bootstrap: Skill or Luck?

Cross-sectional bootstrap procedure following Cuthbertson, Nitzsche & O'Sullivan (2008), implemented via `fm.bootstrap()` (n\_boot = 1000, min\_obs = 24). The procedure constructs a **luck distribution** for the t-statistics of alpha by resampling residuals under the null hypothesis of zero alpha, and compares each fund's observed t-alpha against the corresponding quantile of the simulated distribution.

Results: no ARK ETF clears the 95th percentile of the luck distribution (positive tail) or falls below the 5th percentile (negative tail). The conclusion is that performance across all five funds — both positive and negative — is statistically indistinguishable from luck once systematic factor exposures are controlled for.

The analysis also highlights the difference between classical OLS significance (which ignores the multiple-testing problem) and the bootstrap test: ARKF's significant OLS alpha does not survive the more conservative cross-sectional benchmark.

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

## Theoretical References

- Fama, E.F. & French, K.R. (1993). *Common Risk Factors in the Returns on Stocks and Bonds.* Journal of Financial Economics, 33(1), 3–56.
- Carhart, M.M. (1997). *On Persistence in Mutual Fund Performance.* Journal of Finance, 52(1), 57–82.
- Cuthbertson, K., Nitzsche, D. & O'Sullivan, N. (2008). *UK Mutual Fund Performance: Skill or Luck?* Journal of Empirical Finance, 15(4), 613–634.
- Cuthbertson, K., Nitzsche, D. & O'Sullivan, N. (2022). *Fund Performance Persistence: Factor Models and Portfolio Size.* International Review of Financial Analysis, 81, 102133.

---

## License

This project is protected by copyright. See the [`LICENSE`](LICENSE) file for full terms.  
**Reusing, copying, modifying, or redistributing** the code without explicit written permission from the author is not permitted.
