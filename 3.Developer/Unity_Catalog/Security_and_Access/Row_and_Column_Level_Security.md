# 🔐 Row & Column Level Security in Unity Catalog

Unity Catalog (UC) allows **fine-grained access control** over data, not just at the object (table/view) level, but also at the **row** and **column** level.  
This ensures **sensitive information is protected** while still enabling wide data access for analytics.

---

## 1️⃣ Row-Level Security (RLS)

### 🔹 What is RLS?
- Restricts access to **specific rows** in a table based on user identity or group membership.  
- Implemented using **row filters** (security policies).

---

### 🛠️ Example: Row Filter Policy

```sql
-- Create a row filter policy
CREATE ROW FILTER POLICY sales_region_filter
AS (region STRING) -> region = current_user();

-- Apply policy to a table
ALTER TABLE finance.sales.transactions
ATTACH ROW FILTER sales_region_filter ON (region);
````

📌 In this example:

* Each user only sees rows where `region = current_user()`.
* If `alice@databricks.com` queries the table, she only sees rows with her region.

---

### ✅ Benefits

* Enforces **multi-tenant security** (different users see only their data).
* Prevents **data leaks** in shared environments.

---

## 2️⃣ Column-Level Security (CLS)

### 🔹 What is CLS?

* Restricts access to **specific columns** in a table.
* Sensitive fields (like SSNs, salaries, credit card numbers) can be hidden from non-privileged users.
* Implemented using **column masks** or **view-based masking**.

---

### 🛠️ Example: Column Masking Policy

```sql
-- Create a column masking policy
CREATE MASKING POLICY ssn_mask
AS (val STRING) -> CASE
    WHEN current_user() IN ('hr_admin@databricks.com') THEN val
    ELSE 'XXX-XX-XXXX'
END;

-- Apply policy to a table column
ALTER TABLE hr.employees
ALTER COLUMN ssn
SET MASKING POLICY ssn_mask;
```

📌 In this example:

* `hr_admin@databricks.com` can see full SSNs.
* Other users see masked values: `XXX-XX-XXXX`.

---

## 3️⃣ Using Views for Security

Sometimes, **secure views** are used as an additional mechanism:

```sql
-- Secure view for column masking
CREATE OR REPLACE VIEW hr.secure_employees AS
SELECT
    name,
    CASE 
        WHEN current_user() IN ('hr_admin@databricks.com') THEN ssn
        ELSE 'XXX-XX-XXXX'
    END AS ssn
FROM hr.employees;
```

---

## 4️⃣ Governance Best Practices

* 🔒 Apply **masking policies** on sensitive columns like PII, financial data.
* 👥 Use **groups**, not individuals, for managing policies.
* 🧩 Combine **row-level** and **column-level** security for maximum protection.
* 🛡️ Always log & audit policy usage via **event logs**.

---

## ✅ Summary

* **Row-Level Security (RLS)** → Controls **which rows** users can see.
* **Column-Level Security (CLS)** → Controls **which columns** users can access (or how they’re masked).
* Both use **policies** in Unity Catalog that can be attached to tables and views.
* Ensure compliance with **PII, GDPR, HIPAA, SOC2** by enforcing these policies.

---
