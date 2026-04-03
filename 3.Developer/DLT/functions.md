# 🚀 All Decorators & Functions in Databricks Lakeflow Declarative Pipeline (DLT / Lakeflow)

> 📌 Lakeflow Declarative Pipelines (LDP) use **DLT-style decorators + functions** to define pipelines in PySpark

---

# 🧩 1. Core Decorators

## 🟢 `@dlt.table`
Defines a **materialized table** in pipeline

```python
@dlt.table
def sales():
    return spark.read.table("source")
````

### ⚙️ Options:

```python
@dlt.table(
  name="silver_sales",
  comment="Cleaned sales data",
  table_properties={"quality": "silver"}
)
```

---

## 🔵 `@dlt.view`

Defines a **temporary logical view**

```python
@dlt.view
def temp_view():
    return spark.read.table("sales").filter("amount > 0")
```

---

## 🟣 `@dlt.expect`

Adds **data quality rule (warning only)**

```python
@dlt.expect("valid_amount", "amount > 0")
```

---

## 🔴 `@dlt.expect_or_drop`

Drops invalid records

```python
@dlt.expect_or_drop("valid_amount", "amount > 0")
```

---

## 🚫 `@dlt.expect_or_fail`

Fails pipeline if condition fails

```python
@dlt.expect_or_fail("valid_amount", "amount > 0")
```

---

## 🟡 `@dlt.append_flow`

Defines **append-only streaming flow**

```python
@dlt.append_flow
def append_data():
    return spark.readStream.table("source")
```

---

## 🔄 `@dlt.apply_changes`

Used for **CDC (Change Data Capture)**

```python
dlt.apply_changes(
  target = "target_table",
  source = "source_table",
  keys = ["id"],
  sequence_by = col("timestamp"),
  apply_as_deletes = expr("op = 'DELETE'")
)
```

---

# ⚙️ 2. Important Functions

---

## 📥 `dlt.read()`

Reads from another DLT table

```python
df = dlt.read("bronze_sales")
```

---

## 🔄 `dlt.read_stream()`

Reads streaming data

```python
df = dlt.read_stream("bronze_sales")
```

---

## 🧠 `dlt.apply_changes()`

Handles **merge/upsert logic (CDC)**

```python
dlt.apply_changes(
  target="silver_table",
  source="bronze_table",
  keys=["id"],
  sequence_by=col("updated_at")
)
```

---

## 📊 `dlt.create_target_table()`

Explicitly create target table

```python
dlt.create_target_table(
  name="target_table",
  comment="Final table"
)
```

---

## 🧹 `dlt.expect_all()`

Apply multiple expectations

```python
dlt.expect_all({
  "valid_amount": "amount > 0",
  "valid_id": "id IS NOT NULL"
})
```

---

## 🚫 `dlt.expect_all_or_drop()`

```python
dlt.expect_all_or_drop({
  "valid_amount": "amount > 0"
})
```

---

## 🔥 `dlt.expect_all_or_fail()`

```python
dlt.expect_all_or_fail({
  "valid_amount": "amount > 0"
})
```

---

# 🔄 3. Streaming Functions

---

## 📡 Auto Loader (Streaming Ingestion)

```python
spark.readStream.format("cloudFiles") \
  .option("cloudFiles.format", "json") \
  .load("/mnt/data")
```

---

## 🔁 Continuous Pipeline

* Automatically processes new data

---

# 🏗️ 4. Full Example Pipeline

```python
import dlt
from pyspark.sql.functions import col

# 🥉 Bronze
@dlt.table
def bronze():
    return spark.readStream.format("cloudFiles") \
        .option("cloudFiles.format", "json") \
        .load("/mnt/raw")

# 🥈 Silver
@dlt.table
@dlt.expect_or_drop("valid_amount", "amount > 0")
def silver():
    return dlt.read("bronze")

# 🥇 Gold
@dlt.table
def gold():
    return dlt.read("silver") \
        .groupBy("region") \
        .sum("amount")
```

---

# ⚠️ Important Notes

* 🔹 Works with **Delta Lake**
* 🔹 Integrated with **Unity Catalog**
* 🔹 Supports **batch + streaming**
* 🔹 Manages dependencies automatically

---

# 🚫 Common Mistakes

* ❌ Using `spark.read` instead of `dlt.read`
* ❌ No expectations (data quality missing)
* ❌ Mixing batch & streaming incorrectly
* ❌ Not using CDC for updates

---

# 🎯 Final Cheat Sheet

## ✅ Most Used

* `@dlt.table`
* `@dlt.view`
* `@dlt.expect_*`
* `dlt.read()`
* `dlt.apply_changes()`

---

## 🚀 Advanced

* `@dlt.append_flow`
* `dlt.expect_all_*`
* `dlt.create_target_table()`

---

# 🏁 Final Takeaway

👉 Lakeflow / DLT decorators help you:

* 📊 Build pipelines faster
* 🔄 Handle incremental loads
* 🧪 Ensure data quality
* 🚀 Scale automatically

---

## 🚀 Want Next?

* 🔬 Real-world CDC pipeline example
* ⚙️ Production deployment (CI/CD)
* 📊 Performance tuning in Lakeflow pipelines
