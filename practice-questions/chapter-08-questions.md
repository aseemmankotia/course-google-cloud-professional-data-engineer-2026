## Chapter 8: BigQuery ML vs Vertex AI for Data-Driven ML — Practice Questions

**Q1.** A data analytics team with strong SQL skills but no Python/ML engineering experience needs to build a customer churn classifier using data that already resides in BigQuery. The model must be retrained monthly with minimal operational overhead. Which approach BEST meets these requirements?

A) Export data to Vertex AI Workbench and build a custom TensorFlow model
B) Use Vertex AI AutoML Tables trained on exported CSV files
C) Build a custom PyTorch model deployed on Vertex AI Prediction
D) Use BigQuery ML to create a boosted tree classifier directly in BigQuery

<details>
<summary>Answer</summary>

**Correct: D**

SQL-only skillset, data already in BigQuery, and a standard tabular classification task make BQML CREATE MODEL the direct, low-ops fit with no data movement.

**Why the others are wrong:**
- Requires Python/TensorFlow expertise the team lacks and adds data movement plus ops overhead
- Requires unnecessarily exporting data out of BigQuery, adding complexity/cost versus staying in-warehouse
- Highest complexity option; unnecessary for a standard tabular churn model

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A team needs to train a custom convolutional neural network on millions of proprietary satellite images using GPUs, with full control over model architecture and hyperparameters. Which service should they use?

A) BigQuery ML DNN_CLASSIFIER
B) BigQuery ML TENSORFLOW model import
C) Vertex AI custom training job with GPU accelerators
D) Vertex AI AutoML Vision

<details>
<summary>Answer</summary>

**Correct: C**

Custom architecture control, GPU-accelerated training, and image data best fit a Vertex AI custom training job.

**Why the others are wrong:**
- DNN_CLASSIFIER in BQML is a simple feed-forward network for tabular data, not CNNs on images
- Importing a pre-trained TF model does not allow training a new custom CNN from scratch
- AutoML Vision does not give the full architecture/hyperparameter control required

</details>

---

**Q3.** Multiple data science teams at a company are independently engineering the same customer features (e.g., 30-day purchase count) for different models, causing inconsistent values between training and online serving. Which service addresses this?

A) Vertex AI Feature Store
B) BigQuery scheduled queries
C) BigQuery ML TRANSFORM clause
D) Vertex AI Pipelines

<details>
<summary>Answer</summary>

**Correct: A**

Vertex AI Feature Store centralizes feature definitions and values so multiple teams reuse consistent features across offline training and online serving.

**Why the others are wrong:**
- Scheduled queries only automate SQL jobs; they don't solve cross-team feature reuse or consistency
- TRANSFORM only applies feature logic within a single BQML model, not shared across teams/models
- Pipelines orchestrates workflows but does not store or serve features

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which BigQuery ML SQL function returns performance metrics such as precision, recall, and log loss for a trained model?

A) ML.PREDICT
B) ML.EXPLAIN_PREDICT
C) ML.EVALUATE
D) ML.FEATURE_IMPORTANCE

<details>
<summary>Answer</summary>

**Correct: C**

ML.EVALUATE computes evaluation metrics against a holdout or evaluation set.

**Why the others are wrong:**
- ML.PREDICT returns predicted labels/values, not evaluation metrics
- ML.EXPLAIN_PREDICT returns per-prediction feature attributions (Shapley values), not aggregate metrics
- ML.FEATURE_IMPORTANCE returns global feature importance for tree-based models, not evaluation metrics

</details>

---

**Q5.** A data engineering team needs a reproducible, orchestrated ML workflow that runs feature extraction, training, evaluation, and conditional deployment steps as a DAG, with lineage tracking across runs. Which service should be used?

A) Cloud Composer with Airflow DAGs only
B) BigQuery scheduled queries
C) Cloud Functions triggered by Pub/Sub
D) Vertex AI Pipelines

<details>
<summary>Answer</summary>

**Correct: D**

Vertex AI Pipelines is the purpose-built Kubeflow-based ML orchestrator with step caching and lineage/metadata tracking via Vertex ML Metadata.

**Why the others are wrong:**
- Composer/Airflow can orchestrate generic tasks but lacks native ML lineage and step-caching built into Vertex AI Pipelines
- Scheduled queries cannot run a conditional multi-step ML workflow across services
- Cloud Functions are event-driven single tasks, not full pipeline orchestration

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** In the statement CREATE OR REPLACE MODEL `ds.model1` OPTIONS(model_type='LOGISTIC_REG', ______) AS SELECT * FROM `ds.table1`, which OPTIONS argument is REQUIRED to specify the target column?

A) label_col=['churn']
B) input_label_cols=['churn']
C) target_column='churn'
D) output_col='churn'

<details>
<summary>Answer</summary>

**Correct: B**

input_label_cols is the correct BQML OPTIONS argument name and accepts an array of column names.

**Why the others are wrong:**
- Not a valid BQML OPTIONS argument name
- Not valid BQML syntax
- Not valid BQML syntax

</details>

---

**Q7.** What is the primary purpose of the TRANSFORM clause in a BQML CREATE MODEL statement?

A) It filters training rows below a quality threshold
B) It sets the hyperparameter tuning search space
C) It defines feature engineering logic that is automatically reapplied during ML.PREDICT to prevent train/serve skew
D) It exports the trained model to Vertex AI Model Registry

<details>
<summary>Answer</summary>

**Correct: C**

TRANSFORM captures preprocessing logic once and BQML automatically reapplies it at prediction time, eliminating train/serve skew.

**Why the others are wrong:**
- Row filtering belongs in a WHERE clause, not TRANSFORM
- Hyperparameter tuning search space is set via HPARAM_* OPTIONS, not TRANSFORM
- Model export uses the EXPORT MODEL statement, not TRANSFORM

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which statement BEST describes the fundamental difference between BigQuery ML and Vertex AI custom training?

A) BigQuery ML only supports unsupervised learning while Vertex AI only supports supervised learning
B) Vertex AI is only for AutoML while BigQuery ML is only for custom code models
C) BigQuery ML trains and serves models using SQL directly on data in BigQuery, while Vertex AI provides a broader ML platform supporting custom code, any framework, and non-tabular data
D) BigQuery ML and Vertex AI are mutually exclusive and cannot be used together

<details>
<summary>Answer</summary>

**Correct: C**

This accurately captures the core distinction: BQML is in-warehouse and SQL-driven for supported model types, while Vertex AI is a full ML platform for custom code and any data type.

**Why the others are wrong:**
- False — BQML supports both supervised and unsupervised model types
- Reversed/incorrect — Vertex AI supports both AutoML and custom training; BQML does not support arbitrary custom-code training from scratch
- False — BQML models can be exported to Vertex AI, and BQML can invoke Vertex AI AutoML/hyperparameter tuning under the hood

</details>

---

