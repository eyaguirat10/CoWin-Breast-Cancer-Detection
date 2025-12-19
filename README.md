# 🎀 WeWomen — AI-Powered Breast Cancer Detection, Clustering & Recommendations 🎀

> Web platform for breast cancer decision support: **diagnosis prediction (DSO1)**, **metabolic risk clustering (DSO2)**, and **personalized recommendations (DSO3)**, with separate experiences for **doctors** and **patients**.

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

> UI snapshots are available in the deployment document (login/register, patient dashboard, education center, doctor dashboard, prediction modal, patient history modal).  

---

## Data Science Objectives (CRISP-DM aligned)
### DSO1 — Binary Classification (Benign vs Malignant)
- **Dataset:** Wisconsin Diagnostic Breast Cancer (WDBC)
- **Goal:** classify tumors as **Benign (0)** or **Malignant (1)**
- **Models implemented:** XGBoost, Random Forest, SVM, KNN, MLP, Softmax Regression, Linear Regression baseline, GRU-SVM hybrid
- **Top results (from report):**
  - **MLP:** 99.22% accuracy, AUC ≈ 0.999
  - **XGBoost:** 98.44% accuracy, AUC ≈ 0.998
  - Strong alternatives: SVM variants, Random Forest

### DSO2 — Clustering Cancer Patterns (Metabolic Profiles)
- **Dataset:** Coimbra Breast Cancer Dataset
- **Features:** Age, BMI, Glucose, Insulin, HOMA, Leptin, Adiponectin, Resistin, MCP-1
- **Approach:** clustering explored via Elbow/Silhouette/Dendrogram; **K-Means** retained as best-performing structure in k=3 evaluation (cohesion/separation)
- **Interpretation:** clusters reflect a gradient of metabolic risk (moderate → intermediate → severe)

### DSO3 — Recommendation System (Hybrid: Content + Knowledge)
- **Core logic**
  - Build **healthy reference baseline** (mean/std/min/max + percentiles)
  - Compute **Z-scores** per biomarker vs healthy baseline
  - Map deviations to severity labels: Normal / Mild / Moderate / Severe
  - Predict cluster assignment (K-Means context) + compute overall severity score
  - Generate biomarker-specific + cluster-level recommendations
- **Explainability layer (LLM)**
  - LLM rewrites outputs for two audiences:
    - **Doctor-oriented:** clinical/technical language, monitoring priorities, red flags
    - **Patient-oriented:** simplified, actionable guidance
  - LLM does **not** change decision logic (rewrite-only layer)

---

## System Architecture
- **Frontend (HTML/CSS/JS)**
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

