# 📂 Unity Catalog — **Volume**

## 🔹 What is a Volume?
- A **Volume** is a governance-aware **container for files** inside Unity Catalog.  
- Unlike **Tables** (structured, SQL-queriable), Volumes are meant for **unstructured or semi-structured data**:
  - CSV, JSON, Parquet (before being converted to tables)
  - Images, PDFs, Audio, Video
  - Machine Learning datasets, model artifacts, etc.  
- Provides **access control, auditing, and lineage** just like Tables and Views.  
- Acts like a **secure, organized folder** managed within Unity Catalog.

---

## 📌 Key Points
- 🏗️ **Hierarchy**: `Metastore → Catalog → Schema → Volume → Files`  
- 📦 **Stores raw or reference files** (not queryable as SQL tables directly).  
- 🔐 **Access control** → Permissions at volume & file level.  
- 🔄 **Can be converted** into Delta Tables later.  
- ⚡ **Works with dbutils.fs & Python APIs** for easy file operations.  

---

## 🗂️ Example Hierarchy
```

Metastore: company\_metastore
└── Catalog: finance
└── Schema: raw
├── Table: transactions
├── View: sales\_summary
└── Volume: raw\_files
├── 2025\_01\_sales.csv
├── customer\_profiles.json
└── invoices/
└── invoice\_101.pdf

````

---

## 🛠️ SQL Commands

```sql
-- Create a Volume inside a Schema
CREATE VOLUME finance.raw.raw_files;

-- Upload files (using UI/CLI, not SQL directly)
-- Example: Upload 2025_01_sales.csv into the volume

-- List files in a volume
LIST 'dbfs:/Volumes/finance/raw/raw_files/';

-- Use files from a Volume in a SQL table
CREATE TABLE finance.raw.transactions
USING CSV
OPTIONS (path 'dbfs:/Volumes/finance/raw/raw_files/2025_01_sales.csv', header "true");
````

---

## 🐍 Python API & dbutils.fs

```python
from databricks.sdk import WorkspaceClient

w = WorkspaceClient()

# Create a volume
volume = w.volumes.create(
    name="raw_files",
    catalog_name="finance",
    schema_name="raw",
    comment="Stores raw input files for analytics"
)

print(f"Created Volume: {volume.name}")

# Upload file into Volume (example with dbutils)
dbutils.fs.cp("file:/local/path/2025_01_sales.csv",
              "dbfs:/Volumes/finance/raw/raw_files/2025_01_sales.csv")

# List files in the Volume
files = dbutils.fs.ls("dbfs:/Volumes/finance/raw/raw_files/")
for f in files:
    print(f.name, f.size)

# Read a CSV file from Volume into Spark DataFrame
df = spark.read.csv("dbfs:/Volumes/finance/raw/raw_files/2025_01_sales.csv", header=True)
df.show()
```

---

## 🎯 Example Sentence

* **"The `finance.raw.raw_files` volume stores raw CSVs and PDFs uploaded by business teams before data engineers convert them into Delta tables."**

---

✅ In short: A **Volume** is a **managed file container** in Unity Catalog for raw, unstructured, or semi-structured data, with governance, permissions, and easy integration into Delta workflows.
