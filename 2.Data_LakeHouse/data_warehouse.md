# 🏛️ Data Warehouse  

A **Data Warehouse (DW)** is a **centralized repository** designed for **reporting, analytics, and decision-making**. It stores **structured, historical data** collected from different sources and is optimized for **query performance** rather than transactional operations.  

---

## 📌 Key Characteristics of a Data Warehouse  

- ✅ **Schema-on-write** → Data must be cleaned and structured before loading.  
- ✅ **OLAP (Online Analytical Processing)** → Optimized for aggregation & analytics.  
- ✅ **Historical Storage** → Stores large volumes of past data for trend analysis.  
- ✅ **ETL-based** → Data is Extracted, Transformed, and Loaded into DW.  
- ✅ **High Performance** → Uses indexing, partitioning, and columnar storage.  
- ✅ **Governance & Security** → Strong RBAC, encryption, and compliance.  

---

## 🏗️ Data Warehouse Architecture  

```
            📊 Source Systems
      ┌───────────────────────────┐
      │ ERP | CRM | IoT | APIs    │
      └───────────────────────────┘
                    │
                    ▼
            🔄 ETL / ELT Process
      ┌───────────────────────────┐
      │ Extract | Transform | Load │
      └───────────────────────────┘
                    │
                    ▼
         🏛️ Data Warehouse Storage
      ┌───────────────────────────┐
      │ Staging | Fact | Dimension │
      └───────────────────────────┘
                    │
                    ▼
            📈 BI / Analytics Layer
      ┌───────────────────────────┐
      │ Dashboards | Reports | ML │
      └───────────────────────────┘
```

## 📊 Data Warehouse Schema Types  

1. **⭐ Star Schema**  
   - Central **Fact Table** (measures like sales, revenue)  
   - Linked **Dimension Tables** (customers, products, time)  

2. **❄️ Snowflake Schema**  
   - Dimensions are **normalized** into sub-dimensions.  
   - More complex but reduces data redundancy.  

3. **📦 Galaxy Schema (Fact Constellation)**  
   - Multiple fact tables sharing dimension tables.  

---

## ⚙️ Popular Data Warehouses  

- **On-Premise**: Oracle, Teradata, IBM Netezza  
- **Cloud-based**:  
  - 🔹 **Snowflake**  
  - 🔹 **Amazon Redshift**  
  - 🔹 **Google BigQuery**  
  - 🔹 **Azure Synapse Analytics**  

---

## 🆚 Data Warehouse vs Data Lake vs Delta Lake  

| Feature | 🏛️ Data Warehouse | 🪣 Data Lake | 🌊 Delta Lake |
|---------|------------------|--------------|---------------|
| Schema | Schema-on-write | Schema-on-read | Schema-on-read + enforcement |
| Data Type | Structured | Structured + Semi-structured | Structured + Semi-structured |
| Transactions | ✅ Yes | ❌ No | ✅ Yes |
| Cost | 💲💲 High | 💲 Low | 💲 Balanced |
| Performance | ⚡ Very Fast | ⚡ Slow | ⚡ Fast (with optimizations) |
| Use Case | BI, Dashboards, Analytics | Raw storage, ML input | Lakehouse (BI + ML) |

---

## 📝 Example Query (SQL in DW)

```sql
SELECT c.customer_region, SUM(s.sales_amount) AS total_sales
FROM fact_sales s
JOIN dim_customer c ON s.customer_id = c.customer_id
JOIN dim_date d ON s.date_id = d.date_id
WHERE d.year = 2025
GROUP BY c.customer_region
ORDER BY total_sales DESC;
````

---

✅ **In short**:

* **Data Warehouse** = Optimized for **business reporting & analytics**.
* Best suited for **structured, historical, and aggregated data**.
* Complements **Data Lakes/Delta Lake** in a **modern Lakehouse architecture**.
