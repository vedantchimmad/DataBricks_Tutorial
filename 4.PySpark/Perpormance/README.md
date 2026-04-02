# 🚀 Performance Improvement Techniques (Databricks / Spark / SQL)

## ⚡ 1. Data Optimization

### 📂 Use Efficient File Formats
- ✅ Prefer **Delta Lake** / Parquet
- ❌ Avoid CSV/JSON for large datasets

### 🗂️ Partitioning
- Split data based on frequently filtered columns
```sql
PARTITION BY (date, region)
````

* ⚠️ Avoid high-cardinality columns (like IDs)

### 📦 Z-Ordering (Databricks Delta)

```sql
OPTIMIZE sales_table
ZORDER BY (customer_id, product_id);
```

* 🚀 Improves query pruning

---

## 🔄 2. Query Optimization

### 🎯 Select Only Required Columns

```sql
SELECT col1, col2 FROM table;  -- ✅ Good
SELECT * FROM table;           -- ❌ Avoid
```

### 🔍 Filter Early (Predicate Pushdown)

```sql
SELECT * FROM sales WHERE date >= '2025-01-01';
```

### 🧮 Avoid Repeated Calculations

* Use **CTE / temp view**

---

## 🔗 3. Join Optimization

### 🪶 Broadcast Join (Small Table)

```sql
SELECT /*+ BROADCAST(dim_table) */ *
FROM fact_table f
JOIN dim_table d ON f.id = d.id;
```

### 📊 Join Order Matters

* 🔹 Small → Big (best practice)

### 🚫 Avoid Skewed Joins

* Use salting or skew hints

---

## ⚙️ 4. Caching & Persistence

### 💾 Cache Frequently Used Data

```python
df.cache()
```

### 📌 Persist with Storage Level

```python
df.persist()
```

---

## 🔁 5. Shuffle Optimization

### 🔢 Reduce Shuffle Partitions

```python
spark.conf.set("spark.sql.shuffle.partitions", 200)
```

### ⚖️ Repartition vs Coalesce

```python
df.repartition(100)   # Increase partitions
df.coalesce(10)       # Reduce partitions
```

---

## 📊 6. Delta Lake Optimization

### 🧹 Optimize Files (Small File Problem)

```sql
OPTIMIZE table_name;
```

### 🗑️ Vacuum Old Files

```sql
VACUUM table_name RETAIN 168 HOURS;
```

---

## 🧠 7. Execution Plan Analysis

### 🔍 Check Query Plan

```sql
EXPLAIN SELECT * FROM table;
```

* Look for:

    * ❌ Full table scans
    * ❌ Large shuffles
    * ❌ Skew

---

## 🧵 8. Cluster & Resource Tuning

* ⚙️ Use **Autoscaling clusters**
* 🧠 Choose correct **instance type (memory vs compute)**
* 🔁 Enable **Photon engine** (Databricks)

---

## 📈 9. Indexing Alternatives

* 📌 Use:

    * Z-ORDER
    * Data skipping
    * Bloom filters (advanced)

---

## 🚫 Common Mistakes to Avoid

* ❌ Using `SELECT *`
* ❌ Too many small files
* ❌ No partitioning strategy
* ❌ Over-partitioning
* ❌ Ignoring skewed data

---

## 🎯 Quick Checklist

* ✅ Partition + Z-Order
* ✅ Optimize & Vacuum
* ✅ Broadcast small tables
* ✅ Cache reused data
* ✅ Tune shuffle partitions
* ✅ Use Delta format

---

