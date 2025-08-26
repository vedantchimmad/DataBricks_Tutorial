# 🗂️ Unity Catalog in Databricks  

---

## 🔹 Introduction  
**Unity Catalog** is Databricks’ **centralized data governance and security layer**.  
It manages **data access, lineage, and auditing** across all workspaces in a **Lakehouse**.  

👉 Think of Unity Catalog as the **single source of truth for all data assets** (tables, views, files, ML models, dashboards).  

---

## 🧩 Key Features of Unity Catalog  

- 🛡️ **Centralized Access Control** → Manage permissions for all data assets in one place.  
- 📂 **Data Discovery** → Search & explore datasets easily.  
- 🔗 **Lineage Tracking** → See how data flows from raw → curated → consumption.  
- 🧾 **Audit Logging** → Track who accessed what, when, and how.  
- 🔄 **Cross-Workspace Sharing** → Securely share data across teams, regions, or clouds.  
- ⚡ **Supports All Assets** → Tables, views, functions, ML models, dashboards, files.  
- 🔐 **Fine-Grained Permissions** → Control at schema, table, column, or row level.  

---

## 📖 Unity Catalog Hierarchy  

```

🌍 Metastore (One per Region / Org)
└── 🏢 Catalogs (Business Domains, e.g., Finance, HR)
└── 📚 Schemas (Logical group of tables, e.g., Sales, Payroll)
└── 📊 Tables & Views (Managed or External)

```

- **Metastore** → Top-level container for all data governance.  
- **Catalog** → Logical grouping of schemas (e.g., Finance, Marketing).  
- **Schema (Database)** → Group of tables & views.  
- **Tables/Views** → Actual datasets.  

---

## 🖼️ Unity Catalog Architecture  

```
              🛠️ Data Engineers     👩‍💼 Data Analysts    🤖 Data Scientists
                           │
                           ▼
                  🗂️ Unity Catalog (Governance Layer)
            ┌───────────────┬───────────────┬───────────────┐
            │ Access Control │ Lineage       │ Audit Logs     │
            └───────────────┴───────────────┴───────────────┘
                           │
      ┌───────────────┬───────────────┬───────────────┐
      │ Delta Lake     │ DBFS / Files  │ ML Models      │
      └───────────────┴───────────────┴───────────────┘
                           │
                           ▼
                  📊 BI Tools / ML / SQL Queries
```

---

## 🧑‍💻 Example Usage  

### 1️⃣ Creating a Catalog & Schema  
```sql
-- Create a catalog for Finance
CREATE CATALOG finance;

-- Create schema inside catalog
CREATE SCHEMA finance.revenue;
````

---

### 2️⃣ Creating a Table

```sql
-- Create a managed table in Unity Catalog
CREATE TABLE finance.revenue.sales_data (
    order_id STRING,
    customer_id STRING,
    amount DOUBLE,
    order_date DATE
);
```

---

### 3️⃣ Granting Permissions

```sql
-- Grant SELECT permission to analyst group
GRANT SELECT ON TABLE finance.revenue.sales_data TO `analyst_group`;

-- Grant usage on schema
GRANT USAGE ON SCHEMA finance.revenue TO `finance_team`;
```

---

### 4️⃣ Lineage & Discovery

* 🔍 Search datasets using the Catalog Explorer.
* 📈 Track how data is transformed across tables & notebooks.

---

## ✅ Benefits of Unity Catalog

* 🔒 **Enterprise-Grade Security** → Fine-grained control down to rows & columns.
* 📜 **Compliance** → Enables GDPR, HIPAA, SOX auditing.
* 🌐 **Multi-Cloud Governance** → Works across AWS, Azure, and GCP.
* 🔄 **Data Sharing** → Delta Sharing support for external partners.
* ⚡ **Productivity** → Easier collaboration across teams.

---

## 🌟 Example Use Case

1. **Finance team** creates a `finance` catalog with sales data.
2. **HR team** has its own `hr` catalog with employee data.
3. **Unity Catalog** enforces access → Finance team cannot see HR data.
4. **Auditor** can query logs to track who accessed sensitive payroll tables.
5. **Data Scientists** use the same governed datasets for ML model training.

---
