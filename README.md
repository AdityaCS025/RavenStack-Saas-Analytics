# 📊 RavenStack SaaS Subscription & Churn Analysis

An end-to-end Data Analytics project focused on understanding customer churn, retention, customer lifetime value (LTV), and revenue performance in a SaaS business environment.

## 🚀 Project Overview

RavenStack is a fictional SaaS company facing customer retention challenges. The objective of this project was to analyze subscription, usage, support, and churn data to uncover the key drivers of churn and provide actionable business recommendations.

This project demonstrates the complete analytics workflow:

* Data Extraction & Validation
* Data Cleaning & Feature Engineering
* SQL-Based Business Analysis
* Customer Lifetime Value (LTV) Modeling
* Churn & Retention Analysis
* Interactive Power BI Dashboard Development

---

## 🎯 Business Objectives

* Identify the major drivers of customer churn
* Analyze retention across customer segments
* Evaluate revenue health and customer lifetime value
* Understand the impact of acquisition channels
* Provide actionable recommendations to improve retention

---

## 🛠️ Tech Stack

| Tool       | Purpose                                        |
| ---------- | ---------------------------------------------- |
| Python     | Data Cleaning, Validation, Feature Engineering |
| Pandas     | Data Manipulation                              |
| SQLAlchemy | Database Connectivity                          |
| PostgreSQL | Data Storage & Business Analysis               |
| SQL        | KPI Calculations & Queries                     |
| Power BI   | Dashboard & Visualization                      |

---

## 📂 Dataset

Source: Kaggle SaaS Subscription & Churn Analytics Dataset

### Dataset Structure

| Table           | Records |
| --------------- | ------- |
| Accounts        | 500     |
| Subscriptions   | 5,000   |
| Feature Usage   | 25,000  |
| Support Tickets | 2,000   |
| Churn Events    | 600     |

The dataset contains customer demographics, subscription history, product usage, support interactions, and churn information.

---

## 🔄 Data Pipeline

### 1. Extract

* Imported all tables from PostgreSQL using SQLAlchemy.

### 2. Transform

* Data validation
* Missing value analysis
* Churn signal verification
* Feature engineering

### 3. Load

* Created a customer-level analytical dataset (`customer_metrics`)
* Exported cleaned data to PostgreSQL and CSV

### 4. Visualize

* Built an interactive executive dashboard in Power BI

---

## 📈 Key KPIs

| Metric                     | Value  |
| -------------------------- | ------ |
| Total Customers            | 465    |
| Churn Rate                 | 16%    |
| Retention Rate             | 84%    |
| Average Satisfaction Score | 3.95   |
| Average MRR                | $2.30K |

---

## 🔍 Key Insights

### Customer Acquisition Matters More Than Pricing

* Partner-sourced customers showed the lowest churn rate (**14.6%**)
* Event-sourced customers showed the highest churn rate (**30.2%**)

### Plan Tier Does Not Significantly Impact Churn

| Plan       | Churn Rate |
| ---------- | ---------- |
| Basic      | 22.0%      |
| Pro        | 21.9%      |
| Enterprise | 22.1%      |

### Top Churn Reasons

* Features
* Support
* Budget Constraints
* Competitor Products
* Pricing Concerns

### Customer Lifetime Value

Partner-referred customers generated the highest average LTV:

**$24,965**

---

## 📊 Power BI Dashboard

The dashboard includes:

### Executive KPIs

* Total Customers
* Churn Rate
* Retention Rate
* Average Satisfaction
* Average MRR

### Revenue Analysis

* MRR by Industry
* Customer Distribution by Plan

### Churn Analysis

* Churn Rate by Referral Source
* Churn Rate by Plan Tier
* Churn Reason Breakdown

### Customer Success Metrics

* Resolution Time vs Satisfaction
* Product Usage vs Churn Status

---

## 💡 Business Recommendations

### 1. Increase Investment in Partner Channels

Partner-acquired customers have both the highest LTV and lowest churn.

### 2. Improve Event-Based Lead Quality

Event-generated leads churn at twice the rate of partner-generated customers.

### 3. Monitor Post-Upgrade Churn

20.5% of churn events occurred after upgrades, indicating potential buyer's remorse.

### 4. Improve Churn Data Collection

32% of churned customers lacked a recorded churn reason.

### 5. Focus on Acquisition Quality

The analysis suggests churn is driven more by customer fit than pricing strategy.

---

## 📁 Repository Structure

```text
RavenStack-SaaS-Analytics/
│
├── data/
├── sql/
├── notebooks/
├── dashboard/
├── reports/
├── README.md
│
└── RavenStack.pbix
```

---

## 📌 Key Skills Demonstrated

* Data Analytics
* Exploratory Data Analysis (EDA)
* SQL Querying
* Data Validation
* Feature Engineering
* Customer Churn Analysis
* Customer Lifetime Value (LTV)
* Business Intelligence
* Power BI Dashboarding
* Data Storytelling

---

## 📬 Connect With Me

www.linkedin.com/in/aditya-singh-27957a361

Aspiring Data Analyst | Python | SQL | Power BI | Business Intelligence

If you found this project interesting, feel free to ⭐ the repository and connect with me on LinkedIn.
