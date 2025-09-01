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