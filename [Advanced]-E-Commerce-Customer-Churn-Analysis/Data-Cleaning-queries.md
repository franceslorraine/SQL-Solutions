# DATA CLEANING QUERIES & OUTPUTS

## 1. Dataset Inspection
This shows the total number of records and columns

### 1.1 Total Customer Records

```sql
SELECT DISTINCT COUNT(customer_id) as total_number_of_customers
FROM ecommerce_churn;
```

| total_number_of_customers |
| ------------------------- |
| 5630                      |

There are 5,630 customer records. This count serves as the baseline for validating record retention throughout the cleaning process.

### 1.2 Preview the Dataset

The first ten records are retrieved to inspect the available columns, data types as represented in the results, and sample values.

```sql
SELECT *
FROM ecommerce_churn
LIMIT 10;
```

| customer_id | churn | tenure | preferred_login_device | city_tier | warehouse_to_home | preferred_payment_mode | gender | hour_spend_on_app | number_of_device_registered | prefered_order_cat | satisfaction_score | marital_status | number_of_address | complain | order_amount_hike_from_last_year | coupon_used | order_count | day_since_last_order | cashback_amount |
| ----------- | ----- | ------ | ---------------------- | --------- | ----------------- | ---------------------- | ------ | ----------------- | --------------------------- | ------------------ | ------------------ | -------------- | ----------------- | -------- | -------------------------------- | ----------- | ----------- | -------------------- | --------------- |
| 50001       | 1     | 4      | Mobile Phone           | 3         | 6                 | Debit Card             | Female | 3                 | 3                           | Laptop & Accessory | 2                  | Single         | 9                 | 1        | 11                               | 1           | 1           | 5                    | 160             |
| 50002       | 1     | null   | Phone                  | 1         | 8                 | UPI                    | Male   | 3                 | 4                           | Mobile             | 3                  | Single         | 7                 | 1        | 15                               | 0           | 1           | 0                    | 121             |
| 50003       | 1     | null   | Phone                  | 1         | 30                | Debit Card             | Male   | 2                 | 4                           | Mobile             | 3                  | Single         | 6                 | 1        | 14                               | 0           | 1           | 3                    | 120             |
| 50004       | 1     | 0      | Phone                  | 3         | 15                | Debit Card             | Male   | 2                 | 4                           | Laptop & Accessory | 5                  | Single         | 8                 | 0        | 23                               | 0           | 1           | 3                    | 134             |
| 50005       | 1     | 0      | Phone                  | 1         | 12                | CC                     | Male   | null              | 3                           | Mobile             | 5                  | Single         | 3                 | 0        | 11                               | 1           | 1           | 3                    | 130             |
| 50006       | 1     | 0      | Computer               | 1         | 22                | Debit Card             | Female | 3                 | 5                           | Mobile Phone       | 5                  | Single         | 2                 | 1        | 22                               | 4           | 6           | 7                    | 139             |
| 50007       | 1     | null   | Phone                  | 3         | 11                | Cash on Delivery       | Male   | 2                 | 3                           | Laptop & Accessory | 2                  | Divorced       | 4                 | 0        | 14                               | 0           | 1           | 0                    | 121             |
| 50008       | 1     | null   | Phone                  | 1         | 6                 | CC                     | Male   | 3                 | 3                           | Mobile             | 2                  | Divorced       | 3                 | 1        | 16                               | 2           | 2           | 0                    | 123             |
| 50009       | 1     | 13     | Phone                  | 3         | 9                 | E wallet               | Male   | null              | 4                           | Mobile             | 3                  | Divorced       | 2                 | 1        | 14                               | 0           | 1           | 2                    | 127             |
| 50010       | 1     | null   | Phone                  | 1         | 31                | Debit Card             | Male   | 2                 | 5                           | Mobile             | 3                  | Single         | 2                 | 0        | 12                               | 1           | 1           | 1                    | 123             |

The preview provides an initial understanding of the dataset, including customer demographics, purchasing behavior, satisfaction scores, complaints, and churn status. It also reveals potential missing values that require further investigation.

### 1.3 Check for Duplicate Customer IDs

> **Assumption:** `customer_id` is expected to uniquely identify each customer. Duplicate identifiers could lead to inaccurate customer counts and misleading analysis.

```sql
SELECT
    customer_id,
    COUNT(customer_id) AS record_count
FROM ecommerce_churn
GROUP BY customer_id
HAVING COUNT(customer_id) > 1;
```

```sql
Success. No rows returned
```

No duplicate customer IDs were detected. This supports the uniqueness of the customer identifier in the current dataset.

### 1.4 Check for Missing / Null Values

Missing values are identified across all columns to determine which variables require further investigation or treatment.

```sql
SELECT
    'Customer ID' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE customer_id IS NULL

  UNION

SELECT
  'Churn' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE churn IS NULL

  UNION

SELECT
  'Tenure' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE tenure IS NULL

  UNION

SELECT
  'Preferred Login Device' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE preferred_login_device IS NULL

  UNION

SELECT
  'City Tier' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE city_tier IS NULL

  UNION

SELECT
  'Warehouse to Home' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE warehouse_to_home IS NULL

  UNION

SELECT
  'Preferred Payment Mode' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE preferred_payment_mode IS NULL

  UNION

SELECT
  'Gender' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE gender IS NULL

  UNION

SELECT
  'Hours Spend on App' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE hour_spend_on_app IS NULL

  UNION

SELECT
  'Number of Device Registered' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE number_of_device_registered IS NULL

  UNION

SELECT
  'Preferred Order Category' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE prefered_order_cat IS NULL

  UNION

SELECT
  'Satisfaction Score' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE satisfaction_score IS NULL

  UNION

SELECT
  'Marital Status' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE marital_status IS NULL

  UNION

SELECT
  'Number of Address' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE number_of_address IS NULL

  UNION

SELECT
  'Complain' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE complain IS NULL

  UNION

SELECT
  'Order Amount Hike from Last Year' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE order_amount_hike_from_last_year IS NULL

  UNION

SELECT
  'Coupon Used' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE coupon_used IS NULL

  UNION

SELECT
  'Order Count' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE order_count IS NULL

  UNION

SELECT
  'Day Since Last Order' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE day_since_last_order IS NULL

  UNION

SELECT
  'Cashback Amount' AS "Column Name", COUNT(*) AS "Null Count"
  FROM ecommerce_churn
  WHERE cashback_amount IS NULL;
```

| Column Name                      | Null Count |
| -------------------------------- | ---------- |
| Cashback Amount                  | 0          |
| Churn                            | 0          |
| City Tier                        | 0          |
| Complain                         | 0          |
| Coupon Used                      | 256        |
| Customer ID                      | 0          |
| Day Since Last Order             | 307        |
| Gender                           | 0          |
| Hours Spend on App               | 255        |
| Marital Status                   | 0          |
| Number of Address                | 0          |
| Number of Device Registered      | 0          |
| Order Amount Hike from Last Year | 265        |
| Order Count                      | 258        |
| Preferred Login Device           | 0          |
| Preferred Order Category         | 0          |
| Preferred Payment Mode           | 0          |
| Satisfaction Score               | 0          |
| Tenure                           | 264        |
| Warehouse to Home                | 251        |

The table above suggests that `coupon_used`, `day_since_last_order`, `hours_spend_on_app`, `order_amount_hike_from_last_year`, `order_count`, `tenure`, and `warehouse_to_home` contain missing values. These values must be handled accordingly before any calculations.

## 2. Handling Missing Values
Missing values can affect descriptive statistics, visualizations, and downstream analyses. This section examines their prevalence and the distributions of affected numerical columns before selecting an appropriate treatment.

### 2.1 Calculate Missing-Value Percentages
The percentage of missing values is calculated for each affected column to quantify the extent of missing data.

```sql
SELECT
    column_name,
    null_count,
    ROUND(100.0 * null_count / total_rows, 2) AS null_percentage
FROM (
    SELECT
        COUNT(*) AS total_rows,
        COUNT(*) FILTER (WHERE day_since_last_order IS NULL) AS day_since_last_order_nulls,
        COUNT(*) FILTER (WHERE order_amount_hike_from_last_year IS NULL) AS order_amount_hike_nulls,
        COUNT(*) FILTER (WHERE tenure IS NULL) AS tenure_nulls,
        COUNT(*) FILTER (WHERE order_count IS NULL) AS order_count_nulls,
        COUNT(*) FILTER (WHERE coupon_used IS NULL) AS coupon_used_nulls,
        COUNT(*) FILTER (WHERE hour_spend_on_app IS NULL) AS hour_spend_on_app_nulls,
        COUNT(*) FILTER (WHERE warehouse_to_home IS NULL) AS warehouse_to_home_nulls
    FROM ecommerce_churn
) AS missing_summary
CROSS JOIN LATERAL (
    VALUES
        ('day_since_last_order', day_since_last_order_nulls),
        ('order_amount_hike_from_last_year', order_amount_hike_nulls),
        ('tenure', tenure_nulls),
        ('order_count', order_count_nulls),
        ('coupon_used', coupon_used_nulls),
        ('hour_spend_on_app', hour_spend_on_app_nulls),
        ('warehouse_to_home', warehouse_to_home_nulls)
) AS missing_values(column_name, null_count)
WHERE null_count > 0
ORDER BY null_percentage DESC;
```

| column_name                      | null_count | null_percentage |
| -------------------------------- | ---------- | --------------- |
| day_since_last_order             | 307        | 5.45            |
| order_amount_hike_from_last_year | 265        | 4.71            |
| tenure                           | 264        | 4.69            |
| order_count                      | 258        | 4.58            |
| coupon_used                      | 256        | 4.55            |
| hour_spend_on_app                | 255        | 4.53            |
| warehouse_to_home                | 251        | 4.46            |

Missing values range from approximately 4.46% to 5.45% of the dataset. **_Although these percentages are relatively modest, the reasons for missingness should be considered before selecting an imputation method._**

### 2.2 Examine Descriptive Statistics
Mean, median, standard deviation, minimum, and maximum values are examined to understand the distributions of the affected variables.

```sql
SELECT
    'coupon_used' AS column_name,
    COUNT(coupon_used) AS non_null_count,
    COUNT(*) - COUNT(coupon_used) AS null_count,
    ROUND(AVG(coupon_used)::numeric, 2) AS mean,
    ROUND((PERCENTILE_CONT(0.5)
        WITHIN GROUP (ORDER BY coupon_used))::numeric, 2) AS median,
    ROUND(STDDEV(coupon_used)::numeric, 2) AS standard_deviation,
    MIN(coupon_used) AS minimum,
    MAX(coupon_used) AS maximum
FROM ecommerce_churn

UNION ALL

SELECT
    'day_since_last_order',
    COUNT(day_since_last_order),
    COUNT(*) - COUNT(day_since_last_order),
    ROUND(AVG(day_since_last_order)::numeric, 2),
    ROUND((PERCENTILE_CONT(0.5)
        WITHIN GROUP (ORDER BY day_since_last_order))::numeric, 2),
    ROUND(STDDEV(day_since_last_order)::numeric, 2),
    MIN(day_since_last_order),
    MAX(day_since_last_order)
FROM ecommerce_churn

UNION ALL

SELECT
    'order_amount_hike_from_last_year',
    COUNT(order_amount_hike_from_last_year),
    COUNT(*) - COUNT(order_amount_hike_from_last_year),
    ROUND(AVG(order_amount_hike_from_last_year)::numeric, 2),
    ROUND((PERCENTILE_CONT(0.5)
        WITHIN GROUP (ORDER BY order_amount_hike_from_last_year))::numeric, 2),
    ROUND(STDDEV(order_amount_hike_from_last_year)::numeric, 2),
    MIN(order_amount_hike_from_last_year),
    MAX(order_amount_hike_from_last_year)
FROM ecommerce_churn

UNION ALL

SELECT
    'order_count',
    COUNT(order_count),
    COUNT(*) - COUNT(order_count),
    ROUND(AVG(order_count)::numeric, 2),
    ROUND((PERCENTILE_CONT(0.5)
        WITHIN GROUP (ORDER BY order_count))::numeric, 2),
    ROUND(STDDEV(order_count)::numeric, 2),
    MIN(order_count),
    MAX(order_count)
FROM ecommerce_churn

UNION ALL

SELECT
    'tenure',
    COUNT(tenure),
    COUNT(*) - COUNT(tenure),
    ROUND(AVG(tenure)::numeric, 2),
    ROUND((PERCENTILE_CONT(0.5)
        WITHIN GROUP (ORDER BY tenure))::numeric, 2),
    ROUND(STDDEV(tenure)::numeric, 2),
    MIN(tenure),
    MAX(tenure)
FROM ecommerce_churn

UNION ALL

SELECT
    'warehouse_to_home',
    COUNT(warehouse_to_home),
    COUNT(*) - COUNT(warehouse_to_home),
    ROUND(AVG(warehouse_to_home)::numeric, 2),
    ROUND((PERCENTILE_CONT(0.5)
        WITHIN GROUP (ORDER BY warehouse_to_home))::numeric, 2),
    ROUND(STDDEV(warehouse_to_home)::numeric, 2),
    MIN(warehouse_to_home),
    MAX(warehouse_to_home)
FROM ecommerce_churn

UNION ALL

SELECT
    'hour_spend_on_app',
    COUNT(hour_spend_on_app),
    COUNT(*) - COUNT(hour_spend_on_app),
    ROUND(AVG(hour_spend_on_app)::numeric, 2),
    ROUND((PERCENTILE_CONT(0.5)
        WITHIN GROUP (ORDER BY hour_spend_on_app))::numeric, 2),
    ROUND(STDDEV(hour_spend_on_app)::numeric, 2),
    MIN(hour_spend_on_app),
    MAX(hour_spend_on_app)
FROM ecommerce_churn;
```

| column_name                      | non_null_count | null_count | mean  | median | standard_deviation | minimum | maximum |
| -------------------------------- | -------------- | ---------- | ----- | ------ | ------------------ | ------- | ------- |
| coupon_used                      | 5374           | 256        | 1.75  | 1.00   | 1.89               | 0       | 16      |
| day_since_last_order             | 5323           | 307        | 4.54  | 3.00   | 3.65               | 0       | 46      |
| order_amount_hike_from_last_year | 5365           | 265        | 15.71 | 15.00  | 3.68               | 11      | 26      |
| order_count                      | 5372           | 258        | 3.01  | 2.00   | 2.94               | 1       | 16      |
| tenure                           | 5366           | 264        | 10.19 | 9.00   | 8.56               | 0       | 61      |
| warehouse_to_home                | 5379           | 251        | 15.64 | 14.00  | 8.53               | 5       | 127     |
| hour_spend_on_app                | 5375           | 255        | 2.93  | 3.00   | 0.72               | 0       | 5       |

The mean exceeds the median in most examined columns, suggesting possible right skewness. `warehouse_to_home` has a maximum value of 127, substantially higher than its median of 14. 

### 2.3 Handle Missing Values Using Median Imputation

> **Method Reasoning:** The median was selected to fill missing numerical values because it is less sensitive to extreme values than the mean. The median of each column was calculated using its available non-null values, then used to replace missing entries.

```sql
WITH medians AS (
    SELECT
        PERCENTILE_CONT(0.5) WITHIN GROUP (
            ORDER BY coupon_used
        ) AS median_coupon_used,

        PERCENTILE_CONT(0.5) WITHIN GROUP (
            ORDER BY day_since_last_order
        ) AS median_day_since_last_order,

        PERCENTILE_CONT(0.5) WITHIN GROUP (
            ORDER BY order_amount_hike_from_last_year
        ) AS median_order_amount_hike,

        PERCENTILE_CONT(0.5) WITHIN GROUP (
            ORDER BY order_count
        ) AS median_order_count,

        PERCENTILE_CONT(0.5) WITHIN GROUP (
            ORDER BY tenure
        ) AS median_tenure,

        PERCENTILE_CONT(0.5) WITHIN GROUP (
            ORDER BY warehouse_to_home
        ) AS median_warehouse_to_home,

        PERCENTILE_CONT(0.5) WITHIN GROUP (
            ORDER BY hour_spend_on_app
        ) AS median_hour_spend_on_app
    FROM ecommerce_churn
)
UPDATE ecommerce_churn AS e
SET
    coupon_used = COALESCE(
        e.coupon_used, m.median_coupon_used
    ),
    day_since_last_order = COALESCE(
        e.day_since_last_order, m.median_day_since_last_order
    ),
    order_amount_hike_from_last_year = COALESCE(
        e.order_amount_hike_from_last_year,
        m.median_order_amount_hike
    ),
    order_count = COALESCE(
        e.order_count, m.median_order_count
    ),
    tenure = COALESCE(
        e.tenure, m.median_tenure
    ),
    warehouse_to_home = COALESCE(
        e.warehouse_to_home, m.median_warehouse_to_home
    ),
    hour_spend_on_app = COALESCE(
        e.hour_spend_on_app, m.median_hour_spend_on_app
    )
FROM medians AS m
WHERE
    e.coupon_used IS NULL
    OR e.day_since_last_order IS NULL
    OR e.order_amount_hike_from_last_year IS NULL
    OR e.order_count IS NULL
    OR e.tenure IS NULL
    OR e.warehouse_to_home IS NULL
    OR e.hour_spend_on_app IS NULL;
```
> **Description:** Description: `PERCENTILE_CONT(0.5)` calculates each column's median, while `COALESCE()` replaces only null values and preserves existing data.

### 2.4 Verify Missing Values After Imputation

```sql
SELECT
    COUNT(*) FILTER (
        WHERE coupon_used IS NULL
    ) AS coupon_used_nulls,

    COUNT(*) FILTER (
        WHERE day_since_last_order IS NULL
    ) AS day_since_last_order_nulls,

    COUNT(*) FILTER (
        WHERE order_amount_hike_from_last_year IS NULL
    ) AS order_amount_hike_nulls,

    COUNT(*) FILTER (
        WHERE order_count IS NULL
    ) AS order_count_nulls,

    COUNT(*) FILTER (
        WHERE tenure IS NULL
    ) AS tenure_nulls,

    COUNT(*) FILTER (
        WHERE warehouse_to_home IS NULL
    ) AS warehouse_to_home_nulls,

    COUNT(*) FILTER (
        WHERE hour_spend_on_app IS NULL
    ) AS hour_spend_on_app_nulls
FROM ecommerce_churn;
```
> **Expected Result:** All seven columns should have 0 remaining null values if the update succeeds.

| coupon_used_nulls | day_since_last_order_nulls | order_amount_hike_nulls | order_count_nulls | tenure_nulls | warehouse_to_home_nulls | hour_spend_on_app_nulls |
| ----------------- | -------------------------- | ----------------------- | ----------------- | ------------ | ----------------------- | ----------------------- |
| 0                 | 0                          | 0                       | 0                 | 0            | 0                       | 0                       |
> **Conclusion:** Median imputation was applied to the seven numerical columns with missing values. The dataset can now proceed to the remaining data validation and churn analysis steps.


## 3. Data Transformation
This section introduces more interpretable labels for binary variables while retaining the original columns for quantitative analysis.

### 3.1 Create a Readable Churn Label
The churn column contains binary values. A separate label makes the results easier to interpret in tables and dashboards.

```sql
ALTER TABLE ecommerce_churn
ADD COLUMN IF NOT EXISTS churn_label VARCHAR(20);

UPDATE ecommerce_churn
SET churn_label = CASE
    WHEN churn = 1 THEN 'Churned'
    WHEN churn = 0 THEN 'Retained'
    ELSE 'Unknown'
END;
```

The new `churn_label` column provides readable categories for customer churn status. The original churn column remains available for calculating churn rates and other metrics.

### 3.2 Create a Readable Complaint Label
Similar to `churn` labels, `complaint` column also uses binary values. `0` means that the customer did not complain, otherwise it is marked `1`. For clarity purposes, we will create a new column called `complain_label` that shows “ No Complaint” when a customer did not complain and “Complaint” when a customer complained.

```sql
ALTER TABLE ecommerce_churn
ADD COLUMN IF NOT EXISTS complain_label VARCHAR(20);

UPDATE ecommerce_churn
SET complain_label = CASE
    WHEN complain = 1 THEN 'Complaint'
    WHEN complain = 0 THEN 'No Complaint'
    ELSE 'Unknown'
END;
```

The `complain_label` column provides a more descriptive representation of complaint status while preserving the original binary values for analysis.

## 4. Data Consistency and Accuracy Checks
While going through the data, I have found duplicate inputs. These will be standardized to ensure consistent categorization. Standardizing equivalent labels prevents the same device category from being counted separately during aggregation.

### 4.1 Standardize Preferred Login Device

```sql
SELECT DISTINCT preferred_login_device
FROM ecommerce_churn;
```

| preferred_login_device |
| ---------------------- |
| Mobile Phone           |
| Computer               |
| Phone                  |

> **Assumption:** Assume that `Mobile Phone` and `Phone` represent the same category. We can standardized them into one label. _To do so,_

```sql
UPDATE ecommerce_churn
SET preferred_login_device = 'Phone'
WHERE LOWER(TRIM(preferred_login_device)) IN (
    'phone',
    'mobile phone'
);
```
> **Verification:** Below are the login devices in our records.

| preferred_login_device | record_count |
| ---------------------- | ------------ |
| Phone                  | 3996         |
| Computer               | 1634         |

### 4.2 Standardize Preferred Order Category

```sql
SELECT
    prefered_order_cat,
    COUNT(*) AS record_count
FROM ecommerce_churn
GROUP BY prefered_order_cat
ORDER BY record_count DESC;
```

| prefered_order_cat | record_count |
| ------------------ | ------------ |
| Laptop & Accessory | 2050         |
| Mobile Phone       | 1271         |
| Fashion            | 826          |
| Mobile             | 809          |
| Grocery            | 410          |
| Others             | 264          |

> **Assumption:** Assume that `Mobile Phone` and `Phone` represent the same category. We can standardized them into one label. _To do so,_

```sql
UPDATE ecommerce_churn
SET prefered_order_cat = 'Mobile'
WHERE LOWER(TRIM(prefered_order_cat)) IN (
    'mobile',
    'mobile phone'
);
```
> **Verification:** Below are the order categories in our records.

| prefered_order_cat | record_count |
| ------------------ | ------------ |
| Mobile             | 2080         |
| Laptop & Accessory | 2050         |
| Fashion            | 826          |
| Grocery            | 410          |
| Others             | 264          |

### 4.3 Standardize Preferred Payment Mode

Inspect the payment-mode labels to identify inconsistent capitalization, whitespace, abbreviations, or equivalent categories.

```sql
SELECT
    prefered_order_cat,
    COUNT(*) AS record_count
FROM ecommerce_churn
GROUP BY prefered_order_cat
ORDER BY record_count DESC;
```

| preferred_payment_mode | record_count |
| ---------------------- | ------------ |
| Debit Card             | 2314         |
| Credit Card            | 1501         |
| E wallet               | 614          |
| UPI                    | 414          |
| COD                    | 365          |
| CC                     | 273          |
| Cash on Delivery       | 149          |

> **Assumption:** We know that COD is the abbreviation for Cash on Delivery. Thus, replace `COD` with `Cash on Delivery`.  _To do so,_

```sql
UPDATE ecommerce_churn
SET preferred_payment_mode = CASE
    WHEN LOWER(TRIM(preferred_payment_mode)) IN ('cash on delivery', 'cod')
        THEN 'Cash on Delivery'
    WHEN LOWER(TRIM(preferred_payment_mode)) = 'debit card'
        THEN 'Debit Card'
    WHEN LOWER(TRIM(preferred_payment_mode)) = 'e wallet'
        THEN 'E wallet'
    WHEN LOWER(TRIM(preferred_payment_mode)) = 'upi'
        THEN 'UPI'
    WHEN LOWER(TRIM(preferred_payment_mode)) = 'cc'
        THEN 'CC'
    ELSE TRIM(preferred_payment_mode)
END
WHERE preferred_payment_mode IS NOT NULL;
```
> **Verification:** Below are the order categories in our records.

| preferred_payment_mode | record_count |
| ---------------------- | ------------ |
| Debit Card             | 2314         |
| Credit Card            | 1501         |
| E-Wallet               | 614          |
| Cash on Delivery       | 514          |
| UPI                    | 414          |
| CC                     | 273          |

**_Our dataset can now be used for exploratory data analysis and customer churn analysis._**
