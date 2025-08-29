# 📓 dbutils.notebook in Databricks  

The **`dbutils.notebook`** utility in Databricks provides functions to **chain, run, and pass data between notebooks**.  
It helps you build **modular workflows**, where one notebook can call another and exchange results.  

---

## ⚙️ Common `dbutils.notebook` Commands  

### 1. **Run a Notebook**
Runs another notebook and optionally passes parameters.  

```python
result = dbutils.notebook.run("/Users/vedant/notebooks/child_notebook", 60, {"param1": "hello"})
````

* **path** → Notebook path in workspace
* **timeout** → Maximum time (in seconds) to wait
* **arguments** → Dictionary of key-value pairs (optional)

📌 Example:

```python
output = dbutils.notebook.run("/Repos/project/etl_step1", 300, {"date": "2025-08-26"})
print(output)
```

---

### 2. **Exit from a Notebook**

Ends execution of a notebook and returns a value to the caller.

```python
dbutils.notebook.exit("ETL Completed Successfully")
```

---

### 3. **Get Current Notebook Path**

Fetches the path of the notebook currently running.

```python
current_path = dbutils.notebook.entry_point.getDbutils().notebook().getContext().notebookPath().get()
print(current_path)
```

---

## 🛠 Example: Notebook Workflow

**📓 Parent Notebook**

```python
print("Running ETL Step 1...")
step1_result = dbutils.notebook.run("/ETL/step1", 120, {"input_path": "/mnt/raw/data"})

print("Step 1 completed with result:", step1_result)

print("Running ETL Step 2...")
step2_result = dbutils.notebook.run("/ETL/step2", 300, {"processed_path": step1_result})

print("Step 2 completed with result:", step2_result)
```

**📓 Child Notebook (step1)**

```python
dbutils.widgets.text("input_path", "")
input_path = dbutils.widgets.get("input_path")

# Processing logic
processed_data = input_path + "/processed"

# Return path
dbutils.notebook.exit(processed_data)
```

---

## 📊 Key Uses of `dbutils.notebook`

| Feature                | Description                             | Example                             |
| ---------------------- | --------------------------------------- | ----------------------------------- |
| **Run notebooks**      | Modularize workflows into smaller steps | `dbutils.notebook.run("child", 60)` |
| **Pass parameters**    | Send arguments to child notebooks       | `{"date": "2025-08-26"}`            |
| **Return values**      | Exit notebook with status/output        | `dbutils.notebook.exit("done")`     |
| **Chaining notebooks** | Build pipelines with multiple notebooks | ETL workflows                       |

---

## 🚀 Best Practices

* Modularize notebooks into **reusable units** (e.g., ETL steps).
* Use **parameters** to make notebooks dynamic.
* Always handle **timeout** to prevent infinite runs.
* Return meaningful outputs with `dbutils.notebook.exit()`.
* Combine with **Jobs & Workflows** for production scheduling.

---

✅ With `dbutils.notebook`, you can create **flexible, parameterized, and maintainable pipelines** inside Databricks.

