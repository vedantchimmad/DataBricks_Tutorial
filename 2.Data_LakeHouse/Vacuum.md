# 🧹 VACUUM Command in Delta Lake

The **`VACUUM`** command in Delta Lake is used for **cleaning up old, unused data files** that are no longer needed by the Delta Table.  
These files are usually left behind after **updates, deletes, merges, or overwrites**.  

---

## ✅ Purpose
- **Reclaim storage space** by removing obsolete files.  
- **Improve performance** by reducing the number of small files.  
- Ensures that **Delta transaction log (`_delta_log`)** and underlying data are consistent.  

---

## 📝 Syntax

```sql
VACUUM delta.`<path-to-delta-table>` RETAIN <hours>
````

or on a registered table:

```sql
VACUUM table_name RETAIN <hours>
```

---

## ⚙️ Parameters

| Parameter             | Description                                                                                    | Example                                                    |
| --------------------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| `table_name` / `path` | The Delta table to vacuum. Can be a registered table or a file path.                           | `VACUUM sales_data` or `VACUUM delta.`/mnt/delta/sales\`\` |
| `RETAIN <hours>`      | Optional. Minimum number of **hours to retain old data files** (default = 168 hours = 7 days). | `RETAIN 24` (keep last 24 hours of files)                  |

---

## 🔑 Important Notes

* **Default retention**: 7 days (168 hours).
* **Minimum retention**: 0 hours (⚠️ risky, might break time travel if files are deleted too soon).
* **Best practice**: Always retain at least 7 days to allow for **time travel & rollbacks**.

---

## 📌 Example 1: Vacuum by Table Name

```sql
-- Clean old files from a Delta table
VACUUM sales_data RETAIN 168;
```

---

## 📌 Example 2: Vacuum by Path

```sql
-- Clean files in the specified path
VACUUM delta.`/mnt/delta/sales` RETAIN 72;
```

---

## 📌 Example 3: Using Python

```python
# Vacuum Delta table using Spark SQL
spark.sql("VACUUM sales_data RETAIN 72")

# Or directly with DeltaTable API
from delta.tables import DeltaTable

delta_table = DeltaTable.forPath(spark, "/mnt/delta/sales")
delta_table.vacuum(72)  # retain last 72 hours of files
```

---

## 🔍 Why Not Use RETAIN 0?

```sql
VACUUM sales_data RETAIN 0;
```

* This will **delete all unreferenced files immediately**.
* ❌ Breaks **time travel** (can’t query older versions).
* Use only if you are **absolutely sure** you don’t need older versions.

---

## 📊 Workflow with VACUUM

1. Run **Delta operations** (INSERT, UPDATE, DELETE, MERGE).
2. Old files become **orphaned** (not tracked in `_delta_log`).
3. Run **VACUUM** to remove orphaned files permanently.
4. Delta table stays **optimized, compact, and clean**.

---

## 🚀 Summary

* `VACUUM` = **Garbage Collection** for Delta Lake tables.
* Default: **168 hours (7 days)**.
* Use with caution — setting low retention may break **time travel**.
* Available in **SQL, PySpark, and DeltaTable API**.

---

🧹 **Think of VACUUM as a cleanup crew** that ensures Delta tables don’t get cluttered with old, unused files.


