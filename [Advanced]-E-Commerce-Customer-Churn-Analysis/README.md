# 🛒 E-COMMERCE CUSTOMER CHURN ANALYSIS

An exploratory SQL case study investigating customer churn patterns in an e-commerce business. This project examines customer demographics, engagement, purchasing behavior, and customer experience to identify retention opportunities and support data-driven business decisions.

## 📌 Background

Customer churn occurs when customers stop doing business with a company, making customer retention a key concern for long-term business performance. In e-commerce, where customers have numerous alternatives, retaining existing customers requires businesses to understand customer behavior, purchasing preferences, and overall shopping experience.

This project examines customer churn patterns to identify customer segments associated with higher attrition. By analyzing factors such as customer tenure, satisfaction, complaints, payment preferences, and purchasing behavior, the analysis aims to uncover patterns that can inform targeted retention strategies, improve customer engagement, and support long-term customer loyalty.

## 🎯 Project Objectives
- Measure the overall customer churn rate and retention rate.
- Identify customer segments associated with higher churn.
- Examine relationships between customer behavior, purchasing preferences, and churn.
- Translate analytical findings into actionable business recommendations.

## 🛠️ Project Approach
The dataset was sourced from [Kaggle's E-commerce Customer Churn Analysis and Prediction](https://www.kaggle.com/datasets/ankitverma2010/ecommerce-customer-churn-analysis-and-prediction) dataset. It contains 5,630 customer records, including customer demographics, satisfaction scores, preferred payment methods, order activity, and cashback amounts.

SQL was used to clean, validate, and analyze the data, with the analysis organized into the following stages:
1. **[Data Cleaning & Preparation](#data-cleaning)** — Assessed data quality, investigated missing values, and standardized inconsistent categories.
2. **[Exploratory Data Analysis](#exploratory-data-analysis)** — Examined churn patterns across customer demographics, engagement, purchasing behavior, and customer experience.
3. **[Key Findings](#key-findings)** — Identified notable trends and customer segments associated with higher or lower churn.
4. **[Business Recommendations](#recommendations)** — Developed potential retention strategies based on the observed patterns.

## 🗂️ Dataset Overview

The dataset contains customer-level information covering demographics, platform engagement, purchasing behavior, satisfaction, and churn status.

| Category | Variables |
|---|---|
| Customer Profile | Gender, Marital Status, City Tier |
| Customer Tenure | Tenure (Months) |
| Platform Engagement | Hour Spend on App, Number of Registered Devices |
| Purchasing Behavior | Order Count, Coupon Usage, Preferred Order Category |
| Customer Experience | Satisfaction Score, Complaints |
| Logistics | Warehouse-to-Home Distance, Days Since Last Order |
| Incentives | Cashback Amount |
| Target Variable | Churn |

**Dataset Size:** 5,630 customer records, as reported in the reference analysis.

<a id="data-cleaning"></a>
## 🧹 Data Cleaning & Preparation Summary

Before conducting the analysis, SQL was used to assess data quality and prepare the dataset for reliable querying.

Key preparation steps included:
- **Data Integrity**: Checked customer identifiers for duplicate records.
- **Missing Values**: Identified null values in numerical columns and evaluated imputation approaches.
- **Category Standardization**: Resolved inconsistent labels in preferred login devices, payment methods, and order categories.
- **Value Validation**: Inspected extreme values in the `warehouse_to_home` distance metric, choosing to retain them as valid operational edge cases.
- **Data Transformation**: Used conditional logic (`CASE` statements) to create readable customer status (`churn_label`) and complaint (`complaint_label`) indicators.

These steps established a consistent foundation for the subsequent exploratory analysis.

> To review the complete SQL scripts, query tables, and execution outputs for each of these sections, please visit [Data Cleaning Query Results Log](https://github.com/franceslorraine/SQL-Solutions/blob/29a95213790cbdc3151c680affd6d27b34a4c0ec/%5BAdvanced%5D-E-Commerce-Customer-Churn-Analysis/Data-Cleaning-queries.md).

<a id="exploratory-data-analysis"></a>
## 🧐 Exploratory Data Analysis

| Part | Analysis Category | Description | Business Questions | 
| :--- | :--- | :--- | :--- |
| **1** | **Overall Churn Performance** | Establishes the baseline dataset metrics and evaluates broad behavioral differences between retained and churned customers. | • What proportion of customers have churned versus retained?<br>• How do baseline metrics (tenure, order counts, coupons, cashback, satisfaction) compare between churned and retained customers? |
| **2** | **Customer Demographics & Location** | Examines how geographic location, gender, and life stage influence customer retention and attrition patterns. | • Are churn rates different across city tiers (Tier 1, 2, 3)?<br>• Do churn rates differ by gender?<br>• Which marital-status segment exhibits the highest observed churn rate? |
| **3** | **Customer Engagement & Experience** | Evaluates how customer tenure, device usage, customer service touchpoints, app engagement, and order recency impact retention and churn behavior. | • Are newer customers (shorter tenure) more likely to churn than long-standing customers?<br>• Is the number of registered devices associated with differences in churn?<br>• Do customers who file complaints show higher churn rates?<br>• How does observed churn vary across customer satisfaction scores?<br>• Do churned and retained customers spend different amounts of time on the app?<br> • Is a longer interval (recency) since the last order associated with a higher churn rate? |
| **4** | **Purchasing Behavior & Incentives** | Examines how customer shopping preferences, login devices, payment methods, order categories, logistics distances, and incentive programs relate to customer attrition. | • Which preferred login devices are associated with higher churn rates? <br> • Which preferred payment modes have the highest observed churn rates? <br> • Which preferred order categories drive higher churn? <br> • Does churn vary with the distance between customers and warehouses? <br> • Do churned and retained customers differ in average coupon usage? <br> • Does churn vary across cashback amount ranges? <br> • Does the reported increase in order amount from last year differ between segments? |
| **5** | **Advanced Segmentation & Summary** | Moves beyond single-variable analysis by examining the intersection of multiple risk factors to pinpoint exact high-risk customer segments. | • Which multi-dimensional intersections of customer tenure, complaint status, and city tier identify the highest-risk customer segments? |

> To review the complete SQL scripts, query tables, and execution outputs for each of these sections, please visit [EDA Query Results Log](https://github.com/franceslorraine/SQL-Solutions/blob/29a95213790cbdc3151c680affd6d27b34a4c0ec/%5BAdvanced%5D-E-Commerce-Customer-Churn-Analysis/EDA-queries.md).


<a id="key-findings"></a>
## 📊 Key Findings

| No. | Analysis Category | Key Insight & Quantitative Summary |
| :--- | :--- | :--- |
| **1** | **The Onboarding Cliff** | Tenure is the single strongest predictor of retention. Churn is concentrated in the first 6 months of the customer lifecycle (32.42%), dropping to near-zero for customers with over 2 years of tenure. |
| **2** | **Service Recovery Deficit** | Customers who file complaints experience a 31.67% churn rate (compared to 10.93% for non-complainers). When combined with early tenure, complaint filings drive churn rates above 60%. |
| **3** | **Product & Fulfillment Vulnerabilities** | Mobile category purchases (27.40% churn), Cash on Delivery payments (24.90% churn), and long warehouse-to-home distances (20.81% churn for 31+ units) represent significant operational friction points. |
| **4** | **Demographic Risk Profiles** | Single customers (26.73% churn) and Tier 3 city residents (21.37% churn) show substantially higher attrition compared to married users and Tier 1 metropolitan customers. |
| **5** | **Incentive Effectiveness** | High cashback tiers ($300+) successfully suppress churn down to 6.17%, whereas basic coupon usage and app screen time do not differentiate retained users from churned users. |

<a id="recommendations"></a>
## 🧾 Recommendations

| Focus Area | Operational Challenge | Recommended Strategic Intervention | Expected Impact |
| :--- | :--- | :--- | :--- |
| **1. Onboarding & Early Lifecycle** | 32.42% churn rate concentrated within the first 6 months of customer tenure. | Deploy automated, time-triggered welcome sequences, onboarding tutorials, and check-ins during the first 30–90 days. | Mitigates the primary customer leakage point and increases 12-month retention. |
| **2. Customer Service Recovery** | Complainers face a 31.67% churn rate, compounding to >60% for new users. | Implement an SLA-driven priority resolution queue for complaints from early-tenure accounts. Intercept tickets with automated goodwill credits. | Prevents service recovery failures from driving high-value customer defection. |
| **3. Fulfillment & Logistics** | Higher warehouse-to-home distances (31+ units) correlate with 20.81% churn. | Optimize regional fulfillment routing, establish micro-warehouses near Tier 3 / high-distance zones, and improve delivery tracking visibility. | Reduces shipping friction and delivery-delay dissatisfaction. |
| **4. Payment Risk Mitigation** | Cash on Delivery (COD) and E-Wallets exhibit elevated churn (~23–25%). | Incentivize migration from COD to structured digital payment methods (Credit/Debit cards) through targeted cashback bonuses or free shipping perks. | Lowers transaction risk and aligns customers with high-retention payment channels. |
| **5. Targeted Incentive Structuring** | High cashback tiers successfully lower churn to 6.17%, while blanket coupons have no impact. | Restructure marketing budgets away from generic coupon distribution and reallocate funds into tiered cashback loyalty programs for high-risk segments (e.g., Single users in Tier 3 cities purchasing Mobile goods). | Maximizes ROI on retention spending by directly rewarding long-term platform engagement. |


## ⚙️ Tools & Technologies & Skills Demonstrated
| Category | Tools & Technologies | Skills Demonstrated |
| :--- | :--- | :--- |
| **Database & Cloud Environment** | Supabase, PostgreSQL | • Relational database configuration and schema management <br> • Query optimization and execution in a cloud PostgreSQL environment |
| **Data Engineering & Hygiene** | SQL, DDL / DML | • Data cleaning, handling missing values via median imputation <br> • Schema standardization and strict snake_case naming conventions <br> • Feature engineering (creating pre-calculated helper indicator columns like `churn_label` and `complain_label`)  <br> • Outlier evaluation and decision-making on operational edge cases |
| **Advanced SQL Querying** | Aggregate Functions, Filters, CTEs, Window Functions | • Conditional aggregation using `FILTER (WHERE ...)` clauses <br> • Dynamic bucketing and segmentation using Common Table Expressions (`WITH` CTEs)  <br> • Multi-dimensional grouping and `HAVING` clause filtering for robust sample size thresholds <br> • Cross-dimensional consolidation using `UNION ALL` |
| **Data Analysis & Business Intelligence** | Exploratory Data Analysis (EDA), Statistical Aggregation | • Macro-to-micro exploratory profiling of customer retention vs. churn <br> • Identifying high-risk cohort intersections and compound risk multipliers  <br> • Translating technical query outputs into executive-ready business insights and retention strategies |

