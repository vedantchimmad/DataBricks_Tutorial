# 📜 All Queries for Delta Lake Version Control (Time Travel)

Delta Lake provides **SQL & PySpark commands** to explore, query, and restore table versions.  
Here’s a complete set of queries you can use 🚀

---

## 🔍 1. View Table History
Get the full commit history of a Delta table.  

```sql
-- Show all operations performed on the table
DESCRIBE HISTORY sales_data;
````

Output includes: `version`, `timestamp`, `user`, `operation`, `operationParameters`, etc.

---

## ⏪ 2. Query Older Versions

### By Version Number

```sql
-- Get data as it was in version 3
SELECT * FROM sales_data VERSION AS OF 3;
```

### By Timestamp

```sql
-- Get data as it was at a given timestamp
SELECT * FROM sales_data TIMESTAMP AS OF '2025-08-20 10:00:00';
```

---

## 🐍 3. PySpark Examples

```python
# Load by version number
df_v3 = spark.read.format("delta") \
    .option("versionAsOf", 3) \
    .table("sales_data")

# Load by timestamp
df_old = spark.read.format("delta") \
    .option("timestampAsOf", "2025-08-20 10:00:00") \
    .table("sales_data")
```

---

## 🔄 4. Restore to Older Version

### SQL

```sql
-- Replace current table with version 3
CREATE OR REPLACE TABLE sales_data AS
SELECT * FROM sales_data VERSION AS OF 3;
```

### PySpark

```python
# Restore using version 3
df_old = spark.read.format("delta") \
    .option("versionAsOf", 3) \
    .table("sales_data")

df_old.write.format("delta").mode("overwrite").saveAsTable("sales_data")
```

### 🔁 What if you want to restore **different versions** on the **same existing table**?

Sometimes you may want to replace the **current table** with data from another version (say version 5) without creating a new table name.

* **SQL Approach**

```sql
-- Overwrite existing table with version 5 data
INSERT OVERWRITE TABLE sales_data
SELECT * FROM sales_data VERSION AS OF 5;
```

* **PySpark Approach**

```python
# Load version 5 and overwrite existing table
df_v5 = spark.read.format("delta") \
    .option("versionAsOf", 5) \
    .table("sales_data")

df_v5.write.format("delta").mode("overwrite").saveAsTable("sales_data")
```

✅ This ensures that the table `sales_data` keeps the **same name and permissions**, but its content is restored from the selected version.

---

## 🧹 5. Clean Up Old Versions

```sql
-- Remove data files older than 7 days
VACUUM sales_data RETAIN 168 HOURS;

-- Force vacuum (even if retention < 7 days)
VACUUM sales_data RETAIN 1 HOURS DRY RUN;
```

⚠️ After **VACUUM**, old versions may not be accessible.

---

## 🧾 6. Example: Auditing Changes

```sql
-- Show all DELETE operations in table history
SELECT * FROM (
  DESCRIBE HISTORY sales_data
) WHERE operation = 'DELETE';
```

```sql
-- Show who made changes and when
SELECT userId, operation, version, timestamp
FROM (DESCRIBE HISTORY sales_data);
```

---

## 🚀 Summary of Commands

| Task                      | SQL Command                                      | PySpark Equivalent                         |
| ------------------------- | ------------------------------------------------ | ------------------------------------------ |
| Show history              | `DESCRIBE HISTORY sales_data`                    | `spark.sql("DESCRIBE HISTORY sales_data")` |
| Query by version          | `SELECT * FROM tbl VERSION AS OF 3`              | `option("versionAsOf", 3)`                 |
| Query by timestamp        | `SELECT * FROM tbl TIMESTAMP AS OF '2025-08-20'` | `option("timestampAsOf", "...")`           |
| Restore old version       | `CREATE OR REPLACE TABLE ... VERSION AS OF ...`  | Write `.mode("overwrite")`                 |
| Restore different version | `INSERT OVERWRITE ... VERSION AS OF ...`         | `.mode("overwrite")` with chosen version   |
| Cleanup old versions      | `VACUUM tbl RETAIN 168 HOURS`                    | `spark.sql("VACUUM ...")`                  |

---

✅ With these queries, you can **audit, debug, rollback, restore, and reproduce** data pipelines using Delta Lake's version control.