# 🔍 Change Data Feed (CDF) in Databricks

## 📌 What is CDF?
**Change Data Feed (CDF)** in Databricks Delta Lake allows you to **track and read changes** (INSERT, UPDATE, DELETE) made to a Delta table.

👉 Instead of reading the full table, you can read **only the changes between versions**.

---

## 🧠 Why Use CDF?

- 🔄 Incremental data processing  
- ⚡ Faster downstream pipelines  
- 🧾 Audit & history tracking  
- 📡 Enable real-time data sync  

---

## 🔑 Types of Changes Captured

| 🔑 Change Type      | 📖 Description              |
|--------------------|---------------------------|
| insert             | New row added              |
| update_preimage    | Row before update          |
| update_postimage   | Row after update           |
| delete             | Row deleted                |

---

## ⚙️ Step 1: Enable CDF on Delta Table

```sql
ALTER TABLE customers 
SET TBLPROPERTIES (delta.enableChangeDataFeed = true);
````

---

## ⚙️ Step 2: Make Changes to Table

```sql
INSERT INTO customers VALUES (1, 'John');

UPDATE customers SET name = 'Johnny' WHERE id = 1;

DELETE FROM customers WHERE id = 1;
```

---

## ⚙️ Step 3: Read Change Data

### ✅ Using Version

```python
df = (spark.read
      .format("delta")
      .option("readChangeFeed", "true")
      .option("startingVersion", 0)
      .table("customers"))
```

---

### ✅ Using Timestamp

```python
df = (spark.read
      .format("delta")
      .option("readChangeFeed", "true")
      .option("startingTimestamp", "2024-01-01")
      .table("customers"))
```

---

## 📊 Sample Output

| id | name   | _change_type     | _commit_version | _commit_timestamp |
| -- | ------ | ---------------- | --------------- | ----------------- |
| 1  | John   | insert           | 1               | time1             |
| 1  | John   | update_preimage  | 2               | time2             |
| 1  | Johnny | update_postimage | 2               | time2             |
| 1  | Johnny | delete           | 3               | time3             |

---

## 🏗️ How CDF Works

```
Delta Table (CDF Enabled)
        ⬇️
Track Changes Internally
        ⬇️
Read Changes (Version / Time)
        ⬇️
Use in Downstream Systems
``` 

---

## 🔄 CDF vs CDC

| 🔑 Feature        | 🔍 CDF (Delta)           | 🔄 CDC (General)        |
|------------------|--------------------------|------------------------|
| Purpose          | Read changes             | Capture & apply changes|
| Scope            | Delta tables only        | Any database           |
| Setup            | Simple                   | Complex                |
| Use Case         | Analytics pipelines      | Data replication       |

---

## 📈 Real-World Use Case

### 🛒 E-commerce Analytics
- Orders table updated frequently  
- Use CDF to:
  - Capture new orders → insert  
  - Track status changes → update  
  - Handle cancellations → delete  

👉 Only changed data is sent to downstream dashboards

---

## ⚡ Streaming with CDF

```python
df = (spark.readStream
      .format("delta")
      .option("readChangeFeed", "true")
      .table("customers"))
````

---

## 💡 Best Practices

* ✅ Enable CDF only when needed (storage overhead)
* ✅ Use **startingVersion** for precise control
* ✅ Combine with **Auto Loader + CDC pipelines**
* ✅ Use for **Silver → Gold layer processing**
* ✅ Clean up old versions using VACUUM (carefully)

---

## 🧾 Summary

CDF in Databricks helps you:

* 🔍 Track all changes in Delta tables
* ⚡ Process only incremental data
* 📊 Build efficient downstream pipelines
* 🧾 Maintain audit and history

---

