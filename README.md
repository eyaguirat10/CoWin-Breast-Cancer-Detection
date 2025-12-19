#🎀 WeWomen — AI-Powered Breast Cancer Detection, Clustering & Recommendations 🎗️

Web platform for breast cancer decision support: **diagnosis prediction (DSO1)**, **metabolic risk clustering (DSO2)**, and **personalized recommendations (DSO3)**, with separate experiences for **doctors** and **patients**.

---

## Product Overview
- **Doctor portal**
  - Patient list & risk overview
  - **Breast cancer prediction (DSO1)** from clinical features
  - **Profile assignment / metabolic cluster (DSO2)**
  - Patient history (stored predictions + assigned profile)
  - Personalized recommendations & risk context for the patients
- **Patient portal**
  - Health dashboard
  - Personalized recommendations & risk context
  - Patient education center (articles, videos, downloadable resources)
  - Appointment booking flow

---

## System Architecture
- **Frontend (HTML/CSS/JavaScript)**
  - Authentication (register/login)
  - Patient dashboard (biomarkers input, severity view, recommendations)
  - Education center (articles/videos/resources)
  - Appointment booking flow
  - Doctor dashboard: patient list + actions (predict / assign profile / view history)

- **Backend API (FastAPI)**
  - Authentication + role-based access (**Doctor / Patient**)
  - REST endpoints:
    - **DSO1**: prediction endpoints (benign/malignant classification)
    - **DSO2**: clustering/profile assignment endpoints (K-Means metabolic profiles)
    - **DSO3**: recommendation endpoints (rules + severity scoring + explanations)
  - Persistence layer:
    - Users, patients, biomarker records, predictions, assigned profiles, history logs
  - Validation & serialization:
    - Pydantic schemas for request/response models

- **ML Layer**
  - Preprocessing pipeline:
    - cleaning, normalization/scaling, feature preparation
  - Model artifacts:
    - trained model files (e.g., XGBoost/MLP/SVM, K-Means) loaded by the FastAPI service
  - Evaluation utilities:
    - metrics (Accuracy, Precision/Recall/F1), ROC/AUC, confusion matrix
    - explainability outputs (feature importance / interpretation layer)

---

## CRISP-DM Methodology (Project Structure)

### 1) Business Understanding
- **Problem:** support early decision-making for breast cancer risk using patient biomarkers and clinical features.
- **Users & roles:** **Doctors** (clinical decision support + patient follow-up) and **Patients** (education + personalized guidance).
- **Objectives (DSO):**
  - **DSO1:** Predict **Benign vs Malignant** from diagnostic features (WDBC).
  - **DSO2:** Identify **metabolic risk profiles** using clustering (Coimbra).
  - **DSO3:** Deliver **personalized recommendations** using a Hybrid (Content-Based + Knowledge-Based) approach with a rewrite/explanation layer.

### 2) Data Understanding
- **Datasets**
  - **WDBC** (classification): diagnostic tumor features + target label.
  - **Coimbra** (clustering/recommendations): metabolic biomarkers (Age, BMI, Glucose, Insulin, HOMA, Leptin, Adiponectin, Resistin, MCP-1).
- **Exploration & quality checks**
  - Descriptive statistics, missing values checks, distribution inspection, correlation analysis.
  - Identification of class balance (DSO1) and feature scale differences (DSO2/DSO3).

### 3) Data Preparation
- **Cleaning**
  - Handling missing/invalid values (when present), outlier sanity checks.
- **Transformation**
  - Feature scaling/normalization (critical for SVM/KNN/MLP and clustering).
  - Train/test split for DSO1 and proper separation of evaluation data.
- **Feature engineering (DSO3)**
  - Build **healthy reference baseline** (mean/std/percentiles).
  - Compute **Z-scores** per biomarker and map to severity buckets (Normal/Mild/Moderate/Severe).
  - Create patient deviation profiles (per-biomarker abnormality vector).

### 4) Modeling
- **DSO1 (Supervised classification)**
  - Models tested: Linear baseline, Softmax Regression, KNN, SVM variants, MLP, Random Forest, XGBoost, GRU-SVM hybrid.
  - Training pipeline: preprocessing → model fitting → inference wrapper.
- **DSO2 (Unsupervised clustering)**
  - Models explored: K-Means (retained), DBSCAN, GMM + validation methods (Elbow/Silhouette).
  - Output: cluster label used as **severity context** (metabolic profile group).
- **DSO3 (Hybrid recommendation engine)**
  - **Knowledge-based rules** + deviation analysis (Z-scores) + cluster context (K-Means label).
  - Two-audience generation:
    - **Doctor view:** clinical phrasing + monitoring priorities.
    - **Patient view:** simplified, actionable instructions.
  - Note: LLM layer (if enabled) is **rewrite/explanation only**; it does not change scoring logic.

### 5) Evaluation
- **DSO1**
  - Metrics: Accuracy, Precision, Recall, F1, ROC/AUC, Confusion Matrix.
  - Model selection based on performance + stability (reported best: MLP and XGBoost).
- **DSO2**
  - Cluster validation: Silhouette/Elbow + interpretability (cohesion vs separation).
- **DSO3**
  - Functional validation: scenario-based checks (expected severity for known abnormal patterns),
  - Consistency checks between Z-score signals, cluster context, and final recommendations.

### 6) Deployment
- **Serving**
  - **FastAPI** backend exposes endpoints for DSO1/DSO2/DSO3 inference and user flows.
  - Model artifacts loaded at runtime for prediction/clustering/recommendation.
- **Frontend**
  - **HTML/CSS/JavaScript** web UI: Doctor dashboard + Patient dashboard + Education center + appointment flow.
- **Persistence**
  - Store users, patients, biomarker records, predictions, clusters/profiles, and history logs.
- **Operational concerns**
  - Input validation with Pydantic schemas, basic error handling, and reproducible inference pipelines.

---

## Data Science Objectives (DSO)

### DSO1 — Binary Classification (Benign vs Malignant)
- **Dataset:** Wisconsin Diagnostic Breast Cancer (WDBC)
- **Goal:** classify tumors as **Benign (0)** or **Malignant (1)**
- **Models implemented:** XGBoost, Random Forest, SVM, KNN, MLP, Softmax Regression, Linear Regression baseline, GRU-SVM hybrid

### DSO2 — Clustering Cancer Patterns (Metabolic Profiles)
- **Dataset:** Coimbra Breast Cancer Dataset
- **Features:** Age, BMI, Glucose, Insulin, HOMA, Leptin, Adiponectin, Resistin, MCP-1
- **Approach:** clustering explored via Elbow/Silhouette; **K-Means** retained
- **Interpretation:** clusters represent metabolic risk profiles (moderate → intermediate → severe)

### DSO3 — Recommendation System (Hybrid: Content + Knowledge)
- **Core logic**
  - Build **healthy reference baseline** (mean/std/min/max + percentiles)
  - Compute **Z-scores** per biomarker vs healthy baseline
  - Map deviations to severity labels: Normal / Mild / Moderate / Severe
  - Predict cluster assignment (K-Means context) + compute overall severity score
  - Generate biomarker-specific + cluster-level recommendations

