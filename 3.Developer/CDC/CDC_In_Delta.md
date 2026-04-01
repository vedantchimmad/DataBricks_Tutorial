# 🔄 CDC on Delta Table in Databricks

## 📌 What is CDC on Delta Table?
CDC (Change Data Capture) on a **Delta Table** means applying **incremental changes (INSERT, UPDATE, DELETE)** directly to a Delta Lake table using its built-in capabilities like **MERGE** and **Change Data Feed (CDF)**.

---

## 🧠 Key Concepts

### 💾 1. Delta Table
- A storage format with **ACID transactions**
- Supports updates, deletes, and versioning

### 🔄 2. CDC Operations
- ➕ INSERT  
- ✏️ UPDATE  
- ❌ DELETE  

---

## ⚙️ Method 1: CDC using MERGE INTO (Most Common)

### 📥 Source (CDC Data)
| id | name   | operation |
|----|--------|----------|
| 1  | John   | INSERT   |
| 2  | Alice  | UPDATE   |
| 3  | Bob    | DELETE   |

---

### ✅ PySpark Example

```python
from delta.tables import DeltaTable

target = DeltaTable.forName(spark, "customers")

source_df = spark.read.format("delta").load("/mnt/cdc-data")

(target.alias("t")
 .merge(source_df.alias("s"), "t.id = s.id")
 .whenMatchedUpdate(condition="s.operation = 'UPDATE'", set={"name": "s.name"})
 .whenMatchedDelete(condition="s.operation = 'DELETE'")
 .whenNotMatchedInsert(condition="s.operation = 'INSERT'", values={"id": "s.id", "name": "s.name"})
 .execute())
````

---

### 💡 Explanation

* Matches records using `id`
* Applies:

    * UPDATE → modifies existing rows
    * DELETE → removes rows
    * INSERT → adds new rows

---

## ⚙️ Method 2: Using SQL MERGE

```sql
MERGE INTO customers t
USING cdc_source s
ON t.id = s.id
WHEN MATCHED AND s.operation = 'UPDATE' THEN
  UPDATE SET t.name = s.name
WHEN MATCHED AND s.operation = 'DELETE' THEN
  DELETE
WHEN NOT MATCHED AND s.operation = 'INSERT' THEN
  INSERT (id, name) VALUES (s.id, s.name);
```

---

## 🔍 Method 3: Change Data Feed (CDF)

### 📌 What is CDF?

* Tracks changes in a Delta table
* Allows downstream systems to read changes

---

### ✅ Enable CDF

```sql
ALTER TABLE customers SET TBLPROPERTIES (delta.enableChangeDataFeed = true);
```

---

### ✅ Read Changes

```python
df = (spark.read
      .format("delta")
      .option("readChangeFeed", "true")
      .option("startingVersion", 0)
      .table("customers"))
```

---

### 📊 Output Includes

* `_change_type` → insert/update/delete
* `_commit_version`
* `_commit_timestamp`

---

## 🔁 CDC Flow with Delta Table

```
CDC Source Data
     ⬇️
MERGE INTO Delta Table
     ⬇️
Delta Table Updated (ACID)
     ⬇️
(Optional) Read Changes via CDF
``` 

---

## 📈 Real-World Example

### 🛒 E-commerce Orders Table

- New orders → INSERT  
- Order status updated → UPDATE  
- Cancelled orders → DELETE  

👉 Use MERGE to apply changes into `orders` Delta table

---

## ⚖️ MERGE vs CDF

| 🔑 Feature        | 🔄 MERGE INTO            | 🔍 Change Data Feed       |
|------------------|--------------------------|--------------------------|
| Purpose          | Apply changes            | Track changes            |
| Usage            | Write operation          | Read operation           |
| Real-time        | Yes                      | Yes                      |
| Complexity       | Medium                   | Low                      |

---

## 💡 Best Practices

- ✅ Always define **primary key** for matching  
- ✅ Use **MERGE** for applying CDC  
- ✅ Enable **CDF** for auditing and downstream pipelines  
- ✅ Store raw CDC data in **Bronze layer**  
- ✅ Optimize Delta tables for performance (Z-ORDER, OPTIMIZE)

---

## 🧾 Summary

CDC on Delta Tables allows you to:
- 🔄 Efficiently apply incremental changes  
- 🛡️ Maintain ACID compliance  
- 🔍 Track changes using CDF  
- ⚡ Build scalable real-time pipelines  

---
