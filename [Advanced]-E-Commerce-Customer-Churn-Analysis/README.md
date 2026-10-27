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
| Customer Tenure | Tenure |
| Platform Engagement | Hour Spend on App, Number of Registered Devices |
| Purchasing Behavior | Order Count, Coupon Usage, Preferred Order Category |
| Customer Experience | Satisfaction Score, Complaints |
| Logistics | Warehouse-to-Home Distance, Days Since Last Order |
| Incentives | Cashback Amount |
| Target Variable | Churn |

**Dataset Size:** 5,630 customer records, as reported in the reference analysis.

<a id="data-cleaning"></a>
## 🧹 Data Cleaning & Preparation

Before conducting the analysis, SQL was used to assess data quality and prepare the dataset for reliable querying.

Key preparation steps included:
- **Data Integrity**: Checked customer identifiers for duplicate records.
- **Missing Values**: Identified null values in numerical columns and evaluated imputation approaches.
- **Category Standardization**: Resolved inconsistent labels in preferred login devices, payment methods, and order categories.
- **Value Validation**: Inspected extreme values in the `warehouse_to_home` distance metric, choosing to retain them as valid operational edge cases.
- **Data Transformation**: Used conditional logic (`CASE` statements) to create readable customer status and complaint indicators.

These steps established a consistent foundation for the subsequent exploratory analysis.

## 🔎 Business Questions

The analysis explores customer churn through four areas:
| No. | Analysis Category | Key Questions |
| :--- | :--- | :--- |
| **1** | **Overall Churn Performance** | • What is the overall customer churn rate?<br>• How many customers stayed versus churned? |
| **2** | **Customer Demographics & Location** | • How does churn vary across city tiers?<br>• Are there differences in churn rates by gender?<br>• Does marital status show a relationship with churn? |
| **3** | **Customer Engagement & Experience** | • How does tenure relate to churn?<br>• Does app usage time differ between churned and retained customers?<br>• Does the number of registered devices relate to churn?<br>• Are customer complaints associated with higher churn?<br>• How do satisfaction scores and days since the last order vary across customer groups? |
| **4** | **Purchasing Behavior & Incentives** | • How does churn vary by preferred login device and payment mode?<br>• Which preferred order categories have higher churn rates?<br>• Does warehouse-to-home distance relate to churn?<br>• How do coupon usage and cashback amounts differ across customer groups? |

<a id="exploratory-data-analysis"></a>
## 🧐 Exploratory Data Analysis

<a id="key-findings"></a>
## 📊 Key Findings

<a id="recommendations"></a>
## 🧾 Recommendations


## ⚙️ Tools & Technologies
- **Cloud Database**: Supabase
- **Query Language**: PostgreSQL


