# 🛡️ Authorization & Access Control in Unity Catalog

While **Identity & Authentication** verifies **who you are**,  
**Authorization & Access Control** ensures **what you can do** with data in **Unity Catalog (UC)**.

Unity Catalog uses a **Role-Based Access Control (RBAC)** model, combined with fine-grained **permissions** on objects (tables, schemas, catalogs, volumes, etc.).

---

## 🔑 Key Concepts

### 1. **Principals**
- Entities that can be granted permissions:
  - **Users** → Human identities (e.g., `alice@databricks.com`).
  - **Groups** → Collections of users (e.g., `finance_team`).
  - **Service Principals** → Applications / pipelines.

---

### 2. **Privilege Model**
UC supports **ANSI SQL-style GRANT/REVOKE** for managing access.

| Privilege | Description | Example |
|-----------|-------------|---------|
| `SELECT` | Read data from a table/view | `GRANT SELECT ON TABLE sales TO finance_team` |
| `MODIFY` | Insert, update, delete, merge data | `GRANT MODIFY ON TABLE sales TO etl_job` |
| `CREATE` | Create schemas, tables, views in container | `GRANT CREATE ON SCHEMA finance TO analyst_team` |
| `USAGE` | Allows access to a catalog/schema but not the data | `GRANT USAGE ON CATALOG finance TO marketing_team` |
| `OWNERSHIP` | Full control over an object (can GRANT to others) | `GRANT OWNERSHIP ON TABLE sales TO admin` |

---

### 3. **Access Control Layers**
Unity Catalog enforces security at **multiple levels**:

1. **Metastore level**
   - Only one per region.
   - Managed by **Metastore Admins**.
   - Controls catalogs across workspaces.

2. **Catalog level**
   - Controls which schemas a user can see.
   - Example: `finance`, `marketing`.

3. **Schema (Database) level**
   - Controls access to objects inside the schema.
   - Example: `finance.sales`.

4. **Table / View / Volume level**
   - Fine-grained control on reading/writing data.

---

### 4. **Row-Level Security (RLS)**
- Restricts data **rows** visible to users.
- Implemented with **dynamic views**.

```sql
-- Example: Restrict sales data to user's region
CREATE OR REPLACE VIEW regional_sales AS
SELECT * FROM sales
WHERE region = current_user_region();
````

---

### 5. **Column-Level Security (CLS)**

* Restricts access to **sensitive columns**.
* Common use cases: PII (emails, phone numbers), salary data.

```sql
-- Example: Mask sensitive column
CREATE OR REPLACE VIEW masked_sales AS
SELECT customer_id, 
       CASE WHEN is_member('analyst_team') THEN email ELSE 'REDACTED' END AS email,
       amount
FROM sales;
```

---

### 6. **Data Lineage**

* Tracks **who accessed what data** and **how it was transformed**.
* Helps with **compliance, debugging, and auditing**.

---

### 7. **Audit Logs**

* Every action (query run, permission granted, data read/write) is logged.
* Integrated with **cloud-native logging services**:

    * AWS CloudTrail
    * Azure Monitor
    * GCP Audit Logs

---

## ⚙️ Example: Granting Access in Unity Catalog

```sql
-- Grant SELECT privilege on a table
GRANT SELECT ON TABLE finance.sales.transactions TO `finance_team`;

-- Allow a group to create tables in a schema
GRANT CREATE ON SCHEMA finance.sales TO `etl_developers`;

-- Restrict ownership transfer
GRANT OWNERSHIP ON TABLE finance.sales.transactions TO `admin`;
```

---

## ✅ Summary

* UC uses **RBAC with ANSI SQL-style GRANT/REVOKE**.
* Access can be controlled at **Metastore → Catalog → Schema → Table/View/Volume** levels.
* Supports **Row-level Security** and **Column-level Security**.
* Provides **lineage and audit logs** for compliance.
* Enforces **fine-grained permissions** for both human and machine identities.

---