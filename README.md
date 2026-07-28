# Target Corp Quarterly Revenue Regression

A time-series regression model that predicts **Target Corporation's** quarterly revenue using a linear time trend and seasonal dummy variables. Built in Python with `statsmodels`, and evaluated on a held-out test set using Mean Absolute Percentage Error (MAPE).

> **Note:** The notebook is titled *Utilities-Company-Regression*, but the analysis filters to `tic == 'TGT'` (Target Corp, a retailer). Rename if needed.

## Overview

The goal is to model how Target's revenue moves over time and to capture its strong seasonal pattern — a spike in Q4 (holiday shopping) and a dip in Q1 (post-holiday). The model is trained on the earlier 75% of quarters and tested on the most recent 25% to check how well it forecasts out-of-sample.

## Data

- **Source:** Quarterly Compustat data (`qSales_2024.csv`)
- **Company:** Target Corp (ticker `TGT`), filtered from a larger multi-company dataset
- **Target variable:** `saleq` — quarterly revenue (USD millions)
- **Coverage:** ~90 quarters (2000 Q4 – 2023 Q1)

## Methodology

**Feature engineering**
- `time` — a linear index (1, 2, 3, …) representing each successive quarter
- `q4_dv` — dummy = 1 if the quarter is Q4, else 0 (holiday season)
- `q1_dv` — dummy = 1 if the quarter is Q1, else 0 (post-holiday dip)

Q2 and Q3 act as the baseline (reference) quarters.

**Train/test split**
- First 75% of quarters → training set
- Last 25% of quarters → testing set

**Model**

Ordinary Least Squares (OLS):

```
saleq = const + β₁·time + β₂·q4_dv + β₃·q1_dv
```

## Results

Estimated coefficients:

| Term      | Coefficient | Interpretation                                      |
|-----------|-------------|-----------------------------------------------------|
| const     | 9,399.40    | Baseline revenue level                              |
| time      | 139.15      | Revenue grows ~$139M each quarter (steady growth)   |
| q4_dv     | 4,472.62    | Q4 adds ~$4.47B vs. baseline (holiday boost)        |
| q1_dv     | -232.80     | Q1 dips slightly below baseline (post-holiday)      |

**Test-set accuracy:** MAPE ≈ **12%** — the model predicts held-out quarters within roughly 12% on average.

## Key Findings

- Target's revenue is **highly seasonal**, with a consistent Q4 peak and Q1 trough repeating every year.
- There's a clear **upward growth trend** over the full period.
- The model slightly **under-predicts** the most recent quarters, since revenue in later years grew faster than the linear trend assumes.
- Q4 revenue is a strong, predictable benchmark for overall company performance.

## Tech Stack

- Python
- pandas
- numpy
- statsmodels (OLS)
- matplotlib

## How to Run

1. Place `qSales_2024.csv` in the working directory.
2. Open the notebook in Google Colab or Jupyter.
3. Run all cells top to bottom.

The notebook loads the data, engineers the features, fits the model, prints the coefficients, computes MAPE, and plots actual vs. predicted revenue.
