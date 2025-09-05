# What is PySpark?

**PySpark** is the **Python API for Apache Spark**, an open-source distributed computing framework.  
It allows developers to write **Spark applications using Python**, enabling large-scale data processing, machine learning, and real-time analytics.

---

## 🔹 Key Features of PySpark

| Feature | Icon | Description |
|---------|------|-------------|
| **Distributed Computing** | ⚡ | Runs computations in parallel across a cluster. |
| **Ease of Use** | 🐍 | Provides Pythonic APIs (RDD, DataFrame, SQL). |
| **Scalability** | 📈 | Handles petabytes of data across thousands of nodes. |
| **Unified Analytics** | 🔄 | Supports batch, streaming, ML, and graph processing. |
| **Integration** | 🌐 | Works with Hadoop, Hive, Kafka, Delta Lake, and cloud storage. |

---

## 🔹 PySpark Components

1. **RDD (Resilient Distributed Dataset)**  
   - Low-level API for distributed data.  
   - Immutable, fault-tolerant collections.  
   - Operations: `map()`, `filter()`, `reduceByKey()`.  

2. **DataFrame API**  
   - High-level API for structured data (like Pandas).  
   - Supports SQL-like queries.  
   - Optimized using **Catalyst optimizer**.  

3. **PySpark SQL**  
   - Run SQL queries on structured data.  
   - Integrates with Hive metastore.  

4. **PySpark MLlib**  
   - Machine learning library.  
   - Algorithms: classification, regression, clustering, recommendation.  

5. **PySpark Streaming / Structured Streaming**  
   - Process real-time streaming data.  
   - Sources: Kafka, socket, EventHub.  

6. **GraphX / GraphFrames**  
   - Graph processing for social networks, recommendations.  

---

## 🔹 Example: PySpark Workflow

```python
from pyspark.sql import SparkSession

# Create Spark session
spark = SparkSession.builder \
    .appName("PySparkExample") \
    .getOrCreate()

# Load data into DataFrame
df = spark.read.csv("employees.csv", header=True, inferSchema=True)

# Transform data
df_filtered = df.filter(df.salary > 50000)

# Run SQL query
df.createOrReplaceTempView("employees")
high_paid = spark.sql("SELECT name, salary FROM employees WHERE salary > 50000")

# Show result
high_paid.show()
````

---

## 🔹 PySpark vs. Pandas

| Aspect      | Pandas 🐼                                        | PySpark ⚡                                      |
| ----------- | ------------------------------------------------ | ---------------------------------------------- |
| Data Size   | Works on small/medium datasets (fits in memory). | Handles big data (distributed across cluster). |
| Execution   | Single machine.                                  | Distributed cluster.                           |
| Performance | Slower for very large datasets.                  | Optimized for large-scale processing.          |
| APIs        | Python only.                                     | Python API for Spark (Scala/Java backend).     |

---

## 🔹 Use Cases of PySpark

* 📊 **Big Data Analytics** → Processing petabytes of logs or transactions.
* ⚡ **Real-time Streaming** → Fraud detection, IoT monitoring.
* 🤖 **Machine Learning** → Training ML models at scale.
* 🏗️ **ETL Pipelines** → Data ingestion, cleaning, and transformation.
* 📈 **Business Intelligence** → Feeding data into BI tools (Power BI, Tableau).

---

## 🔹 Summary

* **PySpark** is the **Python API for Apache Spark**.
* Enables **distributed big data processing** using Python.
* Supports **batch, streaming, SQL, and ML**.
* Ideal for **large-scale analytics, data pipelines, and AI/ML workloads**.

✅ In short: **PySpark = Spark power + Python simplicity**.
