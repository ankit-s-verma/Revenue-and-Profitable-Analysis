# E-Commerce Revenue & Customer Behaviour Analytics

> An end-to-end analytics project covering data exploration, cleaning, feature engineering, SQL-driven business analysis, and Power BI dashboard development.

---

## Project Overview

This project analyses customer purchasing behaviour and revenue patterns for an e-commerce retail company using a dataset of **3,900 transactions across 19 features**. The pipeline moves from raw CSV ingestion through Python-based EDA, PostgreSQL integration, structured SQL querying, and a fully interactive Power BI dashboard — producing a coherent set of evidence-backed business insights and recommendations.

**Dataset:** Customer Shopping Behaviour · 3,900 Transactions · 19 Features  
**Source:** [GitHub — Consumer Shopping Behavior Dataset](https://github.com/amlanmohanty1/customer-trends-data-analysis-SQL-Python-PowerBI/blob/main/customer_shopping_behavior.csv)

---

## Key Performance Indicators

| Total Customers | Avg Purchase Amount | Total Revenue | Avg Review Rating |
|:-:|:-:|:-:|:-:|
| 3,900 | $59.76 | $233,081 | 3.75 / 5.0 |

---

## Tech Stack

| Layer | Tools |
|---|---|
| Data Preparation & EDA | Python · Pandas · NumPy · Matplotlib · Seaborn |
| Database | PostgreSQL · SQLAlchemy · python-dotenv |
| Business Analysis | SQL (10 structured queries) |
| Visualisation | Power BI |
| Version Control | Git · GitHub |

---

## Repository Structure

```
Revenue-and-Profitable-Analysis/
│
├── Screenshots/
│   └── power-bi-dashboard.jpeg
│
├── reports/
│   └── ECommerce_Revenue_Analytics_Report.pdf
│
├── src/
│   ├── databse/
│   │   └── SQL-Driven Business Analysis.sql
│   ├── power-bi/
│   │   └── customer_behaviour_dashboard.pbix
│   └── scripts/
│       ├── Revenue_EDA.ipynb
│       ├── Updated_Revenue_Dataset.csv
│       └── customer_shopping_behavior.csv
│
├── .gitignore
└── README.md
```

---

## Project Workflow

### Phase 1 — Data Understanding
Loaded the raw CSV into a Pandas DataFrame. Confirmed 17 original fields, identified `review_rating` null values requiring imputation, and confirmed `promo_code_used` as a perfect duplicate of `discount_applied` — dropped during cleaning.

### Phase 2 — Data Cleaning
- **Null imputation:** `review_rating` filled using per-category median via `groupby().transform()` to preserve category-level variance.
- **Column standardisation:** names converted to lowercase `snake_case` for PostgreSQL compatibility.
- **Column removal:** `promo_code_used` dropped as a confirmed duplicate.
- Cleaned dataset exported to `Updated_Revenue_Dataset.csv`.

### Phase 3 — Exploratory Data Analysis (EDA)
Eleven plots produced across three analytical layers:

**Univariate:** Item count distribution · Age group distribution · Seasonal transaction volume

**Bivariate:** Age group vs. previous purchases · Review rating vs. purchase frequency · Age group by subscription status · Previous purchases vs. shipping type · Purchase amount by category (bar plot) · Purchase amount distribution by gender (histogram + KDE)

**Multivariate:** Numeric feature correlation heatmap · Age group × category count heatmap (cross-tabulation)

### Phase 4 — Feature Engineering
Two features engineered to enhance analytical depth:
- **`age_group`** — quartile-based age segmentation (`pd.qcut`) into Young Adult / Adult / Middle Age / Senior for balanced group sizes.
- **`purchase_frequency_days`** — numeric translation of `frequency_of_purchases` (e.g., Weekly → 7, Monthly → 30) enabling quantitative frequency analysis.

### Phase 5 — Database Integration
Cleaned DataFrame pushed to a locally hosted PostgreSQL database (`revenue_db`) via SQLAlchemy. Database credentials managed securely using `python-dotenv`.

---

## SQL-Driven Business Analysis

Ten structured analytical queries executed against the `revenue_data` table in PostgreSQL:

| Q# | Question | Key Finding |
|---|---|---|
| Q1 | Revenue by Gender | Male: $157,890 vs Female: $75,191 — mirrors the 68:32 customer split |
| Q2 | High-Spend Discount Users | Discount users still spending above the dataset average — a deal-responsive high-value segment |
| Q3 | Top 5 Products by Avg Rating | Gloves (3.86), Sandals (3.84), Boots (3.82), Hat (3.80), Skirt (3.78) |
| Q4 | Standard vs Express Shipping | Express: $60.48 avg vs Standard: $58.46 avg — consistent $2.02 premium |
| Q5 | Subscription vs Non-Subscription | Near-identical avg spend ($59.49 vs $59.87) despite 73/27 customer split |
| Q6 | Highest Discount Rates | Hat (50%), Sneakers (49.66%), Coat (49.07%), Sweater (48.17%) |
| Q7 | Customer Segmentation | Loyal: 3,116 · Returning: 701 · New: 83 |
| Q8 | Top 3 Products per Category | Uses `ROW_NUMBER()` window function partitioned by category |
| Q9 | Repeat Buyers & Subscription | 2,518 of 3,476 repeat buyers (72.4%) are non-subscribers — major monetisation gap |
| Q10 | Revenue by Age Group | Young Adult: $62,143 · Middle Age: $59,197 · Adult: $55,978 · Senior: $55,763 |

---

## Power BI Dashboard

The Customer Behaviour Dashboard connects to `Updated_Revenue_Dataset.csv` and presents insights in an interactive single-page layout:

- **KPI Cards:** Total Customers · Average Purchase Amount · Average Review Rating
- **Donut Chart:** Subscription status split (73% Non-subscriber / 27% Subscriber)
- **Bar Charts:** Sales by Category · Revenue by Category
- **Horizontal Bar Charts:** Revenue by Age Group · Sales by Age Group
- **Sidebar Filters:** Subscription Status · Gender · Category · Shipping Type

---

## Key Findings & Recommendations

**1. Clothing Dominates; Outerwear Needs Attention**  
Clothing accounts for 44.7% of total revenue ($104,264). Outerwear generates only $18,524 — the smallest category by both volume and revenue. Cross-sell opportunities with Clothing during peak Fall season should be explored.

**2. Subscription Penetration is Low Despite a Loyal Base**  
Only 27% of customers subscribe, yet 80% are classified as Loyal (>10 purchases). Average spend is near-identical across subscriber/non-subscriber groups ($59.49 vs $59.87), meaning the programme is not driving incremental spend. The 2,518 repeat buyers who are not subscribed represent the highest-potential conversion cohort.

**3. Discount Strategy May Be Eroding Margin**  
Half of all Hat and Sneaker transactions include a discount. At this penetration rate, customers may be conditioned to wait for promotions. Selective discount withdrawal tests and shift toward Free Shipping incentives are recommended.

**4. Fall is Peak Revenue; Summer is the Trough**  
Fall generates $60,018 vs Summer's $55,777 — a 7.6% seasonal swing. Inventory and promotional planning should be aligned with this pattern.

**5. Revenue is Broadly Even Across Age Groups**  
The four age groups contribute between $55,763 and $62,143 — an 11.4% spread — indicating genuine cross-generational product appeal.

**6. Male Customers Dominate but Female Segment is Underserved**  
Male customers represent 68% of the base. However, female customers spend at a comparable average rate when normalised by count ($60.24 vs $59.56). The purchase amount histogram confirms near-identical KDE distributions across the full $20–$100 range — the revenue gap is driven entirely by customer count, not individual spend.

---

## Setup & Reproduction

### Prerequisites
```
Python 3.8+
PostgreSQL 13+
Power BI Desktop
```

### Python Dependencies
```bash
pip install pandas numpy matplotlib seaborn sqlalchemy python-dotenv psycopg2-binary
```

### Database Configuration
Create a `.env` file in the project root:
```
DB_USER=your_username
DB_PASSWORD=your_password
DB_HOST=127.0.0.1
DB_PORT=5432
DB_NAME=revenue_db
```

### Run the Notebook
```bash
jupyter notebook src/scripts/Revenue_EDA.ipynb
```

The notebook will clean the data, engineer features, run EDA, and push the cleaned DataFrame to PostgreSQL. SQL queries can then be executed against the `revenue_data` table.

---

## Limitations

- **No time dimension** — the dataset lacks transaction dates, preventing time-series or cohort analysis.
- **No profitability data** — purchase amount is recorded but not cost of goods sold, so margin analysis is not possible.
- **Descriptive only** — relationships identified are correlational, not causal.
- **No geographic visualisation** — a location column exists but was not incorporated into the current dashboard.

---

## Future Scope

- **Predictive Subscription Model** — binary classifier to identify non-subscribed customers most likely to convert.
- **Market Basket Analysis** — association rule mining (Apriori / FP-Growth) for cross-sell recommendation.
- **Customer Lifetime Value (CLV)** — forward-looking CLV estimation using `purchase_frequency_days` and average order value.
- **Geographic Revenue Map** — Power BI map visual using the location field.
- **Seasonal Demand Forecasting** — if transaction dates become available in future data.

---

## Author

**Ankit Verma**  
[GitHub](https://github.com/ankit-s-verma)
