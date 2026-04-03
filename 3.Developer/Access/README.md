# 🔐 Different Types of Access in Databricks

---

## 🧠 Overview

Databricks provides **multiple layers of access control** to secure data, compute, and workflows 🔒

👉 Governed mainly using:
- 🟣 Unity Catalog
- 🔑 IAM / Identity Providers
- ⚙️ Workspace permissions

---

# 🧩 Types of Access in Databricks

---

## 1️⃣ Workspace Access 🏢

### 📌 Controls:
- Notebooks 📓
- Jobs ⚙️
- Dashboards 📊

### 🔑 Permissions:
- 👀 View
- ✏️ Edit
- 🚀 Run

---

## 2️⃣ Cluster Access ⚙️

### 📌 Controls:
- Who can use clusters

### 🔑 Permissions:
- CAN ATTACH TO
- CAN RESTART
- CAN MANAGE

---

## 3️⃣ Table & Data Access (Unity Catalog) 📊

### 📌 Levels:
```text
Catalog → Schema → Table → Column
````

### 🔑 Example:

```sql
GRANT SELECT ON TABLE sales TO `analyst`;
```

---

## 4️⃣ Row-Level Access 🔍

### 📌 Restrict rows based on user

```sql
CREATE VIEW region_filter AS
SELECT * FROM sales
WHERE region = current_user();
```

---

## 5️⃣ Column-Level Access 📐

### 📌 Mask sensitive columns

```sql
SELECT 
  name,
  CASE WHEN is_member('admin') THEN salary ELSE NULL END AS salary
FROM employees;
```

---

## 6️⃣ File Access (Cloud Storage) 📂

### 📌 Controls access to:

* ADLS / S3 / GCS

### 🔑 Managed via:

* IAM roles
* Service principals

---

## 7️⃣ Job Access ⚙️

### 📌 Permissions:

* View 👀
* Run ▶️
* Manage ⚙️

---

## 8️⃣ SQL Warehouse Access 📊

### 📌 Controls:

* Query execution
* Dashboard access

### 🔑 Permissions:

* CAN USE
* CAN MANAGE

---

## 9️⃣ Secret Access 🔐

### 📌 Secure credentials

```python
dbutils.secrets.get(scope="scope", key="key")
```

---

## 🔟 Token-Based Access 🔑

### 📌 Used for:

* API access
* CI/CD pipelines

---

## 1️⃣1️⃣ Service Principal Access 🤖

### 📌 Used for:

* Automation
* CI/CD pipelines

---

## 1️⃣2️⃣ Delta Sharing Access 🔗

### 📌 External access

```sql
GRANT SELECT ON SHARE sales_share TO RECIPIENT partner;
```

---

## 1️⃣3️⃣ Lakehouse Federation Access 🌐

### 📌 Access external DBs

* Controlled via:

    * Connection permissions
    * Catalog access

---

# 🔐 Access Control Models

---

## 🟢 RBAC (Role-Based Access Control)

* Assign roles to users/groups

---

## 🔵 ABAC (Attribute-Based Access Control)

* Based on:

    * User attributes
    * Conditions

---

# 🏗️ Best Practices

* ✅ Use Unity Catalog for all data access
* ✅ Follow least privilege principle
* ✅ Use groups instead of users
* ✅ Separate dev/test/prod access
* ✅ Audit regularly

---

# 🚫 Common Mistakes

* ❌ Giving full access to everyone
* ❌ Not using Unity Catalog
* ❌ Hardcoding secrets
* ❌ Ignoring audit logs

---

# 🎯 Real-World Example

👉 Banking System

* 👨‍💼 Analyst → Read-only access
* 🧑‍💻 Engineer → Full table access
* 🔐 Admin → Manage permissions

---

# ⚡ Access Flow

```text
User → Authentication → Authorization → Resource Access
```

---

# 🏁 Final Takeaway

👉 Databricks access control =

* 🔐 Secure
* 🧩 Granular
* 📊 Governed

➡️ Powered by **Unity Catalog + IAM**

---

## 🚀 Want Next?

* 🔬 Unity Catalog permission hierarchy deep dive
* 🔐 Real-time RBAC setup example
* 📊 Interview questions on Databricks security
