# RavenStack SaaS Subscription & Churn Analysis

## 1. Project Overview

This project analyzes customer subscription and churn behavior for RavenStack, a SaaS company, using relational data across 500 customer accounts, 5,000 subscription records, 25,000 product usage events, 2,000 support tickets, and 600 churn events. The goal is to uncover the drivers of customer churn, evaluate revenue health, and assess support and product engagement patterns to guide retention and acquisition strategy.

## 2. Dataset Summary

- **Tables:** 5 (accounts, subscriptions, feature_usage, support_tickets, churn_events)
- **Accounts:** 500 rows, 10 columns
- **Subscriptions:** 5,000 rows, 14 columns
- **Feature Usage:** 25,000 rows, 8 columns
- **Support Tickets:** 2,000 rows, 9 columns
- **Churn Events:** 600 rows, 9 columns
- **Key Features:**
  - Customer attributes (Industry, Country, Referral Source, Plan Tier, Seats, Trial Status)
  - Subscription details (MRR/ARR Amount, Start/End Date, Upgrade/Downgrade Flags, Billing Frequency)
  - Support behavior (Resolution Time, First Response Time, Satisfaction Score, Escalation Flag)
  - Churn details (Churn Date, Reason Code, Refund Amount, Reactivation Flag)
- **Missing Data:** Satisfaction scores missing for accounts with zero support tickets (32 accounts); feedback_text optional field with nulls; subscriptions.end_date null for the majority of active line items

## 3. Exploratory Data Analysis using Python

We began with data preparation and validation in Python:

- **Data Loading:** Imported all five tables from PostgreSQL using SQLAlchemy.
- **Initial Exploration:** Used `df.info()` and `.describe()` across all tables to check structure, types, and summary statistics.
- **Missing Data Handling:** Identified that `satisfaction_score` nulls represent customers who never responded to a survey, not a score of zero — left as null rather than imputed, to avoid manufacturing a false signal.
- **Data Quality Validation:** Tested whether `accounts.churn_flag` aligned with `subscriptions.end_date` as expected. Found churned and active accounts had nearly identical open-subscription counts (~9 each), disproving the assumption. Switched to `churn_events.churn_date` as the sole reliable exit signal.
- **Date Validation:** Checked for subscriptions starting after they ended, and subscriptions starting before account signup — zero violations found.
- **Feature Engineering:**
  - Created `months_as_customer` using right-censored survival logic (churned accounts measured to exit date; active accounts measured to the dataset's observed ceiling).
  - Created `ltv` as average MRR × months as customer.
  - Created `customer_metrics`, a customer-level analytical table aggregating all five source tables to one row per account.
- **Data Consistency Check:** Verified `average_arr` always equals `average_mrr × 12`; verified zero negative LTV values; verified zero duplicate account IDs.
- **Database Integration:** Loaded the cleaned `customer_metrics` table back into PostgreSQL and exported to CSV for Power BI.

## 4. Data Analysis using SQL (Business Questions)

We performed structured analysis in PostgreSQL/SQLite to answer key business questions:

**1. Churn Rate by Plan Tier** – Compared churn rate across Basic, Pro, and Enterprise accounts.

| plan_tier | total_accounts | churned_accounts | churn_rate_pct |
|---|---|---|---|
| Enterprise | 154 | 34 | 22.1 |
| Basic | 168 | 37 | 22.0 |
| Pro | 178 | 39 | 21.9 |

**2. Churn Rate by Referral Source** – Identified which acquisition channels retain customers best.

| referral_source | total_accounts | churned_accounts | churn_rate_pct |
|---|---|---|---|
| event | 96 | 29 | 30.2 |
| other | 103 | 25 | 24.3 |
| ads | 98 | 23 | 23.5 |
| organic | 114 | 20 | 17.5 |
| partner | 89 | 13 | 14.6 |

**3. Top Churn Reasons** – Found the most common reasons customers cancel.

| reason_code | event_count | pct_of_events |
|---|---|---|
| features | 114 | 19.0 |
| support | 104 | 17.3 |
| budget | 104 | 17.3 |
| unknown | 95 | 15.8 |
| competitor | 92 | 15.3 |
| pricing | 91 | 15.2 |

**4. LTV by Referral Source** – Compared average customer lifetime value across acquisition channels.

| referral_source | avg_ltv | n_accounts |
|---|---|---|
| partner | 24,965 | 88 |
| ads | 23,965 | 97 |
| organic | 21,654 | 113 |
| other | 21,251 | 97 |
| event | 20,276 | 92 |

**5. Upgrade/Downgrade Behavior Preceding Churn** – Tested whether churned accounts had recently upgraded or downgraded.

| churns_after_upgrade | churns_after_downgrade | total_churn_events |
|---|---|---|
| 123 (20.5%) | 53 (8.8%) | 600 |

**6. Data Quality Check — Account Exit Date Resolution** – Tested whether `churn_flag` accounts had a usable exit timestamp.

| churn_flag = 1 accounts | with churn_events row | with NO churn_events row |
|---|---|---|
| 110 | 75 (68%) | 35 (32%) |

**7. Reactivation Signal Check** – Tested whether currently-active accounts had churned previously.

| churn_flag | total_accounts | accounts_with_churn_event_history |
|---|---|---|
| 0 (active) | 390 | 277 (71%) |
| 1 (churned) | 110 | 75 |

## 5. Dashboard in Power BI

Finally, we built a single, interactive executive dashboard in **Power BI** to present insights visually.

**Dashboard sections:**
- Executive KPI strip (Total Customers, Average Satisfaction, Churn Rate, Retention Rate, Average MRR)
- Customer & Revenue Overview (plan tier mix, MRR by industry, referral source breakdown)
- Churn Analysis (churn rate by referral source, churn reasons, churn rate by plan tier)
- Customer Success & Product Usage (resolution time vs. satisfaction, usage by churn status)

**Final KPIs:**

| Metric | Value |
|---|---|
| Total Customers | 465 |
| Average Satisfaction Score | 3.95 |
| Churn Rate | 16% |
| Retention Rate | 84% |
| Average MRR | $2.30K |

## 6. Business Recommendations

- **Reallocate Acquisition Spend** – Shift signup volume toward partner-sourced channels, which churn at roughly half the rate of event-sourced channels (14.6% vs. 30.2%) and carry the highest average LTV ($24,965).
- **Deprioritize Plan-Tier-Based Retention Tactics** – Churn is statistically flat across Basic, Pro, and Enterprise (~22% each), so pricing-tier interventions are unlikely to move retention.
- **Investigate Event-Channel Lead Quality** – Since usage and support activity don't distinguish churners from retained customers, the gap likely originates at acquisition (lead fit), not in-product engagement.
- **Watch for Post-Upgrade Churn** – Upgrade-then-churn (20.5% of churn events) is more than twice as common as downgrade-then-churn (8.8%), suggesting "buyer's remorse" after upsells deserves a closer look in the customer success process.
- **Close the Churn-Event Data Gap** – 32% of churned accounts have no logged churn reason, limiting root-cause diagnosis; improving this capture should be an operational priority.
