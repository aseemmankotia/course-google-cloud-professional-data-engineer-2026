## Chapter 8: BigQuery ML vs Vertex AI for Data-Driven ML — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Which BQML function provides Shapley-value based explainability for predictions? | ML.EXPLAIN_PREDICT — returns per-feature attribution alongside predictions. Exam tip: ML.PREDICT alone does NOT return explanations. |
| 2 | BQML model types for time-series forecasting? | ARIMA_PLUS and ARIMA_PLUS_XREG. Auto-detects seasonality, holidays, and handles missing values. Exam tip: no manual decomposition required. |
| 3 | What does the TRANSFORM clause in CREATE MODEL do? | Defines feature engineering logic that is automatically reapplied at ML.PREDICT time. Exam tip: prevents train/serve skew without extra code. |
| 4 | BQML vs Vertex AI custom training — when to choose which? | BQML: SQL-only team, tabular data already in BQ, standard model types. Vertex AI custom training: custom architectures, GPUs/TPUs, image/text/audio data. |
| 5 | Purpose of Vertex AI Feature Store? | Centralized repository for feature reuse and consistency between training (offline) and serving (online); now built on BigQuery as storage layer. Exam tip: solves training/serving skew across teams. |
| 6 | What orchestrates ML workflows as a DAG with caching and lineage tracking? | Vertex AI Pipelines — built on Kubeflow Pipelines SDK, integrates with Vertex ML Metadata for lineage. Exam tip: preferred over generic Composer/Airflow for MLOps-specific reproducibility questions. |
| 7 | BQML CREATE MODEL syntax to set model type and target label? | CREATE OR REPLACE MODEL `ds.model` OPTIONS(model_type='LOGISTIC_REG', input_label_cols=['label']) AS SELECT ... |
| 8 | ML.EVALUATE vs ML.PREDICT — difference? | ML.EVALUATE returns metrics (precision, recall, log loss, RMSE, etc.) on an eval set. ML.PREDICT returns predicted labels/values on new data. |
| 9 | How does BQML perform hyperparameter tuning? | OPTIONS(num_trials=n, hparam_tuning_objectives=[...]) invokes Vertex AI Vizier under the hood. Exam tip: BQML and Vertex AI are integrated, not mutually exclusive. |
| 10 | What is the Vertex AI Model Registry used for? | Central versioned repository for models (trained in Vertex AI or imported, including BQML exports) used for tracking and deployment. |
| 11 | How do you get a BQML model out of BigQuery for external serving? | EXPORT MODEL statement — supports TensorFlow SavedModel, XGBoost booster, and ONNX formats. |
| 12 | Feature engineering hand-off: BQML TRANSFORM vs Vertex AI Feature Store? | TRANSFORM clause keeps feature logic in SQL for a single model. Feature Store centralizes features for reuse across multiple models/teams with consistent online + offline serving. |
