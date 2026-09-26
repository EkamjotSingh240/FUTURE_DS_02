# FUTURE_DS_02 — Customer Retention & Churn Analysis Dashboard

## 📌 Task Objective
This project was completed as part of the **Data Science & Analytics** track internship (Task 2). The goal was to analyze customer/subscription data to understand why customers leave and what drives retention, then present the findings through a professional, client-ready dashboard. Core business questions addressed:

- Why are customers leaving the platform?
- Which customer segments are most likely to churn?
- How long do customers typically stay active?
- What actions can improve customer retention?

## 📊 Dataset
**Source:** [Telco Customer Churn Dataset (Kaggle)](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)

This dataset represents a telecom subscription business, containing 7,043 customer records with demographic information, account details (contract type, tenure, payment method), subscribed services, and churn status.

**Original columns include:** `customerID`, `gender`, `SeniorCitizen`, `Partner`, `Dependents`, `tenure`, `Contract`, `PaymentMethod`, `PaperlessBilling`, service columns (`PhoneService`, `InternetService`, `OnlineSecurity`, `TechSupport`, `StreamingTV`, etc.), `MonthlyCharges`, `TotalCharges`, and `Churn`.

## 🛠️ Tools Used
- **Jupyter Notebook (Python, pandas)** — data cleaning, feature engineering, and exploratory analysis
- **Power BI Desktop** — dashboard building, DAX measures, and visualization

## 🧹 Data Cleaning Process (Jupyter Notebook)

1. **Fixed `TotalCharges` data type** — 11 rows had blank values (all corresponding to customers with `tenure = 0`, i.e., brand-new customers not yet billed). Converted the column to numeric and filled these blanks with 0.
2. **Standardized `SeniorCitizen`** — converted from 0/1 to "No"/"Yes" for consistency with other categorical columns.
3. **Verified no duplicate `customerID`s** existed.
4. **Reviewed "No internet service" / "No phone service" labels** — kept these as distinct categories rather than simplifying to "No," since they carry meaningfully different information (customer lacks the base service entirely, vs. has the service but declined an add-on).

## ⚙️ Feature Engineering (Jupyter Notebook)

- **`TenureBuckets`** — bucketed `tenure` into ranges (0-12, 13-24, 25-36, 37-48, 49-60, 61-72 months) for cohort-style lifetime analysis, with the `tenure = 0` edge case explicitly handled (`include_lowest=True`).
- **`ChurnFlag`** — numeric (0/1) version of the `Churn` column, enabling churn rate calculations as an average.
- **`TotalServices`** — count of services each customer subscribes to (Phone, Internet, and 7 add-on services), ranging from 1 to 9, used to test whether service adoption correlates with retention.
- **`MonthlyChargesGroup`** — customers bucketed into Low / Medium / High spend tiers using the 25th and 75th percentiles of `MonthlyCharges`.

Cleaned data was exported to CSV and loaded into Power BI for dashboard building.

## 📈 Dashboard Overview

### KPI Cards
- Total Customers
- Churned Customers
- Churn Rate
- Average Tenure (Months)
- Average Monthly Charges

### Visuals
| Visual | Purpose |
|---|---|
| **Churn vs Retained Overview** (Donut) | Overall churn split |
| **Customer Distribution by Tenure / Contract** | Context on segment sizes |
| **Churn Rate by Contract Type** | Identifies contract-based risk |
| **Churn Rate by Tenure Group** | Identifies lifecycle-stage risk |
| **Churn Rate by Internet Service** | Identifies service-based risk |
| **Churn Rate by Payment Method** | Identifies payment-based risk |
| **Churn Rate by Total Services** | Tests engagement depth vs. churn |
| **Churn Rate by Monthly Charges Group** | Tests pricing tier vs. churn |
| **Churn Rate by Demographics** | Senior Citizen / Partner / Dependents breakdown |

### Interactivity
- **Contract slicer**, **Internet Service slicer**, **Tenure Group slicer** — all dropdown style, filtering the entire dashboard

A PDF export of the dashboard is included in the `report/` folder, alongside a PNG screenshot in `screenshots/`.

## 💡 Key Insights & Recommendations

- **Month-to-month contracts drive churn** — customers on month-to-month plans churn at 42.7%, nearly 15x higher than two-year contract holders (2.8%), and make up 55% of the entire customer base. Recommend incentivizing longer-term contracts through loyalty discounts or migration offers.
- **Churn is highest in the first year** — customers in their first 12 months show the steepest churn risk, declining sharply as tenure increases. Retention efforts should be front-loaded into the first year, particularly the first few months.
- **Fiber optic customers are the highest-risk segment** — despite being the premium service, fiber optic churns at 41.9%, nearly double DSL (19.0%) and 6x higher than customers with no internet service (7.4%). This warrants investigation into pricing, reliability, or competitive pressure specific to fiber markets.
- **Electronic check payments strongly correlate with churn** — customers paying via electronic check churn at 45.3%, roughly 3x higher than customers on automatic payment methods (bank transfer: 16.7%, credit card: 15.2%). Migrating these customers to autopay could be one of the most effective retention levers available.
- **Service adoption follows a critical risk window** — churn peaks at 44.9% for customers with just 3 total services, then steadily declines to 5.3% for customers with 9 services. Customers with basic internet but few add-ons (like security or tech support) represent the highest-risk zone — upselling these services could meaningfully reduce churn.
- **Household structure matters** — senior citizens churn at 41.7% (vs. 23.6% for non-seniors), and customers without a partner (33.0%) or dependents (31.3%) churn roughly twice as often as those with a partner (19.7%) or dependents (15.5%). Single, independent customers and seniors represent segments worth targeted retention outreach.

## 📁 Repository Structure
```
FUTURE_DS_02/
│
├── README.md
│
├── .gitignore
│
├── dataset/
│   ├── telco_customer_churn_raw.csv
│   └── telco_customer_churn_cleaned.csv
│
├── notebooks/
│   └── churn_analysis.ipynb
│
├── powerbi/
│   └── churn_retention_dashboard.pbix
│
├── screenshots/
│   └── dashboard_overview.png
│
└── report/
    └── dashboard_overview.pdf
```

## 📝 Note on Cohort Analysis
The task brief referenced cohort analysis "by signup month, plan, or region." Since this dataset does not include explicit signup date or region fields, cohort-style analysis was approached through **Contract type** (as a proxy for "plan") and **Tenure Group** (as a proxy for lifecycle-stage cohorts), which the dataset's structure supports directly.