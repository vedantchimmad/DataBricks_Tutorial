# 🧩 Slowly Changing Dimensions (SCD) – All Types

**Slowly Changing Dimensions (SCDs)** are strategies used in **data warehouses and lakehouses** to manage how **dimension table attributes** change over time.  
They are essential for **CDC (Change Data Capture)** implementations.

---

## 📌 **SCD Types Overview**

| Type | Description | Historical Tracking | Complexity |
|------|-------------|---------------------|------------|
| **Type 0** | Fixed, no changes allowed | ❌ None | ⭐ Very Simple |
| **Type 1** | Overwrite with new values | ❌ None | ⭐ Simple |
| **Type 2** | Add new record + keep history | ✅ Full history | ⭐⭐⭐ Medium |
| **Type 3** | Keep current + one previous value | ✅ Limited | ⭐⭐ Moderate |
| **Type 4** | Separate history table | ✅ Full history (external table) | ⭐⭐⭐ Medium |
| **Type 5** | Mini-dimension + fact table linkage | ✅ Historical + optimized | ⭐⭐⭐⭐ Complex |
| **Type 6 (Hybrid)** | Combination of Type 1, 2, and 3 | ✅ Flexible | ⭐⭐⭐⭐ Complex |
| **Type 7** | Hybrid + unifying surrogate/natural keys | ✅ Both Type 1 & 2 queries | ⭐⭐⭐⭐⭐ Very Complex |

---

## 🔹 **SCD Type 0 – Fixed Dimension**
- No changes allowed, values remain constant forever.
- Useful for attributes that never change (e.g., Date of Birth, National ID).

✅ Pros: Simple  
⚠️ Cons: No flexibility

---

## 🔹 **SCD Type 1 – Overwrite**
- Old value is overwritten with the new one.
- No history of changes.

**Example:**
``` 
| Cust_ID | Name  | City    |
|---------|-------|---------|
| 101     | John  | London  |

John moves to Paris → record becomes:
| Cust_ID | Name  | City    |
|---------|-------|---------|
| 101     | John  | Paris   |
```
---

## 🔹 **SCD Type 2 – Full History**
- Keeps history by creating a **new record** for each change.
- Old record is marked inactive with `End_Date` or `Is_Current` flag.

**Example:**
```
| Cust_ID | Name  | City   | Start_Date | End_Date   | Current |
|---------|-------|--------|------------|------------|---------|
| 101     | John  | London | 2021-01-01 | 2023-03-01 | N       |
| 101     | John  | Paris  | 2023-03-02 | NULL       | Y       |
```
---

## 🔹 **SCD Type 3 – Limited History**
- Keeps **current and one previous value**.
- Adds extra columns like `Previous_City`.

**Example:**
```
| Cust_ID | Name  | Current_City | Previous_City |
|---------|-------|--------------|---------------|
| 101     | John  | Paris        | London        |
```

⚠️ Limited history → Only one previous change is tracked.

---

## 🔹 **SCD Type 4 – History Table**
- Current table holds latest values.  
- Separate **History Table** stores all past versions.

**Example:**

```
**Current Table**
| Cust_ID | Name  | City   |
|---------|-------|--------|
| 101     | John  | Paris  |

**History Table**
| Cust_ID | Name  | City   | Start_Date | End_Date   |
|---------|-------|--------|------------|------------|
| 101     | John  | London | 2021-01-01 | 2023-03-01 |

```
---

## 🔹 **SCD Type 5 – Mini-Dimension**
- Uses a **mini-dimension** table for rapidly changing attributes.  
- The **fact table** joins both current and historical dimensions.

✅ Optimized for large warehouses with frequently changing attributes.

---

## 🔹 **SCD Type 6 – Hybrid (1+2+3)**
- Combines **overwrite (Type 1)**, **history tracking (Type 2)**, and **limited history (Type 3)**.  
- Maintains `Current` and `Previous` columns along with full history records.

**Example:**
```
| Cust_ID | Name  | Current_City | Previous_City | Start_Date | End_Date   |
|---------|-------|--------------|---------------|------------|------------|
| 101     | John  | Paris        | London        | 2023-03-02 | NULL       |
```
---

## 🔹 **SCD Type 7 – Unifying Strategy**
- Extends **Type 6**, allowing queries using both **surrogate keys** (SCD2 history) and **natural keys** (SCD1 overwrite).
- Gives flexibility for **“latest state”** queries and **“historical tracking”** queries at the same time.

---

## 🔹 Summary Diagram – SCD Evolution
* Type 0 → No changes
* Type 1 → Overwrite
* Type 2 → Add new record (history)
* Type 3 → Store one previous value
* Type 4 → History table
* Type 5 → Mini-dimension
* Type 6 → Hybrid (1+2+3)
* Type 7 → Hybrid with surrogate + natural keys
---

## ✅ Key Takeaways
- **Type 1:** Latest value only  
- **Type 2:** Full history  
- **Type 3:** Limited history  
- **Type 4:** Separate history table  
- **Type 6/7:** Hybrid approaches for advanced analytics  

---