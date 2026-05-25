# CAPM, Security Market Line & Luck Distribution Bootstrap

> Master's degree project — *N.F.*
> MIT License — see `LICENSE` for details.

---

## Overview

This project consists of two independent notebooks that together explore two foundational questions in empirical asset pricing: **does beta explain returns?** and **is outperformance due to skill or luck?**

---

## Project Structure

```
.
├── 2.1.ipynb         # CAPM and Security Market Line — STOXX 600
├── 2.2.ipynb         # Luck Distribution Bootstrap — NASDAQ
├── stoxx600.xlsx     # STOXX 600 constituent list (stock universe)
└── ixic.csv          # NASDAQ composite constituent list
```

---

## Notebook 2.1 — CAPM and Security Market Line (STOXX 600)

A random sample of **111 stocks** is drawn from the STOXX 600 universe (seed = 777). Monthly returns are downloaded via `yfinance` and a **CAPM regression** is estimated for each stock using the STOXX 600 ETF (EXSA.DE) as the market proxy and XEON.DE as the risk-free proxy.

The empirical **Security Market Line** is plotted against the theoretical SML to assess how well beta explains cross-sectional returns.

The analysis is then repeated across **three sub-periods** to study how the beta-return relationship shifts across different market regimes:

| Sub-period | Dates | Market context |
|------------|-------|----------------|
| Period 1 | 2017–2019 | Moderate expansion, ECB accommodative policy |
| Period 2 | 2020–2022 | Covid crash, recovery, rate hike cycle begins |
| Period 3 | 2023–2025 | Post-tightening normalization |

A **panel regression with time dummies** is used to formally test whether the slope of the SML differs significantly across periods.

---

## Notebook 2.2 — Luck Distribution Bootstrap (NASDAQ)

A random sample of **300 stocks** is drawn from the NASDAQ universe (seed fixed for reproducibility). Monthly excess returns are computed and a **Fama-French 3-factor model** is estimated for each stock on the in-sample period (2017–2022).

### The Core Problem
On a large cross-section, classical OLS tests for alpha are unreliable: with hundreds of simultaneous tests, some stocks will show significant alpha purely by chance. The standard t-statistic threshold (1.645 at 95%) does not account for this.

### Luck Distribution Bootstrap
Following Fama & French (2010), a **bootstrap procedure with 1,000 simulations** is used to construct the distribution of alpha t-statistics that would arise from luck alone — by randomly shuffling residuals to destroy any true alpha signal while preserving the factor structure.

A stock is classified as **skilled** only if its real t-alpha exceeds the 95th percentile of the simulated luck distribution. This is a substantially higher bar than the classical test.

### Portfolio Construction and Out-of-Sample Evaluation
The **21 skilled stocks** identified in-sample are combined into an equal-weight portfolio. Performance is evaluated out-of-sample (2023–2025) against the NASDAQ Composite benchmark, using an **annual walk-forward rebalancing** scheme with a 6-year rolling estimation window.

---

## Requirements

```bash
pip install numpy pandas matplotlib seaborn yfinance fmfinance quantstats \
            statsmodels scipy openpyxl
```

Python 3.9+ recommended. Run in Jupyter Notebook or JupyterLab.

---

## License

MIT License — see [`LICENSE`](LICENSE) for details.
