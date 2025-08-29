# 🎛️ dbutils.widgets in Databricks  

The **`dbutils.widgets`** utility lets you create **input controls** (widgets) at the top of a notebook.  
These widgets allow you to **pass parameters** dynamically into your notebook code, making notebooks interactive and reusable.  

---

## ⚙️ Types of Widgets  

| Widget Type | Command | Example |
|-------------|---------|---------|
| **Text**    | `dbutils.widgets.text()` | `dbutils.widgets.text("input_path", "/mnt/data")` |
| **Dropdown**| `dbutils.widgets.dropdown()` | `dbutils.widgets.dropdown("env", "dev", ["dev","test","prod"])` |
| **Combobox**| `dbutils.widgets.combobox()` | `dbutils.widgets.combobox("color", "red", ["red","blue","green"])` |
| **Multiselect** | `dbutils.widgets.multiselect()` | `dbutils.widgets.multiselect("days", "Mon", ["Mon","Tue","Wed"])` |

---

## 🛠 Commands  

### 1. **Create a Widget**
```python
dbutils.widgets.text("input_path", "/mnt/raw", "Input Data Path")
````

📌 Creates a text box named `input_path` with default value `/mnt/raw`.

---

### 2. **Get Widget Value**

```python
input_path = dbutils.widgets.get("input_path")
print("Data path is:", input_path)
```

---

### 3. **Remove a Widget**

```python
dbutils.widgets.remove("input_path")
```

---

### 4. **Remove All Widgets**

```python
dbutils.widgets.removeAll()
```

---

## 📊 Example: ETL Notebook with Widgets

```python
# Create widgets
dbutils.widgets.text("input_path", "/mnt/raw", "Raw Data Path")
dbutils.widgets.dropdown("env", "dev", ["dev","test","prod"], "Environment")

# Fetch widget values
input_path = dbutils.widgets.get("input_path")
env = dbutils.widgets.get("env")

print(f"Running ETL on path {input_path} in {env} environment")

# ETL Logic
df = spark.read.format("csv").option("header", "true").load(input_path)
df.show(5)

# Remove widgets after use
dbutils.widgets.removeAll()
```

---

## 🚀 Best Practices

* ✅ Use widgets to make **notebooks parameterized**.
* ✅ Useful when **chaining notebooks** with `dbutils.notebook.run()`.
* ✅ Helps in **interactive testing** (choose environment, dataset, date, etc.).
* ✅ Always **remove widgets** in production pipelines to avoid conflicts.

---

## 🔗 Widgets + Notebook Chaining

**Parent Notebook**

```python
result = dbutils.notebook.run("/ETL/child", 60, {"input_path": "/mnt/raw/data", "env": "prod"})
print(result)
```

**Child Notebook**

```python
# Create widgets for parameters
dbutils.widgets.text("input_path", "")
dbutils.widgets.text("env", "")

input_path = dbutils.widgets.get("input_path")
env = dbutils.widgets.get("env")

print(f"Processing {input_path} in {env} environment...")

dbutils.notebook.exit("Success")
```

---

✅ `dbutils.widgets` make Databricks notebooks **interactive, parameter-driven, and reusable**.