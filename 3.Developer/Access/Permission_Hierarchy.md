# 🟣 Unity Catalog Permission Hierarchy (Deep Dive)

---

## 🧠 What is Unity Catalog?

👉 **Unity Catalog** is the **central governance layer** in Databricks that manages:
- 🔐 Access control  
- 📜 Data lineage  
- 🏢 Metadata  

---

# 🏗️ Hierarchy Structure

```text
Metastore → Catalog → Schema → Table/View → Column
````

---

# 🔍 1. Metastore Level 🏢 (Top Level)

### 📌 What?

* Root container for all data assets

### 🔑 Permissions:

* `CREATE CATALOG`
* `CREATE EXTERNAL LOCATION`
* `CREATE STORAGE CREDENTIAL`

### 🎯 Example:

```sql
GRANT CREATE CATALOG ON METASTORE TO `data_admin`;
```

---

# 📂 2. Catalog Level 📚

### 📌 What?

* Logical grouping (like database group)

### 🔑 Permissions:

* `USE CATALOG`
* `CREATE SCHEMA`

### 🎯 Example:

```sql id="czm0hz"
GRANT USE CATALOG ON CATALOG finance TO `analyst`;
```

---

# 🗂️ 3. Schema Level 📁

### 📌 What?

* Similar to database

### 🔑 Permissions:

* `USE SCHEMA`
* `CREATE TABLE`
* `CREATE VIEW`

### 🎯 Example:

```sql id="z5k7tu"
GRANT USE SCHEMA ON SCHEMA finance.sales TO `analyst`;
```

---

# 📊 4. Table / View Level 📋

### 📌 What?

* Actual data objects

### 🔑 Permissions:

* `SELECT`
* `INSERT`
* `UPDATE`
* `DELETE`
* `MODIFY`

### 🎯 Example:

```sql id="bcs0qk"
GRANT SELECT ON TABLE finance.sales.transactions TO `analyst`;
```

---

# 📐 5. Column Level 🔍

### 📌 What?

* Fine-grained control

### 🎯 Example:

```sql id="j6ps1s"
SELECT 
  name,
  CASE WHEN is_member('admin') THEN salary ELSE NULL END AS salary
FROM employees;
```

---

# 🔗 Permission Flow (Inheritance)

```text
Metastore
   ↓
Catalog
   ↓
Schema
   ↓
Table
   ↓
Column
```

👉 ⚠️ Important:

* Permissions are **NOT fully inherited automatically**
* Must grant at each level

---

# 🔑 Common Privileges Explained

| Privilege      | Description          |
| -------------- | -------------------- |
| USE CATALOG    | Access catalog       |
| USE SCHEMA     | Access schema        |
| SELECT         | Read data            |
| MODIFY         | Insert/Update/Delete |
| CREATE TABLE   | Create tables        |
| ALL PRIVILEGES | Full control         |

---

# 🧠 Access Requirement Example

👉 To query a table:

You need:

* ✅ `USE CATALOG`
* ✅ `USE SCHEMA`
* ✅ `SELECT`

---

# ⚙️ Real Example (End-to-End Access)

```sql id="sh18lm"
GRANT USE CATALOG ON CATALOG finance TO `analyst`;

GRANT USE SCHEMA ON SCHEMA finance.sales TO `analyst`;

GRANT SELECT ON TABLE finance.sales.transactions TO `analyst`;
```

---

# 🔐 Advanced Controls

---

## 🔍 Row-Level Security

```sql id="hx8s1g"
CREATE VIEW secure_sales AS
SELECT * FROM sales
WHERE region = current_user();
```

---

## 📐 Column Masking

```sql id="i6lgf7"
SELECT 
  name,
  CASE 
    WHEN is_member('admin') THEN salary
    ELSE NULL
  END AS salary
FROM employees;
```

---

# 🧾 Auditing & Monitoring

* 📊 Track:

    * Who accessed data
    * What queries executed

```sql id="f6m1q0"
SHOW GRANTS ON TABLE finance.sales.transactions;
```

---

# 🏗️ Best Practices

* ✅ Use groups (not individual users)
* ✅ Follow least privilege
* ✅ Separate environments (dev/test/prod)
* ✅ Audit permissions regularly
* ✅ Use naming conventions

---

# 🚫 Common Mistakes

* ❌ Forgetting `USE CATALOG`
* ❌ Giving `ALL PRIVILEGES` unnecessarily
* ❌ Not using Unity Catalog
* ❌ Hardcoding access logic

---

# 🎯 Real-World Example

👉 Finance Team Access

| Role     | Access          |
| -------- | --------------- |
| Analyst  | SELECT only     |
| Engineer | MODIFY + CREATE |
| Admin    | ALL PRIVILEGES  |

---

# ⚡ Permission Flow Summary

```text
Grant → Validate → Access → Monitor → Audit
```

---

# 🏁 Final Takeaway

👉 Unity Catalog hierarchy ensures:

* 🔐 Fine-grained security
* 📊 Controlled access
* 🧭 Full governance

➡️ Always grant **minimum required permissions**

---

## 🚀 Want Next?

* 🔬 Hands-on lab (create catalog → schema → table → grant access)
* 🔐 Interview questions on Unity Catalog
* 📊 Real enterprise RBAC design
