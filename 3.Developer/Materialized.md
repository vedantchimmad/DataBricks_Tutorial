# 📊 Materialized View in Databricks

## 🧠 What is a Materialized View?
A **Materialized View (MV)** is a **precomputed and stored result** of a query.  
Unlike a normal view, it **physically stores data**, so queries run faster ⚡

---

## 🔍 Key Features
- ⚡ **Faster query performance** (no need to recompute every time)
- 💾 **Stores data physically**
- 🔄 Can be **refreshed periodically**
- 📉 Useful for **aggregations & heavy transformations**

---

## 🆚 View vs Materialized View

| Feature              | View 👀                | Materialized View 📊        |
|---------------------|----------------------|-----------------------------|
| Data Storage        | ❌ No                | ✅ Yes                      |
| Query Speed         | 🐢 Slower            | ⚡ Faster                   |
| Refresh Needed      | ❌ No                | ✅ Yes                      |
| Use Case            | Simple queries       | Complex/aggregated queries |

---

## 🛠️ How to Create in Databricks

```sql
CREATE MATERIALIZED VIEW mv_sales_summary
AS
SELECT 
    category,
    SUM(sales) AS total_sales,
    COUNT(*) AS total_orders
FROM sales_table
GROUP BY category;
````

---

## 🔄 Refresh Materialized View

```sql
REFRESH MATERIALIZED VIEW mv_sales_summary;
```

---

## 🧩 Important Notes

* ⚠️ Not all Databricks environments support MV (depends on **Unity Catalog + Delta Live Tables / Lakehouse features**)
* 🔁 Refresh can be:

    * Manual 🖐️
    * Scheduled ⏰ (via workflows/jobs)

---

## 💡 When to Use?

* 📈 Dashboard reporting (Power BI, Tableau)
* 🔄 Frequently queried aggregations
* 📊 Large datasets with heavy joins

---

## 🚫 When NOT to Use?

* ❌ Real-time data requirement
* ❌ Small datasets
* ❌ Frequently changing base data (high refresh cost)

---

## 🎯 Example Use Case

👉 Sales dashboard needing:

* Total sales per region
* Daily revenue trends

➡️ Use MV to avoid recalculating every time

---

If you want, I can also show:

* 🧪 MV vs Delta Live Tables
* ⚙️ Incremental refresh setup
* 🔗 Real-time alternatives in Databricks

