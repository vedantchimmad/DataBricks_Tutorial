# 🏗️ Unity Catalog Objects Explained with Code & Sentences  

Below are the **core Unity Catalog objects** explained with **definitions, examples, and SQL/Python code** for better understanding.  

---

## 1️⃣ **Metastore**
- **Definition:** The **top-most governance container** in Unity Catalog. Each Databricks region can have only **one metastore**. It manages all catalogs and security policies.  
- **Example:** `company_metastore`  

```sql
-- Assign a metastore to a workspace
CREATE METASTORE company_metastore
LOCATION 's3://company-bucket/metastore';

-- Check current metastore
SHOW METASTORES;
````

👉 Think of a **Metastore** as the "root governance layer" for all your data.

---

## 2️⃣ **Catalog**

* **Definition:** A **top-level container** inside a metastore. It groups multiple schemas.
* **Example:** `finance`, `marketing`

```sql
-- Create a new catalog
CREATE CATALOG finance;

-- Switch to a catalog
USE CATALOG finance;
```

👉 A **Catalog** is like a "department folder" (e.g., Finance, Marketing) under the company root (Metastore).

---

## 3️⃣ **Schema (Database)**

* **Definition:** A **logical container** inside a catalog that holds tables, views, and functions.
* **Example:** `finance.sales`

```sql
-- Create a schema inside a catalog
CREATE SCHEMA finance.sales;

-- Switch to schema
USE SCHEMA finance.sales;
```

👉 A **Schema** is like a **sub-folder inside a catalog**, organizing related datasets.

---

## 4️⃣ **Table**

* **Definition:** A **structured dataset** governed by UC. Can be **Managed** (stored by Databricks) or **External** (stored in cloud storage).
* **Example:** `finance.sales.transactions`

```sql
-- Create a managed Delta table
CREATE TABLE finance.sales.transactions (
    transaction_id STRING,
    amount DOUBLE,
    transaction_date DATE
);

-- Insert data
INSERT INTO finance.sales.transactions VALUES ('TX1001', 250.75, '2025-01-01');
```

👉 A **Table** is where actual structured data (rows & columns) is stored.

---

## 5️⃣ **View**

* **Definition:** A **virtual table** created from a SQL query. It doesn’t store data but references other tables.
* **Example:** `CREATE VIEW sales_summary`

```sql
-- Create a view summarizing sales
CREATE VIEW finance.sales.sales_summary AS
SELECT transaction_date, SUM(amount) AS daily_sales
FROM finance.sales.transactions
GROUP BY transaction_date;

-- Query the view
SELECT * FROM finance.sales.sales_summary;
```

👉 A **View** is like a "saved query result" you can reuse anytime.

---

## 6️⃣ **Volume**

* **Definition:** A **container for files** (CSV, JSON, PDFs, images, ML datasets, etc.) within Unity Catalog. Volumes allow governance on **unstructured and semi-structured data**.
* **Example:** `finance.raw_files`

```sql
-- Create a volume in schema
CREATE VOLUME finance.sales.raw_files;

-- Upload files to the volume (Python)
dbutils.fs.cp("dbfs:/local/path/data.csv", "dbfs:/Volumes/finance/sales/raw_files/data.csv")
```

👉 A **Volume** is like a **folder for raw files**, governed by Unity Catalog security.

---

✅ **Hierarchy Recap (with Example Path):**

```
Metastore → Catalog → Schema → Table/View/Volume
company_metastore → finance → sales → transactions (table)
```