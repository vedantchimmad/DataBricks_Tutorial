# 📝 Databricks SQL  

---

## 🔹 Introduction  
**Databricks SQL** is a **serverless data warehouse solution** built on top of the **Lakehouse architecture**.  
It allows analysts, BI developers, and data scientists to **query data in Delta Lake using SQL**.  

👉 It brings the **familiar SQL interface** to the **scalability of big data** and the **governance of Unity Catalog**.  

---

## 🧩 Key Features  

- 📊 **SQL Workspace** → Dedicated interface for queries, dashboards, and alerts.  
- 🚀 **Performance** → Photon engine accelerates queries for BI & analytics.  
- 🗂️ **Unity Catalog Integration** → Secure & governed access to data.  
- 🔄 **Delta Lake Support** → ACID transactions, time travel, schema enforcement.  
- ⚡ **Serverless** → No cluster management required; Databricks handles compute.  
- 📈 **Dashboards & Alerts** → Build real-time BI dashboards & set alerts.  
- 🔌 **BI Tool Integration** → Connect Power BI, Tableau, Looker seamlessly.  

---

## 🖼️ Databricks SQL Architecture  

```
                    👩‍💼 Analysts / BI Developers
                               │
                               ▼
                         📝 Databricks SQL
                 (Workspace, Dashboards, Editor)
                               │
              ┌────────────────┼────────────────┐
              │                │                │
         🚀 Photon Engine   🗂️ Unity Catalog   🛡️ Governance
              │                │                │
              └───────────────▼────────────────┘
                               │
                          💎 Delta Lake
                  (Tables, Views, Time Travel)
                               │
                               ▼
                         ☁️ Cloud Storage
```

---

## 🧑‍💻 Databricks SQL in Action  

### 1️⃣ Creating a Table in Unity Catalog  
```sql
CREATE TABLE sales.orders (
    order_id STRING,
    customer_id STRING,
    amount DOUBLE,
    order_date DATE
);
````

---

### 2️⃣ Querying Data

```sql
SELECT customer_id, SUM(amount) AS total_spent
FROM sales.orders
WHERE order_date >= '2024-01-01'
GROUP BY customer_id
ORDER BY total_spent DESC;
```

---

### 3️⃣ Time Travel with Delta Lake

```sql
-- Query older snapshot (version 3)
SELECT * FROM sales.orders VERSION AS OF 3;
```

---

### 4️⃣ Building a Dashboard

* 📊 Create SQL queries inside **Databricks SQL Editor**.
* 📈 Visualize results with charts (bar, pie, line, maps).
* 🔔 Schedule refreshes & set alerts when KPIs cross thresholds.

---

## ✅ Benefits

* 🛠️ **For Analysts** → Simple SQL experience, no Spark knowledge needed.
* 🚀 **For Enterprises** → High performance with Photon & caching.
* 🔒 **For Governance** → Fully integrated with Unity Catalog & fine-grained controls.
* ⚡ **For BI** → Connect to existing tools with JDBC/ODBC.
* 💵 **Cost Efficiency** → Serverless option avoids cluster costs when idle.

---

## 🌟 Example Use Case

1. **Retail company** stores raw sales data in Delta Lake.
2. **Databricks SQL** queries sales to compute KPIs (revenue, top customers).
3. **Dashboards** are built for daily executive reporting.
4. **Alerts** notify sales managers when revenue drops below a threshold.
5. **BI Tools** (Power BI, Tableau) connect directly to Databricks SQL endpoint.

---
