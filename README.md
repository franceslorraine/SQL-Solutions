# 🗄️ SQL SOLUTIONS

A collection of structured SQL case studies, query-based solutions, and data analytics projects showcasing my proficiency in data manipulation, database querying, and business problem-solving using SQL. 

---

## 📌 Repository Overview
This repository serves as a centralized portfolio for my SQL practice and real-world case studies. It covers a diverse range of data challenges focused on extracting, transforming, and analyzing relational data. Each project demonstrates my ability to translate abstract business questions into optimized SQL queries, uncover hidden patterns, and derive actionable insights.

---

## 🛠️ Skills & Techniques Demonstrated

* **Data Engineering & Hygiene**: Schema design, handling missing values, and data standardization.
* **Basic Querying & Filtering**: `SELECT`, `WHERE`, `ORDER BY`, `DISTINCT`, and basic conditional filtering.
* **Data Aggregation**: Grouping metrics using `GROUP BY`, conditional filtering with `HAVING`, and aggregate functions (`COUNT`, `SUM`, `AVG`, `ROUND`).
* **Advanced Joins**: Complex multi-table operations (`INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL OUTER JOIN`).
* **Subqueries & CTEs**: Breaking down complex analytical workflows using Common Table Expressions (`WITH` clauses) and nested subqueries.
* **Window Functions**: Advanced analytical calculations, ranking (`RANK`, `DENSE_RANK`), partitioning (`PARTITION BY`), and running totals.
* **Business Intelligence**: Translating raw data outputs into executive-ready takeaways and data-driven recommendations.

---

## 📂 Project Structure & Tiers

The repository is organized by project complexity to highlight progressive mastery of SQL concepts:

### 🟢 Basic Project
> *Foundational queries focusing on data extraction, filtering, basic sorting, and simple aggregations.*

* **[Analyzing Students' Mental Health](https://github.com/franceslorraine/SQL-Solutions/tree/d8b4c06bd6af0fcef4c1b53660f946ea7ef24d78/%5BBasic%5D-Analyzing-Students'-Mental-Health
)**: Investigates the relationship between international students' length of stay and mental health diagnostic scores (PHQ-9, SCS, ASISS) using foundational SQL filtering (`WHERE`), grouping (`GROUP BY`), data type casting, and aggregate functions (`COUNT`, `AVG`, `ROUND`).

### 🟡 Intermediate Project
> *Multi-table queries incorporating joins, aggregate groupings, case logic, and intermediate data transformations.*
* **[Analyzing and Formatting PostgreSQL Sales Data](https://github.com/franceslorraine/SQL-Solutions/tree/d8b4c06bd6af0fcef4c1b53660f946ea7ef24d78/%5BIntermediate%5D-Analyzing-%26-Formatting-PostgreSQL-Sales-Data)**: Analyzes 51,290 records in a Super Store dataset using PostgreSQL to rank top-performing products by category (leveraging window functions like `RANK()` and `PARTITION BY`) and solve data quality challenges by estimating missing order quantities via reference unit prices and Common Table Expressions (CTEs).

### 🔴 Advanced Project
> *Complex end-to-end case studies leveraging CTEs, window functions, multi-dimensional segmentation, and advanced conditional logic.*
* **[E-Commerce Customer Churn Analysis](https://github.com/franceslorraine/SQL-Solutions/tree/d8b4c06bd6af0fcef4c1b53660f946ea7ef24d78/%5BAdvanced%5D-E-Commerce-Customer-Churn-Analysis)**: An end-to-end case study analyzing 5,630 customer records in PostgreSQL (Supabase) to identify attrition vectors, evaluate behavioral divergence, and segment high-risk customer cohorts using advanced CTEs and conditional aggregations.

---

## 🚀 How to Navigate This Repository
1. Browse the project tiers above (**Basic**, **Intermediate**, **Advanced**).
2. Inside each project directory, you will find comprehensive documentation:
   - **`README.md`**: Complete business context, methodology, data preparation summary, key findings, and strategic recommendations.
   - **Query & EDA Sub-Folders**: Dedicated markdown files and sub-documentation containing production-ready, well-commented SQL queries, execution tables, and analytical takeaways.
3. Feel free to review the analytical workflows or test the SQL queries against your own PostgreSQL-compatible database environments.
   
---

*This repository is continuously updated as I take on new data engineering and analytics challenges to further expand my technical portfolio.*
