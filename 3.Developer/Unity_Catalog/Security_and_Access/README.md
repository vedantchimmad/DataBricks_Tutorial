# 🔐 Security & Access Control in Unity Catalog (UC)

Unity Catalog (UC) provides **fine-grained governance** for data and AI assets across **tables, views, volumes, ML models, and functions**.  
It ensures **data security, access control, and compliance** in a **Lakehouse environment**.

---

## 🛡️ Key Security Features

### 1. **Identity & Authentication**
- Integrates with **Identity Providers (IdPs)** like **Azure AD, Okta, AWS IAM, Google Cloud IAM**.
- Supports **Single Sign-On (SSO)** and **Multi-Factor Authentication (MFA)**.

---

### 2. **Authorization & Access Control**
- Implements **Role-Based Access Control (RBAC)**.
- Permissions are granted at different object levels:  
  - **Metastore → Catalog → Schema → Table/View/Volume**  
- Supports **Inheritance** (if you have access to a schema, you can query its tables unless explicitly revoked).

✅ **SQL Example – Grant Permissions**
```sql
-- Grant SELECT permission on a table
GRANT SELECT ON TABLE finance.sales.transactions TO `analyst_role`;

-- Grant ALL privileges on a schema
GRANT ALL PRIVILEGES ON SCHEMA finance.sales TO `data_engineer_role`;

-- Revoke permissions
REVOKE SELECT ON TABLE finance.sales.transactions FROM `analyst_role`;
````

---

### 3. **Object Ownership**

* Every UC object (catalog, schema, table, etc.) has an **owner**.
* Owners can **grant/revoke permissions** or transfer ownership.

```sql
-- Transfer ownership
ALTER TABLE finance.sales.transactions OWNER TO `senior_analyst_role`;
```

---

### 4. **Row & Column Level Security**

* Control access at **fine-grained levels**.
* **Column Masking** → Hide sensitive columns.
* **Row Filters** → Show only specific rows to certain users.

```sql
-- Create a view to enforce row-level security
CREATE VIEW finance.sales.secure_transactions AS
SELECT * FROM finance.sales.transactions
WHERE region = 'APAC';

GRANT SELECT ON VIEW finance.sales.secure_transactions TO `apac_analyst_role`;
```

---

### 5. **Data Lineage & Auditing**

* UC automatically **captures lineage**:
  → Which datasets feed into others
  → Who accessed or modified data
* Event logs provide **auditability** for compliance (GDPR, HIPAA, SOC 2).

---

### 6. **Token & Credential Management**

* Works with **Secrets Management** via Databricks Secrets + UC.
* External storage credentials (S3, ADLS, GCS) are secured in UC.

```sql
-- Create a storage credential
CREATE STORAGE CREDENTIAL s3_read_access
WITH AWS ROLE 'arn:aws:iam::1234567890:role/databricks-access';
```

---

### 7. **Privilege Types in UC**

| Privilege        | Description                                    |
| ---------------- | ---------------------------------------------- |
| `USAGE`          | Grants visibility of object, not data access   |
| `SELECT`         | Read data from tables/views                    |
| `MODIFY`         | Insert, update, delete rows                    |
| `CREATE`         | Create schemas, tables, views in parent object |
| `ALL PRIVILEGES` | Full control on object                         |
| `OWNERSHIP`      | Transfer/assign ownership                      |

---

### 8. **Secure Sharing**

* With **Delta Sharing**, UC enables **cross-organization data sharing** with fine-grained access control.

---

# 📌 Example Scenario

👩‍💼 **Finance Analyst**:

* Needs **read-only access** to sales data → `GRANT SELECT`.

👨‍💻 **Data Engineer**:

* Needs to **create and update tables** in the finance schema → `GRANT CREATE, MODIFY`.

👮 **Security Officer**:

* Needs to **audit access and manage ownership** → `GRANT ALL PRIVILEGES`.

---

# ✅ Summary

* **Authentication**: IdPs, SSO, MFA
* **Authorization**: RBAC, object-level privileges
* **Ownership**: Full control per object
* **Fine-grained Security**: Row/column masking, views
* **Auditing**: Lineage & event logs
* **Secure Access**: Secrets, credentials, Delta Sharing

---

⚡ In short:
Unity Catalog makes **data security enterprise-grade** by unifying governance, RBAC, auditing, and fine-grained access across **all Lakehouse assets**.
