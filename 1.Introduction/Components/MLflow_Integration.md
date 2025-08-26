# 🤖 MLflow Integration with Databricks  

---

## 🔹 Introduction  
**MLflow** is an **open-source machine learning (ML) lifecycle platform** integrated natively with Databricks.  
It helps manage the **end-to-end ML workflow**:  

- 📂 **Experiment Tracking** → Record & compare runs.  
- 📦 **Model Registry** → Centralized hub for managing models.  
- 🚀 **Deployment** → Deploy models to production seamlessly.  
- 🔄 **Reproducibility** → Track code, data, and environment for reproducible results.  

👉 With Databricks, MLflow is deeply integrated into **workspaces, notebooks, clusters, and Unity Catalog**.  

---

## 🧩 Key MLflow Components  

- 🧪 **Tracking** → Log metrics, parameters, artifacts, and models.  
- 🏷️ **Model Registry** → Manage versions, lifecycle stages (Staging, Production, Archived).  
- 🏗️ **Projects** → Package ML code for reproducibility.  
- ☁️ **Deployment** → Serve models with REST APIs, batch inference, or UDFs.  

---

## 🖼️ MLflow + Databricks Architecture  

```
                 👩‍💻 Data Scientists / ML Engineers
                                 │
                                 ▼
                            Databricks ML
                        (Notebooks, Jobs, Clusters)
                                 │
                                 ▼
                            🤖 MLflow
       ┌──────────────────────┬──────────────────────┐
       │ Tracking             │ Model Registry       │
       │ (params, metrics)    │ (versions, stages)   │
       └──────────────────────┴──────────────────────┘
                                 │
                                 ▼
                       🗂️ Unity Catalog (Governed Models)
                                 │
                 ┌───────────────┼───────────────┐
                 │               │               │
             🚀 Deployment   🔬 Experimentation   📊 BI/Apps
```

---

## 🧑‍💻 Example Usage  

### 1️⃣ Tracking Experiments  
```python
import mlflow
import mlflow.sklearn
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_iris

# Data
X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Model
model = RandomForestClassifier(n_estimators=50)

with mlflow.start_run():
    model.fit(X_train, y_train)
    preds = model.predict(X_test)

    acc = accuracy_score(y_test, preds)

    # Log parameters and metrics
    mlflow.log_param("n_estimators", 50)
    mlflow.log_metric("accuracy", acc)

    # Log model
    mlflow.sklearn.log_model(model, "random_forest_model")
````

---

### 2️⃣ Registering Model to Unity Catalog

```python
import mlflow

result = mlflow.register_model(
    "runs:/<RUN_ID>/random_forest_model",
    "main_catalog.ml_models.random_forest"
)
```

---

### 3️⃣ Managing Model Lifecycle

```python
from mlflow.tracking import MlflowClient

client = MlflowClient()

# Transition model to staging
client.transition_model_version_stage(
    name="main_catalog.ml_models.random_forest",
    version=1,
    stage="Staging"
)
```

---

### 4️⃣ Model Deployment in Databricks

* 🔄 **Batch Inference** → Run models on Delta tables.
* ⚡ **Real-Time Inference** → Serve models as REST APIs.
* 🧮 **UDF Inference** → Use ML models directly in SQL queries.

Example: Predict with SQL UDF

```sql
SELECT
  customer_id,
  predict_churn(features) AS churn_prediction
FROM customer_features;
```

---

## ✅ Benefits of MLflow on Databricks

* 📊 **Centralized Experiment Tracking** → No need for external tools.
* 🗂️ **Governed Model Registry** (Unity Catalog integrated).
* 🚀 **Seamless Deployment** → Batch, real-time, or SQL inference.
* 🔒 **Security** → Fine-grained access control for models.
* 🌐 **Collaboration** → Teams share models easily across workspaces.
* 🔄 **Reproducibility** → Track code, data, and environment for reliable experiments.

---

## 🌟 Example Use Case

1. Data Scientist trains churn prediction model in Databricks Notebook.
2. MLflow logs **parameters, metrics, and model artifacts** automatically.
3. Model is **registered in Unity Catalog** with version control.
4. ML Engineer promotes model from **Staging → Production**.
5. Business App calls REST API served model for **real-time predictions**.
6. Dashboards track model performance drift over time.

---