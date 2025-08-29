# 🗂️ Databricks Unity Catalog  

## 🌟 Introduction  
**Unity Catalog (UC)** is **Databricks’ unified governance solution** for data, AI, and ML.  
It provides a **centralized way** to manage access control, auditing, lineage, and compliance across **all data assets** stored in Databricks.  

Think of it as the **"single source of truth"** for managing data security and governance in the **Lakehouse**.

---

## 🔑 Key Features of Unity Catalog  

### 1️⃣ **Centralized Governance** 🛡️  
- Define and enforce **fine-grained access control** (table, column, row level).  
- Manage permissions once and apply everywhere (SQL, Python, R, Scala).  

### 2️⃣ **Data Lineage** 🔍  
- Track **end-to-end lineage** (datasets → tables → dashboards → ML models).  
- Helps with **impact analysis**, **audit compliance**, and **debugging**.  

### 3️⃣ **Secure Data Sharing** 🔄  
- Share data **safely** across clouds and accounts without duplication.  
- Powered by **Delta Sharing**.  

### 4️⃣ **Multi-Cloud Support** ☁️  
- Works seamlessly across **AWS, Azure, and GCP**.  
- Single governance layer for all environments.  

### 5️⃣ **Catalog & Schema Organization** 📚  
- Organizes data in a **3-level hierarchy**:  
```

Catalog → Schema → Table/View

```
- Example:  
```

SELECT \* FROM main.sales.revenue;

```
- `main` = Catalog  
- `sales` = Schema  
- `revenue` = Table  

### 6️⃣ **Fine-Grained Access Control** 🔑  
- Grant/revoke access at:  
- Catalog level  
- Schema level  
- Table level  
- Column level (column masking & row filters)  

### 7️⃣ **Audit & Compliance** 📜  
- Built-in **audit logs** for security compliance (HIPAA, GDPR, SOC2).  
- Ensures who accessed what data and when.  

---

## 🏗️ Unity Catalog Architecture  

```
+------------------------------------------------------------+
|                   🔒 Unity Catalog                         |
|                                                            |
|   Governance Layer (Security, Access Control, Lineage)     |
+------------------------------------------------------------+
|                |                  |
Catalogs          Schemas             Tables/Views
(e.g. main)       (e.g. sales)        (e.g. revenue)

````

- **Catalog** → Top-level container (like a database server).  
- **Schema** → Logical grouping of tables/views.  
- **Tables/Views** → Actual datasets managed under UC.  

---

## 🛠️ Example Commands  

### 🔑 Create Catalog  
```sql
CREATE CATALOG main;
````

### 📂 Create Schema

```sql
CREATE SCHEMA main.sales;
```

### 📊 Create Table

```sql
CREATE TABLE main.sales.revenue (
   id INT,
   region STRING,
   amount DOUBLE
);
```

### 🔒 Grant Access

```sql
GRANT SELECT ON TABLE main.sales.revenue TO `analyst_role`;
```

### 🔍 View Lineage

* In Databricks UI → **Data → Lineage**
* Shows end-to-end data flow.

---

## 📌 Benefits of Unity Catalog

* ✅ **Single governance model** across Databricks workloads.
* ✅ **Stronger security** (row/column-level controls).
* ✅ **Data discovery** with lineage and metadata.
* ✅ **Cross-cloud compatibility**.
* ✅ **Secure data sharing** with partners/customers.

---

## 🚀 Unity Catalog in Action

* **Data Engineers** → Manage pipelines & access rules.
* **Data Analysts** → Query curated data securely.
* **Data Scientists** → Train ML models with governed data.
* **Admins** → Audit access & ensure compliance.

---

👉 In short:
**Unity Catalog = Centralized Security + Lineage + Compliance + Sharing → One Governance Layer for the Lakehouse.**