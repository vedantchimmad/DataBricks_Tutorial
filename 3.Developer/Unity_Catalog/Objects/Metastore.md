# 🏗️ Unity Catalog — **Metastore**

## 🔹 What is a Metastore?
- A **Metastore** is the **top-most governance container** in Databricks Unity Catalog.  
- It manages **all catalogs, schemas, tables, views, and volumes** within a given **cloud region**.  
- A workspace can be **assigned to only one Metastore**.  
- Provides **centralized governance**: authentication, authorization, lineage, auditing, and data discovery.  

---

## 📌 Key Points
- 🌍 **One per region** → You can have only **one Metastore** in each cloud region.  
- 🏢 **Organization-wide** → A Metastore can be shared across multiple workspaces.  
- 🔐 **Security boundary** → Defines **access control policies** at the highest level.  
- 📦 **Container for catalogs** → Each Metastore contains multiple catalogs.  

---

## 🗂️ Hierarchy
```

Metastore → Catalog → Schema → Tables / Views / Volumes

````

Example Path:  
`company_metastore.finance.sales.transactions`

---

## 🛠️ SQL Commands

```sql
-- Create a new Metastore
CREATE METASTORE company_metastore
LOCATION 's3://company-bucket/uc-metastore';

-- Assign Metastore to a workspace
ALTER METASTORE company_metastore
SET OWNER TO `admin@company.com`;

-- List available metastores
SHOW METASTORES;

-- Check which Metastore current workspace is using
DESCRIBE METASTORE;
````

---

## 🐍 Python API (Databricks SDK)

```python
from databricks.sdk import WorkspaceClient

w = WorkspaceClient()

# List all metastores
metastores = w.metastores.list()
for m in metastores:
    print(m.name, m.id, m.region)

# Get details of a specific metastore
metastore = w.metastores.get(metastores[0].id)
print(metastore.name, metastore.storage_root)
```

---

## 🎯 Example Sentence

* **"Our company created a `company_metastore` in AWS `us-east-1` region, which stores governance rules and links multiple catalogs like `finance` and `marketing`."**

---

✅ In short: A **Metastore** is the **root governance layer** of Unity Catalog that ensures centralized **security and data management** across all Databricks workspaces in a region.
