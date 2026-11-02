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

## 4. Purchasing Behavior & Incentives
This section examines how customer shopping preferences, login devices, payment methods, order categories, logistics distances, and incentive programs (cashbacks and coupons) relate to customer attrition.

### 4.1 Churn Rate by Preferred Login Device

> **Business Question:** Which preferred login devices are associated with higher churn rates?

```sql
SELECT
    preferred_login_device,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned')
        / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0),
        2
    ) AS churn_rate_pct

FROM ecommerce_churn
GROUP BY preferred_login_device
ORDER BY churn_rate_pct DESC;
```

| preferred_login_device | total_customers | churned_customers | churn_rate_pct |
| ---------------------- | --------------- | ----------------- | -------------- |
| Computer               | 1634            | 324               | 19.83          |
| Phone                  | 3996            | 624               | 15.62          |

**Key Takeaway:** 
- **Desktop vs. Mobile Divergence**: Customers whose preferred login device is a Computer exhibit a noticeably higher churn rate (19.83%) compared to mobile Phone users (15.62%).
- **Strategic Implication**: While mobile app users account for the vast majority of total volume (3,996 customers), computer-based users may experience different platform constraints, desktop UI friction, or a higher intent to cross-shop competitors.

### 4.2 Churn Rate by Preferred Payment Mode

> **Business Question:** Which preferred payment modes have the highest observed churn rates?

```sql
SELECT
    preferred_payment_mode,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned')
        / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0),
        2
    ) AS churn_rate_pct

FROM ecommerce_churn
GROUP BY preferred_payment_mode
ORDER BY churn_rate_pct DESC;
```

| preferred_payment_mode | total_customers | churned_customers | churn_rate_pct |
| ---------------------- | --------------- | ----------------- | -------------- |
| Cash on Delivery       | 514             | 128               | 24.90          |
| E-Wallet               | 614             | 140               | 22.80          |
| CC                     | 273             | 59                | 21.61          |
| UPI                    | 414             | 72                | 17.39          |
| Debit Card             | 2314            | 356               | 15.38          |
| Credit Card            | 1501            | 193               | 12.86          |

**Key Takeaway:** 
- **Friction in Alternative Payments**: High-friction or cash-reliant methods show the highest attrition. Cash on Delivery (COD) leads with a 24.90% churn rate, closely followed by E-Wallets (22.80%) and shorthand CC entries (21.61%).
- **Stable Digital Channels**: Traditional structured cards provide the strongest stickiness; Credit Card users experience the lowest churn rate at 12.86%, suggesting that established, recurring payment configurations anchor long-term customer loyalty.

### 4.3 Churn Rate by Preferred Order Category

> **Business Question:** Which preferred order categories have higher churn rates?

```sql
SELECT
    prefered_order_cat AS preferred_order_category,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned')
        / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0),
        2
    ) AS churn_rate_pct

FROM ecommerce_churn
GROUP BY prefered_order_cat
ORDER BY churn_rate_pct DESC;
```

| preferred_order_category | total_customers | churned_customers | churn_rate_pct |
| ------------------------ | --------------- | ----------------- | -------------- |
| Mobile                   | 2080            | 570               | 27.40          |
| Fashion                  | 826             | 128               | 15.50          |
| Laptop & Accessory       | 2050            | 210               | 10.24          |
| Others                   | 264             | 20                | 7.58           |
| Grocery                  | 410             | 20                | 4.88           |

**Key Takeaway:** 
- **High Attrition in Mobile Electronics**: The Mobile order category suffers from a disproportionately high churn rate of 27.40%, representing a massive product-specific leakage vector.
- **Consumable Loyalty**: Conversely, daily necessity categories like Grocery display exceptional retention, with a minimal churn rate of just 4.88%. High-frequency purchasing categories successfully anchor users, whereas standalone mobile device purchases skew toward one-and-done transactions.

### 4.4 Churn Rate by Warehouse-to-Home Distance

> **Business Question:** Does churn vary with the distance between customers and warehouses?

```sql
WITH distance_segments AS (
    SELECT
        CASE
            WHEN warehouse_to_home IS NULL THEN 'Unknown'
            WHEN warehouse_to_home <= 5 THEN '0–5 units'
            WHEN warehouse_to_home <= 10 THEN '6–10 units'
            WHEN warehouse_to_home <= 20 THEN '11–20 units'
            WHEN warehouse_to_home <= 30 THEN '21–30 units'
            ELSE '31+ units'
        END AS distance_group,

        CASE
            WHEN warehouse_to_home IS NULL THEN 6
            WHEN warehouse_to_home <= 5 THEN 1
            WHEN warehouse_to_home <= 10 THEN 2
            WHEN warehouse_to_home <= 20 THEN 3
            WHEN warehouse_to_home <= 30 THEN 4
            ELSE 5
        END AS sort_order,

        churn_label
    FROM ecommerce_churn
)
SELECT
    distance_group,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned')
        / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0),
        2
    ) AS churn_rate_pct

FROM distance_segments
GROUP BY distance_group, sort_order
ORDER BY sort_order;
```

| distance_group | total_customers | churned_customers | churn_rate_pct |
| -------------- | --------------- | ----------------- | -------------- |
| 0–5 units      | 8               | 1                 | 12.50          |
| 6–10 units     | 1961            | 265               | 13.51          |
| 11–20 units    | 2318            | 408               | 17.60          |
| 21–30 units    | 872             | 176               | 20.18          |
| 31+ units      | 471             | 98                | 20.81          |

**Key Takeaway:** 
- **Logistical Friction**: Distance directly impacts retention. As the warehouse-to-home distance increases, churn rates climb steadily from 13.51% (6–10 units) up to 20.81% (31+ units).
- **Fulfillment Impact**: Greater geographical distance likely introduces delivery delays, higher shipping friction, or logistics costs that gradually erode customer satisfaction over time.

### 4.5 Average Coupon Usage by Churn Status

> **Business Question:** Do churned and retained customers differ in average coupon usage?

```sql
SELECT
    churn_label AS customer_status,
    COUNT(*) AS customer_count,
    ROUND(AVG(coupon_used)::NUMERIC, 2) AS avg_coupons_used

FROM ecommerce_churn
GROUP BY churn_label
ORDER BY churn_label;
```

| customer_status | customer_count | avg_coupons_used |
| --------------- | -------------- | ---------------- |
| Churned         | 948            | 1.71             |
| Retained        | 4682           | 1.72             |

**Key Takeaway:** 
- **Incentive Utilization Parity**: Promotional coupon usage is identical across groups, with churned users averaging 1.71 coupons used and retained users averaging 1.72.
- **Strategic Insight**: Basic coupon availability or redemption volume does not act as a differentiator for retention, implying that blanket discounts alone cannot prevent customer attrition.

### 4.6 Churn Rate by Cashback Range

> **Business Question:** Does churn vary across cashback amount ranges?

```sql
WITH cashback_segments AS (
    SELECT
        CASE
            WHEN cashback_amount IS NULL THEN 'Unknown'
            WHEN cashback_amount < 100 THEN 'Below 100'
            WHEN cashback_amount < 200 THEN '100–199'
            WHEN cashback_amount < 300 THEN '200–299'
            ELSE '300+'
        END AS cashback_group,

        CASE
            WHEN cashback_amount IS NULL THEN 5
            WHEN cashback_amount < 100 THEN 1
            WHEN cashback_amount < 200 THEN 2
            WHEN cashback_amount < 300 THEN 3
            ELSE 4
        END AS sort_order,

        churn_label
    FROM ecommerce_churn
)
SELECT
    cashback_group,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned')
        / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0),
        2
    ) AS churn_rate_pct

FROM cashback_segments
GROUP BY cashback_group, sort_order
ORDER BY sort_order;
```

| cashback_group | total_customers | churned_customers | churn_rate_pct |
| -------------- | --------------- | ----------------- | -------------- |
| Below 100      | 12              | 0                 | 0.00           |
| 100–199        | 4275            | 808               | 18.90          |
| 200–299        | 1181            | 130               | 11.01          |
| 300+           | 162             | 10                | 6.17           |

**Key Takeaway:** 
- **Strong Retention Impact of Rewards**: Unlike coupon frequency, higher monetary cashback value heavily discourages churn. Churn drops sharply from 18.90% for low cashbacks (100–199) down to just 6.17% for high-tier cashback earners (300+).
- **Value Retention**: Substantial financial reward structures successfully cultivate long-term customer stickiness.

### 4.7 Order Amount Increase by Churn Status

> **Business Question:** Does the reported increase in order amount from last year differ between churned and retained customers?

```sql
SELECT
    churn_label AS customer_status,
    COUNT(*) AS customer_count,

    ROUND(
        AVG(order_amount_hike_from_last_year)::NUMERIC,
        2
    ) AS avg_order_amount_hike

FROM ecommerce_churn
GROUP BY churn_label
ORDER BY churn_label;
```

| customer_status | customer_count | avg_order_amount_hike |
| --------------- | -------------- | --------------------- |
| Churned         | 948            | 15.62                 |
| Retained        | 4682           | 15.69                 |

**Key Takeaway:** 
- **Spending Growth Parity**: Year-over-year order amount hikes are virtually identical between churned (15.62%) and retained (15.69%) groups.
- **Value Independence**: Expansion in transaction value does not protect against churn; customers leaving the platform grow their spending at the exact same historical rate as those who stay, indicating that churn is driven by service, logistics, or lifecycle friction rather than financial stagnation.

## 5. Advanced Segmentation & Risk Analysis
This section moves beyond single-variable analysis by examining the intersection of multiple risk factors—specifically tenure length, complaint status, and city tier—to pinpoint exact high-risk customer segments.

### 5.1 Identify High-Risk Multi-Dimensional Customer Segments

> **Business Question:** Which multi-dimensional intersections of customer tenure, complaint status, and city tier identify the highest-risk customer segments?

```sql
SELECT
    CASE
        WHEN tenure <= 6 THEN 'New Customer'
        WHEN tenure <= 12 THEN 'Developing Customer'
        WHEN tenure > 12 THEN 'Established Customer'
        ELSE 'Unknown'
    END AS tenure_segment,
    complain_label,
    city_tier,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,

    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned')
        / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0),
        2
    ) AS churn_rate_pct

FROM ecommerce_churn
GROUP BY tenure_segment, complain_label, city_tier
HAVING COUNT(*) >= 20
ORDER BY churn_rate_pct DESC, total_customers DESC;
```

| tenure_segment       | complain_label | city_tier | total_customers | churned_customers | churn_rate_pct |
| -------------------- | -------------- | --------- | --------------- | ----------------- | -------------- |
| New Customer         | Complaint      | 3         | 228             | 138               | 60.53          |
| New Customer         | Complaint      | 1         | 413             | 221               | 53.51          |
| New Customer         | No Complaint   | 2         | 52              | 20                | 38.46          |
| New Customer         | No Complaint   | 3         | 515             | 128               | 24.85          |
| Developing Customer  | Complaint      | 3         | 126             | 26                | 20.63          |
| Established Customer | Complaint      | 3         | 144             | 28                | 19.44          |
| Developing Customer  | Complaint      | 1         | 264             | 51                | 19.32          |
| New Customer         | No Complaint   | 1         | 924             | 174               | 18.83          |
| Developing Customer  | No Complaint   | 2         | 66              | 8                 | 12.12          |
| Developing Customer  | No Complaint   | 3         | 336             | 34                | 10.12          |
| Established Customer | Complaint      | 1         | 367             | 24                | 6.54           |
| Developing Customer  | No Complaint   | 1         | 776             | 33                | 4.25           |
| Established Customer | No Complaint   | 3         | 373             | 14                | 3.75           |
| Established Customer | No Complaint   | 1         | 922             | 29                | 3.15           |
| Established Customer | No Complaint   | 2         | 62              | 0                 | 0.00           |
| Established Customer | Complaint      | 2         | 28              | 0                 | 0.00           |

**Key Takeaway:** 
- **Compound Vulnerability in Onboarding**: When early tenure aligns with service friction, attrition reaches critical proportions. New customers (0–6 months) who file a complaint in Tier 3 or Tier 1 cities suffer extreme churn rates of 60.53% and 53.51%, respectively.
- **Protective Effect of Tenure**: Conversely, Established customers (>12 months tenure) with no complaints exhibit near-zero churn (dropping as low as 3.15% in Tier 1 and 0.00% in Tier 2), proving that long-term loyalty effectively insulates users against regional or logistical friction.

### 5.2 Multi-Dimensional Summary Overview

> **Purpose:** Generates a consolidated comparative view across major analytical dimensions for quick portfolio reference and BI integration.

```sql
SELECT
    'City Tier' AS analysis_dimension,
    city_tier::TEXT AS segment,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE churn_label = 'Churned') AS churned_customers,
    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned')
        / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0),
        2
    ) AS churn_rate_pct
FROM ecommerce_churn
GROUP BY city_tier

UNION ALL

SELECT
    'Marital Status',
    marital_status,
    COUNT(*),
    COUNT(*) FILTER (WHERE churn_label = 'Churned'),
    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned')
        / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0),
        2
    )
FROM ecommerce_churn
GROUP BY marital_status

UNION ALL

SELECT
    'Payment Mode',
    preferred_payment_mode,
    COUNT(*),
    COUNT(*) FILTER (WHERE churn_label = 'Churned'),
    ROUND(
        100.0 * COUNT(*) FILTER (WHERE churn_label = 'Churned')
        / NULLIF(COUNT(*) FILTER (WHERE churn_label IN ('Churned', 'Retained')), 0),
        2
    )
FROM ecommerce_churn
GROUP BY preferred_payment_mode

ORDER BY analysis_dimension, churn_rate_pct DESC;
```

| analysis_dimension | segment          | total_customers | churned_customers | churn_rate_pct |
| ------------------ | ---------------- | --------------- | ----------------- | -------------- |
| City Tier          | 3                | 1722            | 368               | 21.37          |
| City Tier          | 2                | 242             | 48                | 19.83          |
| City Tier          | 1                | 3666            | 532               | 14.51          |
| Marital Status     | Single           | 1796            | 480               | 26.73          |
| Marital Status     | Divorced         | 848             | 124               | 14.62          |
| Marital Status     | Married          | 2986            | 344               | 11.52          |
| Payment Mode       | Cash on Delivery | 514             | 128               | 24.90          |
| Payment Mode       | E-Wallet         | 614             | 140               | 22.80          |
| Payment Mode       | CC               | 273             | 59                | 21.61          |
| Payment Mode       | UPI              | 414             | 72                | 17.39          |
| Payment Mode       | Debit Card       | 2314            | 356               | 15.38          |
| Payment Mode       | Credit Card      | 1501            | 193               | 12.86          |

**Key Takeaway:**
- This cross-dimensional summary unifies the primary structural risk factors across the platform: City Tier 3 locations (21.37% churn), Single life-stage status (26.73% churn), and alternative transaction methods like Cash on Delivery (24.90% churn) emerge as the highest-risk macro segments requiring targeted retention frameworks.

