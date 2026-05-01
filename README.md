# A/B Testing Analysis — Website UI Conversion Study

![Python](https://img.shields.io/badge/Python-3.9+-blue) ![Statistics](https://img.shields.io/badge/Stats-Z--test-green) ![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

An end-to-end A/B test to determine whether a redesigned e-commerce UI significantly improves purchase conversion rates.

---

## Overview

Two groups of 5,000 simulated users were exposed to either the current design (control) or a new design (treatment). Conversion events were recorded and analysed using a two-sample proportions Z-test to determine whether the observed improvement was statistically significant.

## Results at a glance

| Metric | Control (old UI) | Treatment (new UI) |
|---|---|---|
| Users | 5,000 | 5,000 |
| Conversions | 578 | 793 |
| Conversion rate | 11.56% | 15.86% |
| Relative uplift | — | +37.2% |
| P-value (Z-test) | — | 0.000027 ✓ |
| 95% confidence interval | — | [+2.81%, +5.79%] |

**Conclusion:** The new design produces a statistically significant improvement across all device types (mobile, desktop, tablet). Estimated additional revenue: ~$274,000/month at 100K monthly visitors.

## Project structure

```
ab-testing-analysis/
├── A-B_Testing_analysis.ipynb   ← main notebook (all steps)
├── ab_test_data.csv             ← generated dataset (10,000 rows)
├── eda_charts.png               ← exploratory analysis charts
├── requirements.txt             ← Python dependencies
└── README.md
```

## How to run

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/ab-testing-analysis.git
cd ab-testing-analysis

# 2. Install dependencies
pip install -r requirements.txt

# 3. Open the notebook
jupyter notebook A-B_Testing_analysis.ipynb
```

## Methodology

1. **Data generation** — 10,000 synthetic users with realistic conversion rates (12% control, 15% treatment) based on e-commerce benchmarks.
2. **Data validation** — checked for missing values, duplicates, and group balance before any analysis.
3. **Exploratory analysis** — conversion rates by group and by device type.
4. **Statistical test** — one-tailed two-sample proportions Z-test (`statsmodels.stats.proportion.proportions_ztest`), α = 0.05.
5. **Business impact** — uplift translated into estimated revenue using assumed monthly traffic (100K) and average order value ($65).

## Tech stack

| Library | Purpose |
|---|---|
| `pandas` | Data manipulation |
| `numpy` | Random data generation |
| `scipy` | Statistical testing |
| `statsmodels` | Proportions Z-test |
| `matplotlib` | Charts and visualisations |
| `seaborn` | Chart styling |

## Key findings

- The treatment group outperformed control on **every device type**
- The p-value (0.000027) is well below the 0.05 threshold — the result is not due to chance
- Even the lower bound of the 95% CI (+2.81%) represents a meaningful business gain
- **Recommendation:** roll out the new design to 100% of users

## Notes

This project uses synthetic data generated to reflect realistic e-commerce conversion benchmarks. It was built as a learning exercise demonstrating a complete A/B testing workflow from data generation through to business recommendation
