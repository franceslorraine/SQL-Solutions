# EXPLORATORY DATA ANALYSIS

This presents the queries and results to answer the business questions. 

## 1. Overall Churn Performance
This section establishes the baseline dataset metrics and evaluates broad behavioral differences between retained and churned customers.

### 1.1 Calculate the Overall Churn Rate
This establishes the baseline for the entire analysis.

> **Business Question:** What proportion of customers have churned versus retained?

```sql
SELECT
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Retained') AS retained_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned') / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0), 2
    ) AS churn_rate_pct,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Retained') / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0), 2
    ) AS retention_rate_pct

FROM ecommerce_churn;
```

| total_customers | churned_customers | retained_customers | churn_rate_pct | retention_rate_pct |
| --------------- | ----------------- | ------------------ | -------------- | ------------------ |
| 5630            | 948               | 4682               | 16.84          | 83.16              |

**Key Takeaway:** The baseline analysis reveals a total customer base of 5,630 records. The overall churn rate stands at 16.84% (948 churned users), leaving a strong platform retention rate of 83.16% (4,682 retained users).

### 1.2 Compare Average Customer Metrics by Churn Status

> **Business Question:** How do the characteristics of churned customers differ from those of retained customers?

```sql
SELECT
    CASE
        WHEN Churn = 1 THEN 'Churned'
        WHEN Churn = 0 THEN 'Retained'
        ELSE 'Unknown'
    END AS customer_status,

    COUNT(*) AS customer_count,
    ROUND(AVG(tenure)::NUMERIC, 2) AS avg_tenure,
    ROUND(AVG(hour_spend_on_app)::NUMERIC, 2) AS avg_app_hours,
    ROUND(AVG(order_amount_hike_from_last_year)::NUMERIC, 2) AS avg_order_count,
    ROUND(AVG(coupon_used)::NUMERIC, 2) AS avg_coupon_used,
    ROUND(AVG(day_since_last_order)::NUMERIC, 2) AS avg_days_since_last_order,
    ROUND(AVG(cashback_amount)::NUMERIC, 2) AS avg_cashback,
    ROUND(AVG(satisfaction_score)::NUMERIC, 2) AS avg_satisfaction_score

FROM ecommerce_churn
GROUP BY Churn
ORDER BY Churn;
```

| customer_status | customer_count | avg_tenure | avg_app_hours | avg_order_count | avg_coupon_used | avg_days_since_last_order | avg_cashback | avg_satisfaction_score |
| --------------- | -------------- | ---------- | ------------- | --------------- | --------------- | ------------------------- | ------------ | ---------------------- |
| Retained        | 4682           | 11.40      | 2.93          | 15.69           | 1.72            | 4.71                      | 180.63       | 3.00                   |
| Churned         | 948            | 3.86       | 2.96          | 15.62           | 1.71            | 3.22                      | 160.37       | 3.39                   |

**Key Takeaway:** 
- **Tenure Divergence**: Retained customers maintain a significantly longer average tenure (11.40 months) compared to churned users (3.86 months), indicating that customer acquisition risk is highest early in the lifecycle.
- **Engagement & Incentives**: App usage hours and coupon utilization remain virtually identical across both groups, though retained customers experience higher average cashback rewards (180.63 vs. 160.37).

## 2. Customer Demographics & Location
This section examines how geographic location, gender, and life stage influence customer retention and attrition patterns.

### 2.1 Churn Rate by City Tier

> **Business Question:** Are churn rates different across city tiers?

```sql
SELECT
    city_tier,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE Churn = 1) AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE Churn = 1)
        / NULLIF(COUNT(*) FILTER (WHERE Churn IN (0, 1)), 0),
        2
    ) AS churn_rate_pct

FROM ecommerce_churn
GROUP BY city_tier
ORDER BY city_tier;
```

| city_tier | total_customers | churned_customers | churn_rate_pct |
| --------- | --------------- | ----------------- | -------------- |
| 1         | 3666            | 532               | 14.51          |
| 2         | 242             | 48                | 19.83          |
| 3         | 1722            | 368               | 21.37          |

**Key Takeaway:** 
- **Geographic Disparities**: City tiers have a clear impact on customer retention. Tier 3 cities experience the highest churn rate at 21.37%, followed closely by Tier 2 at 19.83%.
- **Metropolitan Stability**: Tier 1 cities exhibit significantly stronger loyalty with a lower churn rate of 14.51%, likely reflecting better local infrastructure, service availability, or brand familiarity in major metropolitan hubs.
  
### 2.2 Churn Rate by Gender

> **Business Question:** Do churn rates differ by gender?

```sql
SELECT
    gender,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE Churn = 1) AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE Churn = 1)
        / NULLIF(COUNT(*) FILTER (WHERE Churn IN (0, 1)), 0),
        2
    ) AS churn_rate_pct

FROM ecommerce_churn
GROUP BY gender
ORDER BY churn_rate_pct DESC;
```

| gender | total_customers | churned_customers | churn_rate_pct |
| ------ | --------------- | ----------------- | -------------- |
| Male   | 3384            | 600               | 17.73          |
| Female | 2246            | 348               | 15.49          |

**Key Takeaway:** 
- **Minimal Gender Variance**: Both male and female segments display relatively comparable churn behaviors. Male customers exhibit a slightly higher churn rate (17.73%) compared to female customers (15.49%).
- **Strategic Implication**: While male users churn at a marginally higher rate, the narrow gap suggests that gender alone is not a primary driver of customer defection.

### 2.3 Churn Rate by Marital Status

> **Business Question:** Which marital-status segment has the highest observed churn rate?

```sql
SELECT
    marital_status,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE Churn = 1) AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE Churn = 1)
        / NULLIF(COUNT(*) FILTER (WHERE Churn IN (0, 1)), 0),
        2
    ) AS churn_rate_pct

FROM ecommerce_churn
GROUP BY marital_status
ORDER BY churn_rate_pct DESC;
```

| marital_status | total_customers | churned_customers | churn_rate_pct |
| -------------- | --------------- | ----------------- | -------------- |
| Single         | 1796            | 480               | 26.73          |
| Divorced       | 848             | 124               | 14.62          |
| Married        | 2986            | 344               | 11.52          |

**Key Takeaway:** 
- **Life Stage Vulnerability**: Marital status is a strong indicator of platform loyalty. Single customers experience a disproportionately high churn rate of 26.73%—more than double that of married users.
- **Core Retention Group**: Married customers represent the largest and most stable segment, boasting the lowest churn rate at 11.52%, indicating that household stability or long-term buying habits heavily anchor retention.

## 3. Customer Engagement & Experience
This section evaluates how customer tenure, device usage, customer service touchpoints, app engagement, and order recency impact retention and churn behavior.

### 3.1 Churn Rate by Tenure Group

> **Business Question:** Are newer customers more likely to churn than customers with longer tenure?

```sql
WITH tenure_segments AS (
    SELECT
        CASE
            WHEN tenure IS NULL THEN 'Unknown'
            WHEN tenure <= 6 THEN '0–6 months'
            WHEN tenure <= 12 THEN '7–12 months'
            WHEN tenure <= 24 THEN '13–24 months'
            ELSE '25+ months'
        END AS tenure_group,

        CASE
            WHEN tenure IS NULL THEN 5
            WHEN tenure <= 6 THEN 1
            WHEN tenure <= 12 THEN 2
            WHEN tenure <= 24 THEN 3
            ELSE 4
        END AS sort_order,

        churn_label
    FROM ecommerce_churn
)
SELECT
    tenure_group,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned')
        / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0),
        2
    ) AS churn_rate_pct

FROM tenure_segments
GROUP BY tenure_group, sort_order
ORDER BY sort_order;
```

| tenure_group | total_customers | churned_customers | churn_rate_pct |
| ------------ | --------------- | ----------------- | -------------- |
| 0–6 months   | 2150            | 697               | 32.42          |
| 7–12 months  | 1584            | 156               | 9.85           |
| 13–24 months | 1467            | 95                | 6.48           |
| 25+ months   | 429             | 0                 | 0.00           |

> **Note:** _The tenure bands are analytical choices._

**Key Takeaway:** 
- **Critical Onboarding Window**: Tenure is one of the strongest predictors of churn in the dataset. Customers in their first 6 months experience an alarming 32.42% churn rate, accounting for the vast majority of total customer attrition.
- **Long-Term Loyalty**: Churn drops drastically after the first year (falling to 6.48% for 13–24 months) and reaches 0.00% for customers with over 25 months of tenure, proving that retention efforts must focus heavily on the initial onboarding phase.


### 3.2 Churn Rate by Number of Registered Devices

> **Business Question:** Is the number of registered devices associated with differences in churn?

```sql
SELECT
    number_of_device_registered,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned') / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0), 2
    ) AS churn_rate_pct

FROM ecommerce_churn
GROUP BY number_of_device_registered
ORDER BY number_of_device_registered;
```

| number_of_device_registered | total_customers | churned_customers | churn_rate_pct |
| --------------------------- | --------------- | ----------------- | -------------- |
| 1                           | 235             | 22                | 9.36           |
| 2                           | 276             | 26                | 9.42           |
| 3                           | 1699            | 254               | 14.95          |
| 4                           | 2377            | 392               | 16.49          |
| 5                           | 881             | 198               | 22.47          |
| 6                           | 162             | 56                | 34.57          |

**Key Takeaway:** 
- **Multi-Device Multiplier Effect**: There is a clear positive correlation between the number of registered devices and churn risk. Customers registering 1 to 2 devices maintain low churn rates under 10%.
- **Friction in Account Sharing or Syncing**: Churn scales upward as device counts increase, peaking at 34.57% for users with 6 registered devices. This could indicate account-sharing friction, household multi-user friction, or technical bugs related to cross-device session synchronization.

### 3.3 Churn Rate by Complaint Status

> **Business Question:** Do customers who file complaints have a higher churn rate?

```sql
SELECT
    complain_label,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned') / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0), 2
    ) AS churn_rate_pct

FROM ecommerce_churn
GROUP BY complain_label
ORDER BY complain_label;
```

| complain_label | total_customers | churned_customers | churn_rate_pct |
| -------------- | --------------- | ----------------- | -------------- |
| Complaint      | 1604            | 508               | 31.67          |
| No Complaint   | 4026            | 440               | 10.93          |

**Key Takeaway:** 
- **Service Failures Drive Defection**: Filing a complaint nearly triples a customer's likelihood of leaving. Users who logged a complaint exhibit a 31.67% churn rate, compared to just 10.93% for those who did not.
- **Operational Priority**: This highlights a critical service recovery gap. Unresolved or poorly handled customer complaints are major leakage points that require immediate operational intervention and proactive support workflows.
  
### 3.4 Churn Rate by Satisfaction Score

> **Business Question:** How does observed churn vary across satisfaction scores?

```sql
SELECT
    satisfaction_score,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned')
        / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0),
        2
    ) AS churn_rate_pct

FROM ecommerce_churn
GROUP BY satisfaction_score
ORDER BY satisfaction_score;
```

| satisfaction_score | total_customers | churned_customers | churn_rate_pct |
| ------------------ | --------------- | ----------------- | -------------- |
| 1                  | 1164            | 134               | 11.51          |
| 2                  | 586             | 74                | 12.63          |
| 3                  | 1698            | 292               | 17.20          |
| 4                  | 1074            | 184               | 17.13          |
| 5                  | 1108            | 264               | 23.83          |

**Key Takeaway:** 
- **Counter-Intuitive Distribution**: Interestingly, the highest churn rate (23.83%) occurs among customers reporting a maximum satisfaction score of 5, whereas the lowest churn rate (11.51%) is found at a score of 1.
- **Survey Bias Interpretation**: This unexpected pattern suggests that satisfaction scores captured in this dataset may reflect post-churn frustration, exit-survey polarization, or a disconnect between stated satisfaction and actual platform utility.

### 3.5 Average App Usage by Churn Status

> **Business Question:** Do churned and retained customers spend different amounts of time on the app?

```sql
SELECT
    churn_label AS customer_status,
    COUNT(*) AS customer_count,
    ROUND(AVG(hour_spend_on_app)::NUMERIC, 2) AS avg_hours_spent_on_app

FROM ecommerce_churn
GROUP BY churn_label
ORDER BY churn_label;
```

| customer_status | customer_count | avg_hours_spent_on_app |
| --------------- | -------------- | ---------------------- |
| Churned         | 948            | 2.96                   |
| Retained        | 4682           | 2.93                   |

**Key Takeaway:** 
- **Platform Engagement Parity**: App engagement time is virtually identical between groups, with churned users averaging 2.96 hours and retained users averaging 2.93 hours.
- **Strategic Implication**: Pure screen time or app presence does not signal loyalty risk. High app usage does not inherently safeguard against churn, meaning retention strategies must look deeper into transaction behavior and service quality rather than app activity alone.

### 3.6 Churn Rate by Days Since Last Order

> **Business Question:** Is a longer interval since the last order associated with a higher churn rate?

```sql
WITH recency_segments AS (
    SELECT
        CASE
            WHEN day_since_last_order IS NULL THEN 'Unknown'
            WHEN day_since_last_order <= 7 THEN '0–7 days'
            WHEN day_since_last_order <= 30 THEN '8–30 days'
            WHEN day_since_last_order <= 60 THEN '31–60 days'
            ELSE '61+ days'
        END AS recency_group,

        CASE
            WHEN day_since_last_order IS NULL THEN 5
            WHEN day_since_last_order <= 7 THEN 1
            WHEN day_since_last_order <= 30 THEN 2
            WHEN day_since_last_order <= 60 THEN 3
            ELSE 4
        END AS sort_order,

        churn_label
    FROM ecommerce_churn
)
SELECT
    recency_group,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned') / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0), 2
    ) AS churn_rate_pct

FROM recency_segments
GROUP BY recency_group, sort_order
ORDER BY sort_order;
```

| recency_group | total_customers | churned_customers | churn_rate_pct |
| ------------- | --------------- | ----------------- | -------------- |
| 0–7 days      | 4328            | 825               | 19.06          |
| 8–30 days     | 1300            | 122               | 9.38           |
| 31–60 days    | 2               | 1                 | 50.00          |

**Key Takeaway:** 
- **Immediate Re-engagement Window**: The vast majority of customer activity concentrates within the 0–7 day window. Customers ordering within the last week display a 19.06% churn rate, reflecting high-frequency users who may also churn rapidly if expectations aren't met.
- **Dormancy Data Scarcity**: Intervals exceeding 31 days contain very few records in this dataset, indicating that inactive users either quickly convert to full churn or are systematically purged from active tracking tables.
