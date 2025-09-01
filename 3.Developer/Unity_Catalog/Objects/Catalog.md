# 📂 Unity Catalog — **Catalog**

## 🔹 What is a Catalog?
- A **Catalog** is a **top-level container** inside a **Metastore**.  
- It is used to **organize schemas (databases)** logically across teams, projects, or domains.  
- Acts like a **namespace** that groups related schemas together.  

---

## 📌 Key Points
- 🏗️ **Hierarchy level**: `Metastore → Catalog → Schema → Tables/Views/Volumes`  
- 📦 **Container for schemas** → Each Catalog can hold multiple schemas.  
- 🔐 **Access control** → Permissions can be applied at Catalog level to control who can create or access schemas and tables.  
- 🌍 **Multi-team support** → Different teams (e.g., `finance`, `marketing`, `engineering`) can have their own **Catalogs**.  

---

## 🗂️ Example Hierarchy
```

Metastore: company\_metastore
├── Catalog: finance
│      ├── Schema: sales
│      │       ├── Table: transactions
│      │       └── View: sales\_summary
│      └── Schema: hr
└── Catalog: marketing
└── Schema: campaigns

````

---

## 🛠️ SQL Commands

```sql
-- Create a new Catalog
CREATE CATALOG finance;

-- Grant usage on a Catalog
GRANT USE CATALOG ON CATALOG finance TO `analyst@company.com`;

-- Show available catalogs
SHOW CATALOGS;

-- Drop a Catalog
DROP CATALOG marketing;
````

---

## 🐍 Python API (Databricks SDK)

```python
from databricks.sdk import WorkspaceClient

w = WorkspaceClient()

# Create a catalog
catalog = w.catalogs.create(name="finance", comment="Finance domain catalog")

# List all catalogs
for cat in w.catalogs.list():
    print(cat.name, cat.comment)

# Delete a catalog
w.catalogs.delete(name="marketing")
```

---

## 🎯 Example Sentence

* **"We created a `finance` catalog inside the `company_metastore` to group all finance-related schemas like `sales` and `hr`."**

---

✅ In short: A **Catalog** is a **namespace within a Metastore** that helps organize multiple **schemas/databases** for teams or projects.