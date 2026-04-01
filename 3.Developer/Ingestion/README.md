# 📥 Types of Ingestion in Databricks

Databricks supports multiple data ingestion patterns depending on data source, latency requirements, and architecture.

---

## 📦 1. Batch Ingestion
- 📅 Data ingested at scheduled intervals
- 📊 Best for historical or large datasets
- ⚙️ Tools:
    - Databricks Jobs
    - Apache Spark batch processing
- 📁 Sources:
    - CSV, Parquet files (S3, ADLS, GCS)
    - Database exports

---

## ⚡ 2. Streaming Ingestion (Real-Time)
- 🔄 Continuous data ingestion
- ⏱️ Low-latency processing
- 🧠 Powered by Structured Streaming
- 🔌 Sources:
    - Kafka
    - Event Hubs
    - IoT devices

---

## 📂 3. Auto Loader (Incremental File Ingestion)
- 🔍 Detects new files automatically
- 📈 Highly scalable and efficient
- 🛡️ Fault-tolerant
- ⚙️ Modes:
    - Directory listing
    - File notification
- 🧩 Works seamlessly with Delta Lake

---

## 🔁 4. Delta Live Tables (DLT)
- 🧾 Declarative pipeline framework
- 🔄 Supports batch + streaming
- ✅ Built-in data quality checks
- 🤖 Automated pipeline management
- 🏭 Ideal for production pipelines

---

## 🔄 5. Change Data Capture (CDC)
- 🧬 Tracks incremental changes (Insert/Update/Delete)
- 🔗 Keeps systems in sync
- ⚙️ Methods:
    - APPLY CHANGES INTO
    - External tools (e.g., Debezium)

---

## 🌐 6. API-Based Ingestion
- 🔌 Pull data from REST APIs
- 🐍 Implemented via Python/Spark
- ☁️ Used for SaaS integrations

---

## 🤝 7. Partner Connect / ETL Tools
- 🔗 Integrates with external ETL platforms
- 🧰 Examples:
    - Fivetran
    - Informatica
    - Talend
- 🏢 Ideal for enterprise ingestion

---

## 📊 Summary Table

| 📌 Type               | ⚙️ Mode       | 🎯 Use Case                    |
|----------------------|--------------|-------------------------------|
| 📦 Batch             | Scheduled    | Historical data               |
| ⚡ Streaming         | Real-time    | Live dashboards               |
| 📂 Auto Loader       | Incremental  | File ingestion                |
| 🔁 DLT               | Both         | Managed pipelines             |
| 🔄 CDC               | Incremental  | Database sync                 |
| 🌐 API               | On-demand    | External data                 |
| 🤝 ETL Tools         | Varies       | Enterprise integrations       |
