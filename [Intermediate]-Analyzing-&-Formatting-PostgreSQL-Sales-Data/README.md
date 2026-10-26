# ANALYZING AND FORMATTING POSTGRESQL SALES DATA
A SQL case study using PostgreSQL to analyze sales performance, rank top-performing products, and address missing data across 51,290 records in a hypothetical Super Store dataset. This project demonstrates how SQL can support product performance analysis, data quality management, and data-driven business decisions.

**Project Source:** [DataCamp — Analyzing and Fromatting PostgreSQL Sales Data](https://projects.datacamp.com/projects/2610)

## 📌 Business Problem
Retail businesses need reliable sales data to evaluate product performance, understand profitability, and make informed inventory and merchandising decisions. However, analyzing large transactional datasets can be challenging when sales records contain missing values or when performance needs to be compared across multiple product categories.

This project addresses two analytical challenges:
- **Product Performance Analysis**: Identify the top five products by total sales within each category while evaluating their corresponding profit contributions.
- **Data Quality and Missing Value Imputation**: Estimate missing order quantities using historical unit-price information from comparable transactions.

## 🎯 Objectives & Requirements
- Rank products by total sales within each category.
- Calculate total sales and profit for individual products.
- Identify products that perform well in terms of sales.
- Investigate missing quantity values in transactional records.
- Estimate missing quantities using comparable unit-price data.

## 🛠️ Approach
The analysis uses four relational tables: `orders, products, returned_orders`, and `people`. The queries in this project primarily use the `orders` and `products` tables.

### 1. Top 5 Products Per Category
**Goal:** Identify the five highest-selling products within each product category.
**SQL Techniques:**
- `INNER JOIN` to combine transactional records with product information.
- `SUM()` to aggregate sales and profit by product.
- `RANK()` to rank products independently within each category.
- Subqueries to organize the aggregation and ranking logic.
- `ROUND()` and `CAST()` to format monetary values to two decimal places.
- Filtering with `WHERE` to retain the top five rank positions.

### SQL Query
````sql
SELECT *
FROM (
    SELECT
        p.category,
        p.product_name,
        ROUND(CAST(SUM(o.sales) AS NUMERIC), 2) AS product_total_sales,
        ROUND(CAST(SUM(o.profit) AS NUMERIC), 2) AS product_total_profit,
        RANK() OVER (
            PARTITION BY p.category
            ORDER BY SUM(o.sales) DESC
        ) AS product_rank
    FROM orders AS o
    INNER JOIN products AS p
        ON o.product_id = p.product_id
    GROUP BY
        p.category,
        p.product_name
) AS tmp
WHERE product_rank < 6
ORDER BY
    category ASC,
    product_total_sales DESC;
````

### Analytical Approach
- Joined order transactions with product information using `product_id`.
- Aggregated total sales and total profit for each product name within its category.
- Applied `RANK()` with `PARTITION BY` to rank products by sales independently within each category.
- Filtered for ranks below 6 to retrieve the top five rank positions.

> **Important Consideration**: `RANK()` assigns the same rank to products with tied sales and skips subsequent rank numbers. Consequently, a category may return more than five products when ties occur at the fifth rank. Because the query groups by category and product name rather than product ID, products with identical names within a category are aggregated together.

### 🧾 Results
<div align="center">
  <img width="1205" height="547" alt="image" src="https://github.com/user-attachments/assets/b3a957f9-dc80-4150-9c21-c5b1cc9cc71c" />
</div>

### 🔎 Key Findings
- **Leading Products by Sales:** The analysis identifies the highest-ranked products in each category. The _Apple Smart Phone, Full Size_ leads Technology with approximately $86,935.78 in total sales, while the _Hon Executive Leather Armchair, Adjustable_ leads Furniture with approximately $58,193.48.
- **Differences Between Sales and Profitability:** The results reveal that **high sales do not necessarily translate into positive profit**. For example, the Hoover Stove, White ranks second in Office Supplies with $32,842.60 in sales but records a negative total profit of $2,180.63.

## 💡 Strategic Implications
- **Revenue vs. Profitability**: Sales-based rankings identify products generating substantial revenue, but they do not establish which products contribute most to profitability. Products with negative profit warrant further investigation into pricing, discounts, costs, and other contributing factors. A complementary analysis ranking products by total profit or calculating profit margins would provide a more complete view of product performance.
- **Product Identification and Aggregation**: The query groups records by category and product name rather than a unique product identifier. If distinct products share the same name within a category, their results may be combined. Using `product_id` alongside product name would help preserve product-level distinctions and improve analytical reliability.

### 2. Estimating Missing Order Quantities
**Goal:** Estimate missing quantity values using unit-price information from comparable transactions while retaining the original order data.
**SQL Techniques:**
- Common Table Expressions (CTEs) to isolate missing quantities and derive reference unit prices.
- `RIGHT JOIN` to associate missing orders with transactions that have observed quantities.
- `INNER JOIN` to match missing records with reference unit prices.
- `CAST()` to convert calculated values to a numeric data type.
- `ROUND()` to round estimated quantities to whole numbers.
- `DISTINCT` to remove duplicate result rows.
  
### SQL Query
````sql
WITH missing AS (
	SELECT product_id,
		discount, 
		market,
		region,
		sales,
		quantity
	FROM orders 
	WHERE quantity IS NULL
), 

unit_prices AS (SELECT o.product_id,
	CAST(o.sales / o.quantity AS NUMERIC) AS unit_price
FROM orders o
RIGHT JOIN missing AS m 
	ON o.product_id = m.product_id
	AND o.discount = m.discount
WHERE o.quantity IS NOT NULL
)

SELECT DISTINCT m.*,
	ROUND(CAST(m.sales AS NUMERIC) / up.unit_price,0) AS calculated_quantity
FROM missing AS m
INNER JOIN unit_prices AS up
	ON m.product_id = up.product_id;
````

### Analytical Approach
- Isolated orders with missing quantity values using the missing CTE.
- Derived reference unit prices from transactions with available quantities by dividing sales by quantity.
- Matched reference transactions to missing orders using `product_id` and `discount`.
- Joined the missing records with the derived unit prices using `product_id`.
- Estimated missing quantities by dividing sales by the matched unit price and rounding the result to a whole number.
- Applied `DISTINCT` to remove duplicate output rows.

> **Important Consideration**: The query matches reference transactions using both product ID and discount when deriving unit prices, but its final join uses only product ID. It does not explicitly match market or region, even though those columns are retained in the output. Multiple reference transactions may therefore produce different estimates for a missing order. The INNER JOIN also excludes missing orders without a matching reference unit price. The query assumes that the reference unit price is suitable for estimating the missing quantity and does not explicitly protect against division by zero.

### 🧾 Results
<div align="center">
   <img width="1050" height="210" alt="image" src="https://github.com/user-attachments/assets/b4e32cf9-2e45-4172-9c89-3c1e0f190857" />
</div>

## 🔎 Key Findings
- **Missing-Value Estimation:** The query estimates missing quantities by dividing recorded sales by a reference unit price derived from transactions involving the same product and discount. For example, the results include an estimated quantity of 4 for product FUR-ADV-10000571, based on sales of $438.96.
- **Data Completeness:** The approach provides estimates for missing quantities when matching reference transactions are available. These estimates can support downstream analysis, although they should be distinguished from observed quantities.

## 💡Operational Implications
- **Inconsistent Matching Criteria**: The query uses `product_id` and `discount` to associate reference transactions with missing orders when deriving unit prices, but its final join matches records using only `product_id`. It also does not explicitly match market or region. _Consequently, a missing order may be paired with multiple reference prices from different discounts, markets, or regions, potentially producing inconsistent quantity estimates._
- **Division-by-Zero and Data Validation**: The query does not explicitly use `NULLIF()` to guard against a zero reference unit price. A zero or invalid unit price can cause calculation errors or unreliable estimates. _Additional validation should check reference prices, estimated quantities, and unusual sales values before the estimates are used in operational reporting._
- **Estimation Uncertainty**: The calculated quantities are estimates rather than recovered ground-truth values. Rounding can introduce discrepancies, and missing orders without a matching reference transaction are excluded by the `INNER JOIN`. _These limitations should be considered before using the results for inventory planning or demand forecasting._


> **Note on Data Access & Environment:** 
> This analysis was completed in DataCamp Workspace using its pre-loaded dataset of 51,290 order records. The findings reflect the available dataset.

## ⭐ Skills Demonstrated
- Common Table Expressions (CTEs) for structuring multi-step SQL transformations.
- SQL aggregation using `SUM()` and `GROUP BY`.
- Window functions using `RANK()` and `PARTITION BY` for category-level ranking.
- Relational joins using `INNER JOIN` and `INNER JOIN`.
- Data type conversion and numerical formatting using `CAST()` and `ROUND()`.
- Duplicate handling using `DISTINCT`.
- Critical interpretation of revenue and profitability metrics.
- Evaluation of data quality, join logic, and imputation limitations.

