## BQML vs Vertex AI — Chapter 8 Cheat Sheet

### Decision Table
| Factor | BigQuery ML | Vertex AI |
|---|---|---|
| Team skillset | SQL only | Python/ML engineering |
| Data location | Already in BigQuery | Any source, any format |
| Data type | Tabular, time series | Tabular, image, text, audio, video |
| Model control | Limited to supported model_types | Full custom architecture (TF, PyTorch, XGBoost, etc.) |
| Compute | Managed by BQ slots | Custom GPU/TPU training jobs |
| Ops overhead | Very low (no data movement) | Higher, but full MLOps tooling available |

### BQML Supported model_type Values
- Regression/Classification: `LINEAR_REG`, `LOGISTIC_REG`, `BOOSTED_TREE_CLASSIFIER/REGRESSOR`, `RANDOM_FOREST_CLASSIFIER/REGRESSOR`, `DNN_CLASSIFIER/REGRESSOR`, `DNN_LINEAR_COMBINED_*`
- Unsupervised: `KMEANS`, `PCA`, `AUTOENCODER` (anomaly detection), `MATRIX_FACTORIZATION` (recommendations)
- Time series: `ARIMA_PLUS`, `ARIMA_PLUS_XREG`
- AutoML: `AUTOML_CLASSIFIER`, `AUTOML_REGRESSOR`
- Imported: `TENSORFLOW`, `TENSORFLOW_LITE`, `XGBOOST`, `ONNX`

### Key BQML SQL Functions
| Function | Purpose |
|---|---|
| `CREATE MODEL ... OPTIONS(...)` | Train model with SQL |
| `ML.PREDICT` | Generate predictions |
| `ML.EVALUATE` | Return precision/recall/RMSE/log loss |
| `ML.EXPLAIN_PREDICT` | Shapley-value feature attribution |
| `ML.FEATURE_IMPORTANCE` | Global feature importance (tree models) |
| `EXPORT MODEL` | Export to TF SavedModel/XGBoost/ONNX |
| `TRANSFORM` clause | Feature engineering, auto-reapplied at PREDICT |

### Vertex AI Components (MLOps)
| Component | Purpose |
|---|---|
| Vertex AI Training | Custom training jobs, AutoML training |
| Vertex AI Pipelines | DAG orchestration (Kubeflow-based), caching, lineage |
| Vertex AI Feature Store | Centralized feature reuse, online + offline serving, backed by BigQuery |
| Vertex ML Metadata | Lineage/reproducibility tracking |
| Vertex AI Model Registry | Versioned model repository, deployment source |
| Vertex AI Model Monitoring | Drift/skew detection post-deployment |

### Exam Gotchas
1. BQML and Vertex AI are **not mutually exclusive** — BQML hyperparameter tuning uses Vertex AI Vizier; BQML models can export to Vertex AI Model Registry.
2. TRANSFORM ≠ WHERE — TRANSFORM handles feature engineering reapplied automatically at prediction time.
3. Feature Store solves **cross-team feature consistency**, not orchestration (that's Pipelines) and not preprocessing (that's TRANSFORM).
4. ML.EVALUATE returns metrics; ML.PREDICT returns predictions — do not confuse the two on exam questions.
