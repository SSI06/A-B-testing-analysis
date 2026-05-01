# A/B Testing Analysis
Website UI Conversion Study

> **TL;DR** — I designed and ran a full A/B test on 10,000 users. The new UI increased purchases by 37%. The result was statistically significant. At scale, that's worth $274,000 extra per month. Here's exactly how I figured that out.

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=flat-square&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0-150458?style=flat-square&logo=pandas&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-Stats-8CAAE6?style=flat-square&logo=scipy&logoColor=white)
![Status](https://img.shields.io/badge/Result-Ship%20It%20✓-2da44e?style=flat-square)

---

## The Problem

Imagine you're a data analyst at an e-commerce company. The design team has built a new version of the website. The product manager asks: *"Will this actually get more people to buy things — or does it just look nice?"*

That's not a question you can answer by staring at the design. You need data.

This project is my answer to that question. I built an A/B test from scratch — the same framework used by teams at Google, Airbnb, and Netflix — to make a defensible, data-driven decision.

---

## My Thinking Before Writing a Single Line of Code

Before I touched Python, I asked three questions:

**1. What am I actually measuring?**
Conversion rate — the percentage of visitors who make a purchase. Not clicks, not time-on-page. The thing the business cares about.

**2. How will I know if the result is real and not just luck?**
A two-sample proportions Z-test. If the p-value drops below 0.05, the result is statistically significant. I also calculated a 95% confidence interval so I could tell the business *how big* the effect is — not just *whether* it exists.

**3. What does this mean in pounds and pence?**
A percentage uplift is meaningless without context. I translated it into estimated monthly revenue so the recommendation has weight.

This order of thinking — define the question, choose the right test, frame the output for the audience — is how I approach every analysis.

---

## The Setup

I split 10,000 simulated users into two equal groups:

| Group | Design | Conversion Rate (true) |
|-------|--------|------------------------|
| Control (A) | Old UI | 12% |
| Treatment (B) | New UI | 15% |

Users were assigned randomly. Device type (mobile, desktop, tablet) was recorded to allow segmentation later. Data was generated using `numpy.random.binomial` with a fixed seed — so the results are fully reproducible.

> **Why synthetic data?** Because the methodology is the point. The same pipeline works on real clickstream data — you'd just swap the data source.

---

## What the Data Showed

Before running any test, I explored the data to make sure I understood it.

```
Control  conversion rate:   11.56%
Treatment conversion rate:  15.86%
Absolute difference:        +4.30 percentage points
Relative uplift:            +37.2%
```

The new design also outperformed the old one on **every device type**:

| Device | Control | Treatment |
|--------|---------|-----------|
| Mobile | 11.35% | 14.97% |
| Desktop | 12.08% | 16.24% |
| Tablet | 10.98% | 20.11% |

That consistency across segments matters. If the result only showed up on one device, I'd be suspicious. Seeing it everywhere makes it convincing.

---

## Proving It Wasn't Luck

Here's where most tutorials stop. I didn't.

I ran a one-tailed two-sample proportions Z-test to check whether the treatment group's higher conversion rate could have happened by chance.

```python
from statsmodels.stats.proportion import proportions_ztest

count = np.array([conv_treatment, conv_control])
nobs  = np.array([n_treatment, n_control])

z_stat, p_value = proportions_ztest(count, nobs, alternative="larger")
```

**Result:**

```
Z-statistic:  4.09
P-value:      0.000027
```

With a p-value of 0.000027 — far below the 0.05 threshold — I rejected the null hypothesis. The improvement is real.

I also calculated a **95% confidence interval** for the true effect size:

```
Observed difference:  +4.30 percentage points
95% CI:               [+2.81%, +5.79%]
```

Even in the most pessimistic scenario (the lower bound), the new design still improves conversion by 2.81 percentage points. That's the number I'd put in front of a product manager.

---

## What This Means for the Business

Statistics alone don't drive decisions — money does. So I translated the result:

```
Assumed monthly visitors:   100,000
Assumed avg. order value:   $65

Monthly conversions (old):  11,560
Monthly conversions (new):  15,860
Extra conversions/month:    +4,300

Extra revenue per month:    $279,500
Extra revenue per year:     $3,354,000
```

> These figures use assumed inputs — in a real setting, I'd pull actual traffic and AOV from the analytics platform before presenting this. The methodology is what matters here.

---

## My Recommendation

**Ship the new design.**

Three reasons:
1. The improvement is statistically significant (p = 0.000027)
2. It holds across all device types — this isn't a mobile-only effect
3. Even the conservative lower bound of the CI (+2.81%) represents meaningful revenue

One caveat: this test used synthetic data and assumed business inputs. Before a real rollout, I'd confirm the test ran for at least two full weeks (to capture day-of-week variation) and validate the AOV and traffic figures with the business team.

---

## Project Structure

```
ab-testing-analysis/
├── A-B_Testing_analysis.ipynb   ← full walkthrough, step by step with commentary
├── ab_test_data.csv             ← the dataset (10,000 rows)
├── eda_charts.png               ← 3-panel visualisation (conversion by group + device)
├── requirements.txt             ← all dependencies, pinned
└── README.md
```

---

## How to Run It Yourself

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/ab-testing-analysis.git
cd ab-testing-analysis

# Install dependencies
pip install -r requirements.txt

# Open the notebook
jupyter notebook A-B_Testing_analysis.ipynb
```

---

## Stack

| Tool | Why I used it |
|------|---------------|
| `pandas` | Data wrangling, groupby aggregations, summary tables |
| `numpy` | Reproducible synthetic data generation |
| `scipy` | Statistical foundations |
| `statsmodels` | Proportions Z-test — the right tool for binary conversion data |
| `matplotlib` | Full control over chart layout and annotation |
| `seaborn` | Cleaner default aesthetics on top of matplotlib |

---

## If I Were Doing This at Work

Things I'd add on a real project:

- **Power analysis upfront** — formally calculate the minimum sample size needed to detect a meaningful effect before the test runs, not after
- **Bonferroni correction** — when running multiple sub-group comparisons (e.g. by device), adjust the significance threshold to avoid false positives
- **Minimum two-week runtime** — to smooth out day-of-week and seasonal effects
- **Novelty effect check** — monitor conversion rate over time to ensure the lift isn't just curiosity about the new design fading after a few days

---

*This project is part of my data analytics portfolio. The methodology reflects real industry practice — the data is synthetic, the thinking is not.*

*— Pune, India*
