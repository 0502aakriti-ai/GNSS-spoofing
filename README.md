# 🛰️ GNSS Spoofing Detection  
### Physics-Aware, AI-Driven Signal Anomaly Detection Pipeline

---

## 📌 Overview

This project presents a **physics-aware anomaly detection framework** for identifying GNSS spoofing attacks.  
Unlike conventional classification pipelines, this approach models spoofing as a **violation of physical consistency across space and time**.

---

## 🧠 Problem Framing

GNSS spoofing is fundamentally a **signal integrity attack**, where adversaries attempt to replicate satellite signals.

However, spoofers fail to replicate:
- Independent satellite motion  
- True geometric diversity  
- Natural temporal evolution  
- Physical coupling between measurements  

---

## ⚙️ Feature Engineering: Signal Intelligence Layer

Features are designed to expose **latent inconsistencies** across spatial, temporal, and physical domains.

---

## 🔬 Feature Selection Rationale

### 1. Doppler Coefficient of Variation (`doppler_cv`)
- Captures satellite motion diversity  
- Spoofers produce uniform Doppler → low variation  

### 2. Active Channel Count (`active_channels`)
- Detects satellite dropout or unnatural stability  

### 3. CN0 Features (`cn0_mean`, `cn0_cv`, `cn0_high`)
- Detect overpowering and unnatural signal cleanliness  

### 4. Pseudorange Spread (`range_spread`)
- Detects unrealistic satellite geometry  

### 5. Temporal Jumps (`doppler_jump`, `phase_jump`)
- Captures spoofing takeover transitions  

### 6. Rolling Statistics
- Detect sustained anomalies over time  

### 7. Doppler–Clock Drift Ratio (`tcd_doppler_ratio`)
- Identifies violations of physical signal relationships  

---

## 🏗️ Model Architecture

### Stage 1: Isolation Forest
- Unsupervised anomaly detection  
- Generates pseudo-labels  

### Stage 2: XGBoost
- Learns non-linear relationships  
- Produces spoofing probability  

---

## 🧪 Training Strategy

- Removed inactive channels  
- Standardized features  
- Used pseudo-labeling  
- Handled imbalance via `scale_pos_weight`  
- Optimized threshold using Weighted F1  

---

## 🚀 Run Instructions

```bash
pip install pandas numpy scikit-learn xgboost
python gnss_pipeline.py
```

---

## 🎯 Final Insight

This system detects spoofing by identifying:

> Violations in spatial diversity, temporal continuity, and physical consistency
