# WeWomen — AI-Powered Breast Cancer Detection, Subtyping & Recommendations

WeWomen is an end-to-end clinical decision support concept that combines:
- **DSO1 (Classification):** real-time malignancy prediction
- **DSO2 (Clustering):** metabolic subtype / risk profile assignment
- **DSO3 (Recommendations):** personalized guidance with an LLM used **only as an explanation layer**

> This project is designed as **decision support**, not autonomous diagnosis. Outputs must be clinically validated by a healthcare professional.

---

## Product snapshot

### Roles & UI
- **Patient experience**
  - Sign up / sign in
  - Personal dashboard with health summary, reminders, and tools
  - “Patient Education Center” (articles, videos, downloadable resources)
  - Appointment booking flow
  - “Patient Health Guide” form for entering biomarker data

- **Doctor experience**
  - Doctor dashboard (patient list + risk labels)
  - **Breast Cancer Prediction (DSO1)** modal: enter features → run prediction → view result + probabilities
  - **Assign Patient Profile (DSO2)** modal: enter biomarkers → assign cluster/profile → interpretation
  - Patient medical history view (stored DSO1 + DSO2 outcomes)

---

## What’s inside (Data Science)

### DSO1 — Tumor Classification (Supervised Learning)
- Goal: classify tumor as benign vs malignant
- Reported top models for deployment:
  - **MLP ~99.22% accuracy**
  - **XGBoost ~98.44% accuracy**

### DSO2 — Patient Subtyping (Unsupervised Learning)
- Goal: identify clinically interpretable metabolic subtypes
- Selected approach:
  - **K-Means (k=3)** (best overall clustering quality vs alternatives)

### DSO3 — Hybrid Recommendation System (+ LLM explanation)
- Goal: generate recommendations as a **clinical decision support** output, evaluated by:
  - functional validity
  - clinical coherence
  - usability
  - scenario-based validation
- LLM is used to produce:
  - **Doctor-oriented**: clinical summaries, risk stratification, prioritized interventions, monitoring notes
  - **Patient-oriented**: simplified language, actionable lifestyle advice
- Important: the LLM does **not** change the medical logic—only explains it.

---

## End-to-end pipeline (high level)

1. **Input:** patient biomarker data  
2. **Preprocessing:** scaling / encoding / feature selection  
3. **Classification (DSO1):** malignancy prediction  
4. **Clustering (DSO2):** metabolic subtype assignment  
5. **Recommendation (DSO3):** personalized guidance generation  
6. **Output:** role-based reports (doctor vs patient)

---

## Safety & clinical integration principles

- Human oversight is mandatory for all diagnoses
- Predictions must be clinically validated
- Clear communication of confidence/probabilities
- Transparent explanations and auditability (recommended)

---

## Repository structure (suggested)

> Adjust these names to match your actual folders.

