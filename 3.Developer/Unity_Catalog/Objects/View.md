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
