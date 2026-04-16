
# GNSS Spoofing Detection

### HighPrep PS3 Solution

Building a robust, AI-driven detection pipeline for GNSS spoofing using temporal signal analysis, anomaly detection, and gradient-boosted modeling.

---


## 1. Problem Understanding
Global Navigation Satellite Systems (GNSS) are fundamental to modern infrastructure, supporting aviation, maritime navigation, autonomous drones, and financial systems. However, GNSS signals are inherently low-power and unencrypted, making them highly vulnerable to spoofing attacks.

Spoofing involves transmitting manipulated signals that cause a receiver to compute incorrect position, velocity, or timing (PVT) information. Such attacks can lead to safety risks, financial losses, and infrastructure instability. Our objective is to design an AI-driven solution that detects these spoofing attempts by analyzing signal behavior, temporal patterns, and cross-channel inconsistencies.

## 2. Feature Engineering
Our pipeline transforms raw GNSS observations into a comprehensive feature matrix designed to highlight anomalous signal behavior:

Statistical Aggregation: For each timestamp, we compute the mean, standard deviation, and range of key signals (CN0, Doppler, Pseudorange, Carrier Phase) across all active channels to identify global distortions.

Temporal Dynamics: We use consecutive differences (.diff()) to capture "jumps" in signal strength and Doppler shifts, which are often indicative of a spoofer taking control of the receiver's tracking loops.

Cross-Channel Consistency: We engineered a Doppler Coefficient of Variation (doppler_cv) and active_channels count. A spoofer broadcasting from a single source often produces suspiciously uniform signal patterns across all channels, whereas real satellites exhibit high variance.

Signal Quality Metrics: Features like cn0_high and tcd_doppler_ratio help detect unnaturally strong signals or deviations in the expected relationship between clock drift and Doppler.

## 3. Model Architecture
We implemented a two-stage anomaly detection architecture to handle the lack of labeled training data:

Anomaly Extraction Agent (Isolation Forest): An unsupervised model used to identify data points that deviate significantly from standard satellite signal distributions. This stage generates high-confidence pseudo-labels for "Authentic" vs "Spoofed" signals.

Refinement Classifier (XGBoost): A Gradient Boosted Decision Tree (GBDT) model that learns the complex, non-linear relationships between the engineered features and the generated pseudo-labels.

Decision Fusion: The final model outputs a spoofing probability, which is passed through a tuned threshold to ensure optimal detection confidence.

## 4. Training Methodology
Data Cleaning: Inactive channels (where PRN=0) were filtered out before feature engineering to prevent the model from learning noise or null values as spoofing behavior.

Preprocessing: Features were normalized using StandardScaler to ensure that high-magnitude features like Pseudorange do not overshadow subtle changes in Signal-to-Noise ratios.

Class Imbalance: To address the rarity of spoofing events, we utilized the scale_pos_weight parameter in XGBoost to penalize the misclassification of the minority "Spoofed" class.

Threshold Tuning: We performed an exhaustive search for the optimal probability threshold to maximize the Weighted F1-Score on our validation set, ensuring a robust balance between precision and recall.

Validation Strategy: We employed a chronological split (last 20% of data for validation) to test the model's ability to generalize to unseen temporal dynamics.
## 5. Justification of Design Decisions
The development of this solution was guided by the need for Robustness, Generalization, and Practical Feasibility. Each technical choice was made to address the unique vulnerabilities of GNSS signals.

Unsupervised-to-Supervised Pipeline
Why Isolation Forest? Since real-world spoofing data is often unlabeled or scarce, we used an Isolation Forest to generate high-confidence pseudo-labels. This model is specifically designed to isolate anomalies—like sudden signal jumps—from the "normal" clusters of authentic satellite data.

Why XGBoost? After identifying anomalies, we trained an XGBoost classifier. This gradient-boosting framework was chosen for its superior ability to handle non-linear relationships and feature interactions (e.g., how Signal-to-Noise Ratio relates to Time Clock Drift) without requiring extensive feature scaling.

Strategic Feature Selection
Multi-Agent Consistency: We focused on features that compare behavior across multiple channels, such as active_channels and doppler_cv. Because a spoofer typically broadcasts from a single source, it often creates "too-perfect" or uniform signal patterns across all channels, which our model identifies as a high-risk indicator.

Temporal Dynamics: By including "jump" features (e.g., doppler_jump, phase_jump), the model captures the specific moment a receiver is "pulled" from an authentic signal to a spoofed one.

Robustness & Fair Evaluation
Scale-Pos-Weight: To account for the fact that spoofing attacks are rare (class imbalance), we used the scale_pos_weight parameter to ensure the model does not become biased toward "Normal" operation.

Chronological Validation: We implemented a time-based split for training and validation to ensure the model can generalize to future signal behavior rather than simply memorizing patterns from a specific time window.

Fairness & Data Integrity: Strict measures were taken to ensure no data leakage occurred, maintaining a clear separation between training data and the official test dataset provided.

### How to Run the Project

## 1. Prerequisites
Ensure you have Python 3.8+ installed. It is recommended to use a virtual environment.

## 2. Install Dependencies
Install the required libraries (specifically xgboost, pandas, and scikit-learn) using the following command:

Bash
pip install -r requirements.txt
## 3. Data Setup
Create a folder named dataset in the root directory.

Place the official train.csv and test.csv files inside the dataset/ folder.

Note: As per competition rules, these raw data files are not included in this repository.

## 4. Running the Pipeline
You can run the full detection pipeline using the Jupyter Notebook:

Open the notebook:

Bash
jupyter notebook notebooks/code.ipynb
Run all cells (Cell > Run All).

The notebook will:

Perform feature engineering (Temporal and Statistical analysis).

Train the XGBoost classifier.

Generate a submission.csv file in the output/ directory.

## 5. Evaluating Results
Once the notebook finishes execution, it will print a Feature Importance table. You can use these insights to verify which signal parameters (like Doppler shifts or Signal-to-Noise ratios) were most indicative of spoofing attempts.

🛠️ Built With
XGBoost - Primary Gradient Boosting Framework

Pandas & NumPy - Data Manipulation

Scikit-Learn - Preprocessing and Metrics

Jupyter - Interactive Development Environment
