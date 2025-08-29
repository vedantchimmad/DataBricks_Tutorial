# 📖 Key Terms & Explanations in **Databricks Unity Catalog**

Unity Catalog introduces a set of **governance and security terms** to organize, secure, and audit all data assets in the Lakehouse.  
Below is a glossary of the most important terms with explanations.

---

## 🏗️ **Core Structural Terms**

| Term | Explanation | Example |
|------|-------------|---------|
| **Metastore** | The top-level governance container in Unity Catalog. Holds metadata, permissions, and lineage. One per region. | `uc_metastore_us_east_1` |
| **Catalog** | A top-level container inside a Metastore. Used to organize schemas, tables, and other objects. | `main`, `finance`, `marketing` |
| **Schema (Database)** | Logical grouping of tables, views, and functions within a catalog. Similar to a database in SQL. | `main.sales` |
| **Table** | Structured dataset governed by UC. Can be **Managed** (data stored by Databricks) or **External** (data stored in cloud storage). | `main.sales.revenue` |
| **View** | Virtual table built on top of tables or queries. Governed by UC security. | `CREATE VIEW ...` |
| **Function (UDF)** | User-Defined Function (SQL, Python, etc.) governed by UC. Can be shared securely. | `CREATE FUNCTION my_mask()` |
| **Volume** | A container for unstructured/semi-structured data (CSV, JSON, images, PDFs, etc.). | `CREATE VOLUME main.raw_files` |

---

## 🔐 **Security & Access Control**

| Term | Explanation | Example |
|------|-------------|---------|
| **RBAC (Role-Based Access Control)** | Access control model used in UC. Permissions are granted to groups, roles, or users at Catalog, Schema, or Table levels. | `GRANT SELECT ON TABLE main.sales.revenue TO analyst_role` |
| **Row-Level Security** | Restrict access to rows in a table based on conditions. | Only see rows for `region = 'India'` |
| **Column-Level Security** | Restrict access to sensitive columns. | Hide SSN column from non-admins |
| **Masking Policy** | Policy applied to hide or obfuscate sensitive data values. | Masking SSN as `XXX-XX-1234` |
| **Row Filter Policy** | Restrict rows dynamically based on user identity. | Salesperson only sees their accounts |

---

## 📂 **Data Location & Storage**

| Term | Explanation | Example |
|------|-------------|---------|
| **External Location** | A pointer to a storage path in S3, ADLS, or GCS with governance applied by UC. | `s3://company-data/sales` |
| **Storage Credential** | Secure cloud access configuration (IAM role, Service Principal, etc.) that UC uses to access external locations. | `CREATE STORAGE CREDENTIAL s3_role` |
| **Managed Table** | Data stored and fully managed by Databricks. | `CREATE TABLE sales_data` |
| **External Table** | Data stored in external cloud storage, registered in UC but not physically stored in Databricks. | `CREATE TABLE sales_data LOCATION 's3://bucket/'` |

---

## 🔎 **Governance Features**

| Term | Explanation | Example |
|------|-------------|---------|
| **Data Lineage** | Tracks the full lifecycle of data (input → transformation → output → dashboards). Shown visually in Databricks UI. | View where `revenue` table data came from |
| **Audit Logs** | Track who accessed data, when, and how. Useful for compliance (GDPR, HIPAA, SOC2). | `SELECT * FROM system.access.audit_logs` |
| **Data Sharing (Delta Sharing)** | Securely share data with external users, orgs, or platforms without copying data. | Share sales data with a partner |

---

## 🤖 **ML & AI Governance**

| Term | Explanation | Example |
|------|-------------|---------|
| **Feature Store** | Managed repository for ML features, governed by UC. | `SELECT * FROM main.ml.features.customer_age` |
| **ML Models in UC** | MLflow models stored and governed within Unity Catalog. | `main.ml_models.churn_model` |

---

## 📌 **Namespace & Identifiers**

- **3-level namespace** → `catalog.schema.table`
  - Example: `main.sales.transactions`
- **Fully qualified object name** is required when multiple catalogs exist.
- Consistent naming ensures **clarity, governance, and isolation**.

---

## 📝 Quick Summary

- **Metastore** → Root governance unit.  
- **Catalog** → Top-level container (like a folder).  
- **Schema** → Logical grouping of data objects.  
- **Table/View/Function/Volume** → Data assets.  
- **RBAC, Row/Column Security, Policies** → Data protection.  
- **Lineage, Audit Logs** → Governance & compliance.  
- **External Locations & Storage Credentials** → Cloud storage integration.  
- **Delta Sharing** → Cross-organization secure data sharing.  

---

✅ Unity Catalog = **Organize + Govern + Secure + Audit + Share** data across the **Lakehouse**.  
