# 🧪 PyTest Framework Setup in Databricks (Step-by-Step)

---

## 🧠 Goal

👉 Set up **PyTest** in Databricks to test:
- 🔄 Data transformations  
- 📊 ETL pipelines  
- ⚙️ Business logic  

---

# 🏗️ Architecture Overview

```text
Databricks Notebook / Code → PyTest → Test Cases → CI/CD
````

---

# 🔧 Step 1: Install PyTest

### 📌 Option 1: Notebook (Temporary)

```python
%pip install pytest
```

### 📌 Option 2: Cluster Library (Recommended)

* Go to **Cluster → Libraries**
* Install: `pytest`

---

# 📂 Step 2: Project Structure

```text
project/
│
├── src/
│   └── transformations.py
│
├── tests/
│   └── test_transformations.py
```

---

# 🧩 Step 3: Sample Transformation Code

```python
# src/transformations.py

def filter_positive(df):
    return df.filter("amount > 0")
```

---

# 🧪 Step 4: Write Test Cases

```python
# tests/test_transformations.py

import pytest
from pyspark.sql import SparkSession
from src.transformations import filter_positive

@pytest.fixture(scope="session")
def spark():
    return SparkSession.builder \
        .appName("pytest") \
        .getOrCreate()

def test_filter_positive(spark):
    data = [(1, 100), (2, -50)]
    df = spark.createDataFrame(data, ["id", "amount"])

    result = filter_positive(df)

    assert result.count() == 1
```

---

# ▶️ Step 5: Run Tests in Databricks

### 📌 From Notebook

```python
!pytest tests/
```

---

# ⚙️ Step 6: Use with Databricks Repos

* Connect GitHub / Azure DevOps repo
* Store code + tests together
* Run tests before deployment

---

# 🔄 Step 7: CI/CD Integration

👉 Add PyTest step in pipeline:

```yaml
- name: Run Tests
  run: pytest tests/
```

---

# 🧠 Advanced Testing Techniques

---

## 🔍 1. Schema Validation

```python
def test_schema(spark):
    df = spark.createDataFrame([(1, 100)], ["id", "amount"])
    assert "amount" in df.columns
```

---

## 📊 2. Data Validation

```python
def test_no_nulls(spark):
    df = spark.createDataFrame([(1, None)], ["id", "amount"])
    assert df.filter("amount IS NULL").count() == 1
```

---

## 🔄 3. Parameterized Testing

```python
@pytest.mark.parametrize("input_val,expected", [
    (100, True),
    (-10, False)
])
def test_values(input_val, expected):
    assert (input_val > 0) == expected
```

---

## ⚡ 4. Mocking Data

* Use small datasets instead of full data
* Improves speed

---

# 📊 Best Practices

* ✅ Keep tests small & fast
* ✅ Use sample/mock data
* ✅ Separate code & tests
* ✅ Run tests in CI/CD
* ✅ Test edge cases

---

# 🚫 Common Mistakes

* ❌ Using large datasets in tests
* ❌ Not using fixtures
* ❌ Testing only in notebooks
* ❌ Ignoring failures

---

# 🎯 Real-World Example

👉 ETL Pipeline

* Test transformation logic
* Validate schema
* Check data quality

---

# ⚡ Pro Tip

👉 Combine with:

* DLT expectations 🧪
* CI/CD pipelines 🔄
* Data quality tools 📊

---

# 🏁 Final Takeaway

👉 PyTest in Databricks =

* 🧪 Reliable pipelines
* ⚙️ Automated testing
* 🚀 Production-ready code

---

## 🚀 Want Next?

* 🔬 Mocking Spark DataFrames deeply
* ⚙️ PyTest + DLT integration
* 📊 Great Expectations vs PyTest comparison
