# ✈️ Airline Loyalty Behavioral Intelligence Project

> **Redefining airline loyalty from static value metrics to dynamic behavioral engagement signals.**

---

## 📋 Project Overview

This project builds a complete behavioral intelligence framework for a Canadian airline loyalty program. Instead of relying on formal cancellation records or historical CLV, the analysis detects **silent disengagement** — customers who stop flying without ever formally leaving.

**Dataset:** ~16,737 Canadian airline loyalty customers | **Period:** 2012–2018

---

## 🎯 Core Objectives

| Objective | Approach |
|---|---|
| Predict customer churn | Random Forest Classifier with temporal leakage prevention |
| Segment customers meaningfully | Rule-based behavioral segmentation (6 segments) |
| Detect silent high-value risk | CLV × engagement cross-analysis |
| Recommend retention actions | Segment-specific campaign playbook |

---

## 📁 Project Structure

```
AirlineLoyaltyProgram/
│
├── 📊 Data
│   ├── Customer Loyalty History.csv       # Demographics, CLV, tier, enrollment
│   ├── Customer Flight Activity.csv       # Monthly flights, distance, points
│   ├── Calendar.csv                       # Date reference table
│   └── Airline Loyalty Data Dictionary.csv
│
├── 📓 Analysis
│   └── nb1.ipynb                          # Complete analysis notebook (PRIMARY)
│
├── 📄 Reports & Deliverables
│   ├── technical_report.pdf               # Full technical report (this file)
│   ├── README.md                          # This file
│   └── Problem Statements.pdf             # Original problem statement
```

---

## 🔍 Methodology

### 1. Behavioral Churn Definition
Standard cancellation tracking captures only ~12% churn. This project defines:

> **Behavioral Churn = 6+ consecutive months with zero flight activity**

This captures **15.7% churn** — revealing a population of silently disengaging customers invisible to traditional systems.

### 2. Leakage-Safe Feature Engineering

| Window | Purpose |
|---|---|
| Jan 2017 – Jun 2018 | Feature generation (historical behavioral data) |
| Jul 2018 – Dec 2018 | Churn label observation (future prediction target) |

All features were built exclusively from the historical window. No future activity was used.

### 3. Engineered Features

| Feature | Description |
|---|---|
| Active Months | Months with at least one flight (consistency signal) |
| Redemption Ratio | Points Redeemed / Points Accumulated |
| Redeemer Flag | Binary: has the customer ever redeemed? |
| Engagement Score | MinMax-normalized composite (active months + flights + points) |
| CLV Segment | Quartile-based value band |
| Tenure | Years since enrollment |

---

## 📊 Key Results

### Behavioral Churn Analysis

| Metric | Value |
|---|---|
| Overall Behavioral Churn Rate | **15.7%** |
| Churn Rate — Non-Redeemers | 34.0% |
| Churn Rate — Redeemers | 7.0% |
| Avg Active Months (Retained) | 11.7 months |
| Avg Active Months (Churned) | 3.6 months |

### Predictive Model (Random Forest)

| Metric | Score |
|---|---|
| Accuracy | **94%** |
| Precision | **0.90** |
| Recall | 0.68 |
| F1 Score | 0.78 |
| Future Churn Rate | 13.9% |

**Confusion Matrix:**
```
                  Predicted Stay   Predicted Churn
Actual Stay           2,618              34
Actual Churn            136             294
```

### Top Feature Importances

```
Points Accumulated   ████████████████████  0.23
Distance             ████████████████████  0.20
Tenure               ████████████████████  0.18
Active Months        ████████████          0.11
Total Flights        ████████████          0.10
CLV                  ████████████          0.10   ← surprisingly weak
Redemption Ratio     █████                 0.04
Points Redeemed      ████                  0.03
Redeemer Flag        █                     0.004
```

---

## 👥 Customer Segments

| Segment | Count | Share | Churn Rate | Avg CLV | Priority |
|---|---|---|---|---|---|
| Reward-Oriented Active | 9,700 | 58.0% | 0.0% | $7,971 | Maintain |
| Regular Customers | 3,708 | 22.2% | 16.8% | $7,420 | Grow |
| Dormant Low Engagement | 1,350 | 8.1% | 100.0% | $5,138 | Low-cost nudge |
| Engaged Loyalists | 948 | 5.7% | 0.0% | $7,968 | Retain & leverage |
| **Silent High Value** | **660** | **3.9%** | **100.0%** | **$17,239** | **🔴 Urgent** |
| New Promotion Users | 371 | 2.2% | 0.3% | $8,116 | Onboard |

### ⚠️ Silent High Value — The Critical Segment
These 660 customers have the **highest average CLV ($17,239)** but **100% behavioral churn** and near-zero recent activity. Traditional loyalty systems would never flag them — their CLV score remains high. Only behavioral monitoring reveals the risk.

**Recommended action:** Personalised premium win-back campaign referencing prior travel history, double-mile incentives on previously flown routes, dedicated relationship manager for top 100 by CLV.

---

## 💡 Key Business Insights

1. **Loyalty tier ≠ churn predictor.** Aurora (17.0%), Nova (16.0%), Star (14.9%) — nearly identical churn rates across all tiers.
2. **Redemption is the strongest retention signal.** Non-redeemers churn at 4.9× the rate of redeemers.
3. **Engagement consistency matters more than peak activity.** Retained customers show 3.2× more active months.
4. **CLV does not predict future loyalty.** Churn is ~34% across ALL four CLV quartiles.
5. **Silent churn is real and underdetected.** 15.7% behavioral churn vs. 12% formal cancellation — a meaningful gap.

---

## 🗂️ Dashboard (Power BI)

The accompanying dashboard (`dashboard.pbix`) contains 5 pages:

| Page | Content |
|---|---|
| 1 — Executive Overview | KPI cards, churn by segment/tier/year |
| 2 — Customer Segmentation | Segment distribution, engagement comparisons, CLV matrix |
| 3 — Churn Intelligence | Predicted churners, Silent High Value drill-down |
| 4 — Retention Action Center | Segment-by-segment campaign recommendations |
| 5 — Model Explainability | Feature importance, confusion matrix, model metrics |

See **Power BI Build Guide** (below) for detailed construction instructions.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python 3.x | Data processing and modeling |
| pandas / numpy | Data manipulation |
| scikit-learn | Random Forest model, MinMaxScaler |
| Jupyter Notebook | Analysis and documentation |
| Power BI Desktop | Interactive dashboard |
| ReportLab | PDF report generation |

---

## ▶️ How to Reproduce

```bash
# 1. Place all 3 CSV files in the working directory
#    (Customer Loyalty History.csv, Customer Flight Activity.csv, Calendar.csv)

# 2. Open nb1.ipynb in Jupyter
jupyter notebook nb1.ipynb

# 3. Run all cells in order (Kernel → Restart & Run All)

# 4. Outputs: customer_summary DataFrame with all features + segments + predictions
```

---

## 📌 Project Contribution

The core contribution of this project is **not** simply building a churn model. It is demonstrating that:

> *Airlines can substantially improve retention intelligence by shifting from static customer valuation to dynamic behavioral monitoring — detecting silent disengagement before it becomes permanent, and intervening with segment-specific precision.*

---

*Airline Loyalty Behavioral Intelligence Project | Technical Analysis | 2012–2018 Dataset*
