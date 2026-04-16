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

## 🚀 How to Run

Follow these steps to properly set up and run the GNSS Spoofing
Detection pipeline in accordance with the problem statement and dataset
requirements:

------------------------------------------------------------------------

### 1. Clone the Repository

``` bash
git clone <your-repo-link>
cd <repo-folder>
```

------------------------------------------------------------------------

### 2. Create Virtual Environment (Recommended)

``` bash
python -m venv venv
source venv/bin/activate      # On Mac/Linux
venv\Scripts\activate         # On Windows
```

------------------------------------------------------------------------

### 3. Install Required Libraries

Make sure you have Python ≥ 3.8 installed.

``` bash
pip install --upgrade pip
pip install numpy pandas scikit-learn xgboost matplotlib seaborn jupyter
```

------------------------------------------------------------------------

### 4. Setup Dataset

Create a `data/` directory in the root of the project:

``` bash
mkdir data
```

Place the following files inside the `data/` folder:

    data/
    │── test.csv
    │── submission_format.csv


------------------------------------------------------------------------

### 5. Verify File Access in Code

Make sure your code reads files like this:

``` python
import pandas as pd

test = pd.read_csv("data/test.csv")
submission_format = pd.read_csv("data/submission_format.csv")
```

------------------------------------------------------------------------

### 6. Run the Pipeline

``` bash
jupyter notebook
```

-   Open: `notebooks/code.ipynb`
-   Run all cells sequentially


------------------------------------------------------------------------

### 7. Generate Submission File

-   The model will generate predictions on `test.csv`
-   Output file must:
    -   Match **exact format of `submission_format.csv`**
    -   Contain correct column names
    -   Be saved as:

``` bash
submission.csv
```


------------------------------------------------------------------------

Your pipeline should now be fully reproducible and compliant with the
requirements.


## 🎯 Final Insight

This system detects spoofing by identifying:

> Violations in spatial diversity, temporal continuity, and physical consistency
