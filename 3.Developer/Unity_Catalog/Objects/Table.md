# 📊 Unity Catalog — **Table**

## 🔹 What is a Table?
- A **Table** is a **structured dataset** stored in Databricks Unity Catalog.  
- It can be:
  - **Managed Table** → Databricks manages both the metadata and the underlying storage.  
  - **External Table** → Metadata is managed by Unity Catalog, but data resides in external storage (e.g., ADLS, S3).  
- Tables can be queried using **SQL** or APIs, and support **Delta, Parquet, CSV, JSON** formats.  

---

## 📌 Key Points
- 🏗️ **Hierarchy level**: `Metastore → Catalog → Schema → Table`  
- 📦 **Data storage unit** → Contains rows and columns.  
- 🔐 **Access control** → Permissions can be applied at the table level.  
- ♻️ **Supports ACID transactions** when created as **Delta tables**.  
- 📊 **Supports schema evolution** and **time travel** (for Delta).  

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
-- Create a Managed Delta Table
CREATE TABLE finance.sales.transactions (
    transaction_id STRING,
    customer_id STRING,
    amount DECIMAL(10,2),
    transaction_date DATE
) USING DELTA;

-- Create an External Table (data stored outside UC managed storage)
CREATE TABLE finance.sales.customers
USING PARQUET
LOCATION 'abfss://datalake/finance/sales/customers/';

-- Insert data into table
INSERT INTO finance.sales.transactions
VALUES ("T001", "C123", 100.50, "2025-08-25");

-- Query a table
SELECT * FROM finance.sales.transactions;

-- Drop a table
DROP TABLE finance.sales.customers;
````

---

## 🐍 Python API (Databricks SDK)

```python
from databricks.sdk import WorkspaceClient

w = WorkspaceClient()

# Create a Managed Table
table = w.tables.create(
    name="transactions",
    catalog_name="finance",
    schema_name="sales",
    table_type="MANAGED",
    comment="Transaction details for sales"
)

# List Tables in Schema
tables = w.tables.list(catalog_name="finance", schema_name="sales")
for t in tables:
    print(t.name, t.table_type)

# Delete a Table
w.tables.delete(name="customers", catalog_name="finance", schema_name="sales")
```

---

## 🎯 Example Sentence

* **"We created a managed Delta table `finance.sales.transactions` to store all transaction records with ACID support and time travel."**

---

✅ In short: A **Table** in Unity Catalog is a **structured dataset** that can be **Managed (Databricks controlled)** or **External (user controlled storage)**, supporting powerful Delta features like **ACID, schema evolution, and time travel**.

---
# 📊 Types of Tables in Unity Catalog (UC)

In **Databricks Unity Catalog**, a **Table** is a governed dataset that can be queried using SQL.  
UC provides different types of tables depending on how the data is **stored** and **managed**.

---

## 🔹 1. Managed Tables
- Databricks **manages both the metadata & data files**.
- Data is stored in a **default location** managed by Unity Catalog.
- Dropping the table removes **both metadata and data**.

✅ **Use case**: When you want UC to fully control lifecycle of data.  

```sql
-- Create Managed Table
CREATE TABLE finance.sales.transactions (
    id INT,
    amount DOUBLE,
    customer STRING
);

-- UC stores files in default managed storage
````

---

## 🔹 2. External Tables

* Metadata is in Unity Catalog, but **data is stored externally** (e.g., in ADLS, S3, GCS).
* Dropping the table **only removes metadata**, not the data files.
* Useful when data is shared between multiple platforms.

✅ **Use case**: When you already have data in a lake (Parquet, Delta, CSV).

```sql
-- Create External Table
CREATE TABLE finance.sales.external_transactions
USING DELTA
LOCATION 's3://my-bucket/sales_data/';
```

---

## 🔹 3. Delta Tables

* Tables backed by the **Delta Lake format** (`.delta`).
* Support **ACID transactions, time travel, schema evolution**.
* Can be **Managed** or **External**.

✅ **Use case**: Most common in modern Lakehouse architectures.

```sql
-- Delta Table example
CREATE TABLE finance.sales.delta_transactions
USING DELTA
AS SELECT * FROM parquet.`/mnt/data/sales/`;
```

---

## 🔹 4. Temporary Tables

* Session-scoped tables (disappear when session ends).
* Used for intermediate transformations.
* Not governed by UC.

✅ **Use case**: Testing, quick exploration.

```sql
-- Temporary Table
CREATE TEMPORARY TABLE temp_sales (id INT, amount DOUBLE);
```

---

## 🔹 5. Views (Virtual Tables)

* Not physical storage → Just a **saved SQL query**.
* Can be **Managed in UC** like tables.
* Used for abstraction, simplification, and security.

✅ **Use case**: Share filtered or masked data.

```sql
-- Create View
CREATE VIEW finance.sales.sales_summary AS
SELECT customer, SUM(amount) as total_spent
FROM finance.sales.transactions
GROUP BY customer;
```

---

# 📑 Quick Comparison

| Table Type          | Data Location                 | Lifecycle Control          | UC Governance | Example Use Case                       |
| ------------------- | ----------------------------- | -------------------------- | ------------- | -------------------------------------- |
| **Managed Table**   | UC-managed storage            | UC deletes data & metadata | ✅             | Default, full control                  |
| **External Table**  | External path (S3, ADLS, GCS) | UC deletes only metadata   | ✅             | Data shared across systems             |
| **Delta Table**     | Managed or External           | Depends                    | ✅             | Time travel, ACID, schema evolution    |
| **Temporary Table** | In-memory / session storage   | Auto-removed after session | ❌             | Testing, intermediate data             |
| **View**            | No data (query only)          | Only metadata stored       | ✅             | Abstractions, security, simplification |

---

## 🎯 Example Sentence

* **"The `finance.sales.transactions` table is a managed Delta table, while `finance.sales.external_transactions` points to raw Parquet files in S3 as an external table."**

