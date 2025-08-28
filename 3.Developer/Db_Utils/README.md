 # 🛠️ `dbutils` in Databricks  

`dbutils` is a **Databricks Utilities API** that helps you **interact with Databricks File System (DBFS), secrets, notebooks, widgets, and more**.  
It’s like a toolbox for developers and data engineers to manage workflows inside Databricks Notebooks.  

---

## 🔹 Major Components of `dbutils`

### 1. 📂 **File System (fs)**  
Used to interact with **DBFS (Databricks File System)**.  

```python
# List files in a directory
dbutils.fs.ls("/mnt/datalake/raw")

# Create a directory
dbutils.fs.mkdirs("/mnt/datalake/processed")

# Copy files
dbutils.fs.cp("dbfs:/mnt/raw/data.csv", "dbfs:/mnt/processed/data.csv")

# Remove files
dbutils.fs.rm("dbfs:/mnt/processed/data.csv", recurse=True)
````

---

### 2. 🔑 **Secrets**

Securely store and access **credentials, tokens, and keys**.

```python
# Get a secret value
token = dbutils.secrets.get(scope="my-scope", key="my-key")

# List available secret scopes
dbutils.secrets.listScopes()

# List secrets within a scope
dbutils.secrets.list("my-scope")
```

✅ Prevents hardcoding passwords or API keys inside notebooks.

---

### 3. 📒 **Notebooks**

Run other notebooks from within your current notebook.

```python
# Run another notebook
result = dbutils.notebook.run("/Shared/ETL_Job", 60, {"input_path": "/mnt/raw"})

# Exit a notebook with a value
dbutils.notebook.exit("ETL Completed Successfully")
```

✅ Helps modularize code into reusable notebooks.

---

### 4. 🎛️ **Widgets**

Create **input forms** in notebooks for parameterization.

```python
# Create widgets
dbutils.widgets.text("input_path", "/mnt/raw", "Input Path")
dbutils.widgets.dropdown("env", "dev", ["dev", "test", "prod"], "Environment")

# Get widget value
env = dbutils.widgets.get("env")
print(f"Running in {env} environment")

# Remove widget
dbutils.widgets.remove("env")
```

✅ Useful in production workflows and job scheduling.

---

### 5. 📜 **Jobs (Job Utilities)**

Used inside jobs to pass values between tasks.

```python
# Exit with a return value
dbutils.notebook.exit("Job Completed")
```

---

### 6. 📑 **Library Utilities**

Install and manage Python libraries dynamically.

```python
# Install a library
dbutils.library.installPyPI("requests", version="2.28.0")

# Restart Python process (required for some installs)
dbutils.library.restartPython()
```

⚠️ Note: In newer runtimes, prefer `%pip` or `%conda` instead of `dbutils.library`.

---

### 7. 📊 **Data Utilities (Experimental)**

Helpers for data exploration.

```python
# Show DataFrame details
dbutils.data.summarize(df)
```

---

## 🔎 Example: End-to-End Use Case

```python
# Step 1: Read parameter
input_path = dbutils.widgets.get("input_path")

# Step 2: Read data
df = spark.read.csv(input_path, header=True)

# Step 3: Process data
df_clean = df.dropna()

# Step 4: Save processed data
output_path = "dbfs:/mnt/processed/clean_data"
df_clean.write.mode("overwrite").parquet(output_path)

# Step 5: Exit notebook with message
dbutils.notebook.exit(f"Data saved at {output_path}")
```

---

## 📌 Summary of `dbutils`

| Module             | Purpose                             |
| ------------------ | ----------------------------------- |
| `dbutils.fs`       | Manage files & directories in DBFS  |
| `dbutils.secrets`  | Access secrets securely             |
| `dbutils.notebook` | Run/exit notebooks programmatically |
| `dbutils.widgets`  | Parameterize notebooks with inputs  |
| `dbutils.library`  | Install/manage libraries            |
| `dbutils.data`     | Explore data (experimental)         |

---