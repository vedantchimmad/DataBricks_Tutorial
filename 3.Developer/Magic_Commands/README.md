# ✨ Magic Commands in Databricks

In **Databricks Notebooks**, **magic commands** are special **percent-prefixed (`%`) commands** that let you control the notebook behavior, run different languages, manage data, and interact with the environment easily.  

They are **shortcuts** for developers and data engineers to work efficiently in Databricks.

---

## 🔑 Key Magic Commands

### 1️⃣ Language Switching Magics
Run code in different languages inside the same notebook cell.

| Command | Description | Example |
|---------|-------------|---------|
| `%python` | Run Python code (default in Python notebooks). | `%python print("Hello from Python!")` |
| `%sql` | Run SQL queries on tables registered in the metastore. | `%sql SELECT * FROM employees LIMIT 5;` |
| `%scala` | Run Scala code. | `%scala println("Hello from Scala!")` |
| `%r` | Run R code. | `%r summary(cars)` |
| `%sh` | Run shell commands. | `%sh ls -lh /dbfs/FileStore/` |

---

### 2️⃣ File System Magics (`%fs`)
Shortcut to interact with **DBFS (Databricks File System)**.

| Command | Description | Example |
|---------|-------------|---------|
| `%fs ls /` | List files in DBFS. | `%fs ls /databricks-datasets/` |
| `%fs mkdirs /mnt/new_dir` | Create a directory. | `%fs mkdirs /mnt/raw_data` |
| `%fs rm -r /mnt/raw_data` | Delete directory recursively. | `%fs rm -r /mnt/raw_data` |

---

### 3️⃣ Databricks Utilities Magics (`%dbutils`)
Run **Databricks utilities** directly.

| Command | Description | Example |
|---------|-------------|---------|
| `%fs` | File system operations. | `%fs ls /mnt/data` |
| `%dbutils.widgets.help()` | Manage input widgets in notebooks. | `%python dbutils.widgets.text("name", "default")` |
| `%dbutils.secrets.listScopes()` | Access secrets stored in Databricks. | `%python dbutils.secrets.listScopes()` |

---

### 4️⃣ Configuration Magics
Manage session-level or cluster-level configurations.

| Command | Description | Example |
|---------|-------------|---------|
| `%config` | Configure notebook environment. | `%config spark.sql.shuffle.partitions=10` |
| `%run` | Import another notebook into the current one. | `%run ./utils/data_preprocessing` |

---

### 5️⃣ Environment / Execution Magics
For debugging, timing, and displaying results.

| Command | Description | Example |
|---------|-------------|---------|
| `%time` | Measure execution time of a cell. | `%time spark.range(10**6).count()` |
| `%md` | Render cell as Markdown. | `%md # This is a Markdown cell 🚀` |
| `%pip` | Install Python packages at runtime. | `%pip install pandas==2.0.3` |
| `%conda` | Manage Conda environment. | `%conda install matplotlib` |

---

### 6️⃣ Display & Visualization Magics
Control how results are shown.

| Command | Description | Example |
|---------|-------------|---------|
| `%sql` + visualization | SQL query results can be turned into charts (bar, pie, scatter). | `%sql SELECT department, avg(salary) FROM employees GROUP BY department` |

---

## 🛠️ Combined Example

```sql
%sql
-- Run SQL query
SELECT region, SUM(sales) as total_sales
FROM finance.transactions
GROUP BY region
````

```python
%python
# Run Python on the same data
df = spark.sql("SELECT region, SUM(sales) as total_sales FROM finance.transactions GROUP BY region")
display(df)
```

```sh
%sh
# Verify files in DBFS
ls -lh /dbfs/mnt/finance/
```

```markdown
%md
# 📊 Sales Dashboard
This notebook combines **SQL + Python + DBFS + Visualization** in one flow!
```

---

## ✅ Summary

* **Magic commands** simplify running **multi-language code** in Databricks.
* You can:

    * Switch between **Python, SQL, R, Scala, and Shell**.
    * Use `%fs`, `%dbutils`, `%pip` for file, secret, and package management.
    * Render **Markdown**, measure **execution time**, and manage **configs**.

---

🚀 Magic commands = **Developer productivity superpower** in Databricks notebooks.

