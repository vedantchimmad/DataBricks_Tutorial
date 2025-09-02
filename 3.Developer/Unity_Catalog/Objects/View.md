# 👁️ Unity Catalog — **View**

## 🔹 What is a View?
- A **View** is a **virtual table** in Databricks Unity Catalog.  
- It is defined by a **SQL query** and **does not store data physically** (unlike tables).  
- Views allow you to **simplify queries, encapsulate logic, and secure data** by controlling what is exposed to users.  
- Two types of views:
  - **Standard View** → Created with `CREATE VIEW`, always reflects the latest underlying data.  
  - **Materialized View** → Stores results of the query for faster access (but needs refresh).  

---

## 📌 Key Points
- 🏗️ **Hierarchy level**: `Metastore → Catalog → Schema → View`  
- 📦 **No physical storage** (except for materialized views).  
- 🔐 **Access control** → Restrict access to sensitive columns via views.  
- ♻️ **Dynamic** → Always returns results based on current underlying table(s).  
- ⚡ **Performance optimization** with Materialized Views.  

---

## 🗂️ Example Hierarchy
```

Metastore: company\_metastore
└── Catalog: finance
└── Schema: sales
├── Table: transactions
├── Table: customers
└── View: sales\_summary

````

---

## 🛠️ SQL Commands

```sql
-- Create a standard view
CREATE VIEW finance.sales.sales_summary AS
SELECT customer_id, SUM(amount) AS total_spent
FROM finance.sales.transactions
GROUP BY customer_id;

-- Query the view
SELECT * FROM finance.sales.sales_summary;

-- Replace or update a view definition
CREATE OR REPLACE VIEW finance.sales.sales_summary AS
SELECT customer_id, COUNT(*) AS purchase_count
FROM finance.sales.transactions
GROUP BY customer_id;

-- Drop a view
DROP VIEW finance.sales.sales_summary;

-- Create a Materialized View (stores query results)
CREATE MATERIALIZED VIEW finance.sales.top_customers AS
SELECT customer_id, SUM(amount) AS total_spent
FROM finance.sales.transactions
GROUP BY customer_id
ORDER BY total_spent DESC
LIMIT 10;
````

---

## 🐍 Python API (Databricks SDK)

```python
from databricks.sdk import WorkspaceClient

w = WorkspaceClient()

# Create a View
view = w.views.create(
    name="sales_summary",
    catalog_name="finance",
    schema_name="sales",
    definition="""
        SELECT customer_id, SUM(amount) AS total_spent
        FROM finance.sales.transactions
        GROUP BY customer_id
    """,
    comment="Summary of total spent per customer"
)

# List Views in a Schema
views = w.views.list(catalog_name="finance", schema_name="sales")
for v in views:
    print(v.name, v.definition)

# Delete a View
w.views.delete(name="sales_summary", catalog_name="finance", schema_name="sales")
```

---

## 🎯 Example Sentence

* **"Instead of querying the `transactions` table directly, analysts use the `sales_summary` view to see each customer’s total spending."**

---

✅ In short: A **View** is a **virtual table** defined by a SQL query that makes data consumption easier, reusable, and more secure without duplicating storage.

---
# 👓 Different Types of Views in Databricks (Unity Catalog & Delta Lake)

Views in Databricks are **virtual tables** that do not store data themselves but represent results of queries.  
They simplify query logic, improve security, and support modular data pipelines.  

---

## 1️⃣ **Standard (Logical) Views**
- **Definition:** A simple SQL query saved as a named object.
- **Storage:** No physical data, just metadata + query logic.
- **Use Case:** Simplify repeated queries or join logic.
- **Example:**
```sql
-- Create a simple logical view
CREATE VIEW sales_summary AS
SELECT region, SUM(amount) AS total_sales
FROM sales_data
GROUP BY region;

-- Query the view
SELECT * FROM sales_summary;
````

---

## 2️⃣ **Global Views**

* **Definition:** Views that are accessible across all sessions and clusters (but limited to the same metastore).
* **Created under:** `global_temp` schema.
* **Use Case:** When multiple notebooks or jobs need to share the same view.
* **Example:**

```sql
-- Create a global view
CREATE GLOBAL TEMP VIEW global_sales_summary AS
SELECT region, COUNT(*) AS orders
FROM sales_data
GROUP BY region;

-- Query the global view
SELECT * FROM global_temp.global_sales_summary;
```

---

## 3️⃣ **Temporary Views**

* **Definition:** Session-scoped views that disappear when the session ends.
* **Storage:** Only in memory (not stored in metastore).
* **Use Case:** Useful for quick transformations or debugging.
* **Example:**

```sql
-- Create a temporary view
CREATE OR REPLACE TEMP VIEW temp_sales AS
SELECT * FROM sales_data WHERE year = 2025;

-- Query temporary view
SELECT COUNT(*) FROM temp_sales;
```

---

## 4️⃣ **Materialized Views (Managed Tables in Delta Lake)**

* **Definition:** Pre-computed, stored results of a query (like a table but auto-refreshed).
* **Performance:** Faster for repeated queries, BI dashboards.
* **Use Case:** Reporting, aggregation-heavy queries.
* **Example:**

```sql
-- Create a materialized view
CREATE MATERIALIZED VIEW monthly_sales_summary
AS
SELECT year, month, SUM(amount) AS total_sales
FROM sales_data
GROUP BY year, month;
```

⚡ Databricks automatically **refreshes** the materialized view when the underlying data changes.

---

## 5️⃣ **Secure Views (Unity Catalog)**

* **Definition:** Views with **row-level or column-level security** applied.
* **Use Case:** Hide sensitive data or enforce data governance policies.
* **Example:**

```sql
-- Secure view that hides customer emails
CREATE VIEW secure_customer_view
AS
SELECT customer_id, region
FROM customers;
```

Admins can grant permissions on the secure view without exposing sensitive columns.

---

# 📊 Summary of View Types

| View Type             | Scope / Storage                       | Use Case                                |
| --------------------- | ------------------------------------- | --------------------------------------- |
| **Standard View**     | SQL metadata, persisted               | Simplify query logic                    |
| **Global View**       | Shared across clusters (global\_temp) | Multi-notebook access                   |
| **Temporary View**    | Session-only (in memory)              | Debugging, quick analysis               |
| **Materialized View** | Stored results, auto-refreshed        | BI dashboards, performance optimization |
| **Secure View**       | Governed view with security           | Compliance, access control              |

---

💡 **Tip:**

* Use **Temporary Views** for short-lived exploration.
* Use **Materialized Views** for **performance-heavy analytics**.
* Use **Secure Views** with **Unity Catalog** for compliance and governance.


