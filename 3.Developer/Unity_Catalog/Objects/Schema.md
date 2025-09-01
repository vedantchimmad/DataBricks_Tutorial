# 🗃️ Unity Catalog — **Schema (Database)**

## 🔹 What is a Schema?
- A **Schema** (also known as a **Database**) is a **logical container** inside a **Catalog**.  
- It is used to organize **tables, views, functions, and volumes** within a domain or project.  
- Helps separate **data assets** in a structured way for teams and projects.  

---

## 📌 Key Points
- 🏗️ **Hierarchy level**: `Metastore → Catalog → Schema → Tables/Views/Volumes`  
- 📦 **Container for tables & views** → Stores actual data objects.  
- 🔐 **Access control** → Permissions can be applied at Schema level for finer control.  
- 🌍 **Multiple schemas per catalog** → Example: `finance.sales`, `finance.hr`  

---

## 🗂️ Example Hierarchy
```

Metastore: company\_metastore
└── Catalog: finance
├── Schema: sales
│       ├── Table: transactions
│       ├── Table: customers
│       └── View: sales\_summary
└── Schema: hr
├── Table: employees
└── View: headcount\_report

````

---

## 🛠️ SQL Commands

```sql
-- Create a new Schema inside the finance catalog
CREATE SCHEMA finance.sales;

-- Switch to a Schema
USE SCHEMA finance.sales;

-- Show available Schemas in a Catalog
SHOW SCHEMAS IN finance;

-- Drop a Schema
DROP SCHEMA finance.hr CASCADE;
````

---

## 🐍 Python API (Databricks SDK)

```python
from databricks.sdk import WorkspaceClient

w = WorkspaceClient()

# Create a Schema
schema = w.schemas.create(
    name="sales",
    catalog_name="finance",
    comment="Schema for sales-related tables"
)

# List Schemas in a Catalog
schemas = w.schemas.list(catalog_name="finance")
for s in schemas:
    print(s.name, s.comment)

# Delete Schema
w.schemas.delete(name="hr", catalog_name="finance")
```

---

## 🎯 Example Sentence

* **"Inside the `finance` catalog, we created a `sales` schema to store tables like `transactions` and `customers`."**

---

✅ In short: A **Schema (Database)** is a **logical grouping inside a Catalog** that holds **tables, views, functions, and volumes**, making data easier to manage and secure.
