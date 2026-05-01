# A/B Testing Analysis — Website UI Conversion Study

> Designed and executed a full-cycle A/B test on 10,000 simulated users to determine whether a redesigned e-commerce interface drives higher purchase conversion — from hypothesis through to statistical validation and revenue impact modelling.

![Python](https://img.shields.io/badge/Python-3.9+-20B2AA?style=flat-square&logo=python&logoColor=white)
![Scipy](https://img.shields.io/badge/SciPy-Statistical%20Testing-2E86AB?style=flat-square)
![Result](https://img.shields.io/badge/Result-Statistically%20Significant-2da44e?style=flat-square)
![Impact](https://img.shields.io/badge/Revenue%20Impact-%2B%24274K%2Fmo-f0a500?style=flat-square)

---

## What I Did

I built a complete A/B testing pipeline from scratch — no templates, no shortcuts. The goal was to simulate a real product team decision: should we ship the new UI or stick with the old one?

The answer: **ship it**.

---

## Results

| Metric | Control (Old UI) | Treatment (New UI) |
|--------|------------------|--------------------|
| Users | 5,000 | 5,000 |
| Conversions | 578 | **793** |
| Conversion Rate | 11.56% | **15.86%** |
| Relative Uplift | — | **+37.2%** |
| P-value | — | **0.000027** ✓ |
| 95% Confidence Interval | — | [+2.81%, +5.79%] |

**At 100K monthly visitors and a $65 average order value, this uplift translates to ~$274,000 in additional monthly revenue.**

---

## Methodology

```
01  Data Generation    →  10,000 synthetic users, binomial sampling, fixed seed
02  Data Validation    →  null checks, duplicate detection, group balance check
03  Exploratory EDA    →  conversion by group + device type (mobile/desktop/tablet)
04  Statistical Test   →  one-tailed two-sample proportions Z-test, α = 0.05
05  Business Impact    →  revenue projection + rollout recommendation
```

The new design outperformed control on **every single device type** — not just overall. That consistency is what makes this result convincing.

---

## Project Structure

```
ab-testing-analysis/
├── A-B_Testing_analysis.ipynb   ← full analysis with step-by-step commentary
├── ab_test_data.csv             ← generated dataset (10,000 rows)
├── eda_charts.png               ← 3-panel exploratory visualisation
├── requirements.txt             ← pinned dependencies
└── README.md
```

---

## How to Run

```bash
git clone https://github.com/YOUR_USERNAME/ab-testing-analysis.git
cd ab-testing-analysis
pip install -r requirements.txt
jupyter notebook A-B_Testing_analysis.ipynb
```

---

## Tech Stack

| Library | Role |
|---------|------|
| `pandas` | Data wrangling & EDA |
| `numpy` | Synthetic data generation |
| `scipy` | Statistical primitives |
| `statsmodels` | Proportions Z-test |
| `matplotlib` | Data visualisation |
| `seaborn` | Chart aesthetics |

---

## What I Learned / Would Improve Next

- Add a **power analysis** to formally justify the sample size of 5,000
- Run the test for a minimum of **2 full weeks** to account for day-of-week variation
- Apply **Bonferroni correction** when running sub-group tests (device segments)
- Validate traffic and AOV assumptions with real business data before final rollout

---

*Built as a portfolio project demonstrating end-to-end A/B testing — from data generation to business recommendation.*  
*Pune, India*
