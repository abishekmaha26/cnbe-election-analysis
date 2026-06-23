# 🗳️ CNBE Election Exit Poll — Voter Party Prediction

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.3%2B-orange?logo=scikit-learn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

> **Client:** CNBE News Channel  
> **Objective:** Predict whether a voter will vote **Labour** or **Conservative** using survey data, and use model outputs to power a real-time election night exit poll seat projection.

---

## 📌 Problem Statement

CNBE conducted a pre-election survey of **1,525 voters** across 9 variables. The goal is to build a classification model that:

1. Predicts each voter's party choice (Labour vs Conservative)
2. Assigns calibrated probabilities usable for **exit poll seat projections**
3. Identifies the **key drivers** of voter behaviour for on-air commentary

---

## 📁 Repository Structure

```
cnbe-election-analysis/
│
├── data/
│   └── Election_Data__1_.xlsx        # Raw survey dataset (1,525 voters × 9 variables)
│
├── notebooks/
│   └── CNBE_Election_Analysis.ipynb  # Full end-to-end ML analysis notebook
│
├── scripts/
│   └── generate_ml_report.py         # Standalone PDF report compiler (ReportLab)
│
├── reports/
│   └── ml_analysis_report.pdf        # Executive summary PDF (generated output)
│
├── requirements.txt                  # Python dependencies
├── .gitignore                        # Git ignore rules
└── README.md                         # This file
```

---

## 📊 Dataset Overview

| Variable | Type | Range | Description |
|---|---|---|---|
| `vote` | Categorical | Labour / Conservative | **Target variable** |
| `age` | Numeric | 24 – 93 | Voter age |
| `economic.cond.national` | Ordinal 1–5 | 1=Poor, 5=Excellent | National economic perception |
| `economic.cond.household` | Ordinal 1–5 | 1=Poor, 5=Excellent | Personal economic perception |
| `Blair` | Ordinal 1–5 | 1=Very Poor, 5=Excellent | Tony Blair approval rating ⭐ |
| `Hague` | Ordinal 1–5 | 1=Very Poor, 5=Excellent | William Hague approval rating ⭐ |
| `Europe` | Ordinal 1–11 | 1=Pro-EU, 11=Eurosceptic | EU integration attitude ⭐ |
| `political.knowledge` | Ordinal 0–3 | 0=None, 3=High | Voter political awareness |
| `gender` | Categorical | male / female | Voter gender |

> ⭐ = Top 3 discriminating features identified by Random Forest importance

**Class Distribution:** Labour 1,063 (69.7%) · Conservative 462 (30.3%)  
**Missing Values:** None

---

## 🤖 Models Trained

| Model | Type | Scaling Required |
|---|---|---|
| Logistic Regression | Linear | ✅ Yes (StandardScaler) |
| Linear Discriminant Analysis (LDA) | Linear | ❌ No |
| K-Nearest Neighbours (KNN) | Distance-based | ✅ Yes (StandardScaler) |
| Naïve Bayes (Gaussian) | Probabilistic | ❌ No |
| Random Forest | Bagging Ensemble | ❌ No |
| Gradient Boosting | Boosting Ensemble | ❌ No |
| Logistic Regression (Tuned) | Linear + GridSearchCV | ✅ Yes |

---

## 📈 Model Performance Results

| Rank | Model | Train Acc | Test Acc | AUC | Overfit Gap |
|---|---|---|---|---|---|
| 🥇 | **Gradient Boosting (Tuned)** | 89.2% | 84.1% | **0.904** | 5.1% |
| 🥈 | Naïve Bayes | 82.9% | 84.5% | 0.897 | −1.6% |
| 🥈 | Logistic Regression | 83.7% | 84.1% | 0.897 | −0.4% |
| 🥈 | LDA | 83.4% | 83.8% | 0.897 | −0.4% |
| 5 | Random Forest (Tuned) | 100.0% | 84.3% | 0.896 | 15.7% ⚠️ |
| 6 | KNN | 83.6% | 81.2% | 0.864 | 2.4% |

> **Note:** Minor metric variations are expected across runs due to train-test split randomness. The model **ranking and conclusions remain consistent** — this is normal in data science practice.

---

## 🏆 Best Model: Gradient Boosting

- **AUC: 0.904** — highest discriminative power
- **Test Accuracy: 84.1%** — strong generalisation
- Tuned hyperparameters: `learning_rate=0.05`, `max_depth=3`, `n_estimators=200`
- Correctly identifies **89.1% of Labour voters** and **73.2% of Conservative voters**

---

## 🔍 Key Findings

1. **Blair approval rating** is the #1 predictor (31.2% RF importance) — voters rating Blair 4–5 are overwhelmingly Labour
2. **Europe (Eurosceptic scale)** is #2 — high Euroscepticism strongly predicts Conservative vote
3. **Hague approval rating** is #3 — mirror of Blair effect
4. **Together these three features account for ~68% of model signal**
5. Economic condition variables are weak discriminators — voters across parties share similar economic views
6. **Projected outcome: Labour majority** — model assigns >70% Labour probability to ~69.7% of the sample

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/cnbe-election-analysis.git
cd cnbe-election-analysis
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the Notebook

```bash
jupyter notebook notebooks/CNBE_Election_Analysis.ipynb
```

### 4. Generate the PDF Report

```bash
python scripts/generate_ml_report.py
# Output: reports/ml_analysis_report.pdf
```

---

## 🛠️ Tech Stack

| Library | Version | Purpose |
|---|---|---|
| `pandas` | ≥1.5 | Data manipulation |
| `numpy` | ≥1.23 | Numerical operations |
| `matplotlib` | ≥3.6 | Plotting |
| `seaborn` | ≥0.12 | Statistical visualisation |
| `scikit-learn` | ≥1.2 | ML models, metrics, CV |
| `reportlab` | ≥4.0 | PDF report generation |
| `openpyxl` | ≥3.1 | Excel file reading |
| `jupyter` | ≥1.0 | Notebook environment |

---

## 📋 Notebook Structure

```
Cell 1  → Environment setup & data loading
Cell 2  → Descriptive statistics & null value check + inference
Cell 3  → Univariate analysis (histograms, pie chart)
Cell 4  → Bivariate analysis (boxplots, crosstab, correlation heatmap)
Cell 5  → Outlier detection (IQR method) + inference
Cell 6  → Label encoding + scaling discussion
Cell 7  → Stratified 70:30 train-test split
Cell 8  → evaluate_model() utility function
Cell 9  → Logistic Regression + coefficient plot
Cell 10 → LDA + discriminant score distribution
Cell 11 → KNN — elbow method (CV) + model training
Cell 12 → Naïve Bayes
Cell 13 → Model tuning — Logistic Regression (GridSearchCV)
Cell 14 → Random Forest (Bagging) + feature importance plot
Cell 15 → Gradient Boosting (Boosting) tuning
Cell 16 → All confusion matrices + overlaid ROC curves
Cell 17 → Final comparison table + recommendation
```

---

## 📄 PDF Report Contents

The executive summary PDF (`ml_analysis_report.pdf`) includes:

- **Title Banner** with CNBE branding
- **2-Column Key Findings** overview panel
- **9-Variable Data Dictionary** table
- **Feature Importance** horizontal bar chart
- **Model Performance Table** with colour-coded verdicts
- **AUC Bar Chart** (all models)
- **Confusion Matrix** with per-class accuracy breakdown
- **Methodology Table** (6-phase workflow)
- **5 Deployment Recommendations** for CNBE broadcast team
- **Final Verdict Box** with exit poll projection summary

---

## ⚠️ Important Note on Metric Variability

Accuracy, precision, F1-score, and AUC values may vary slightly between runs. This is **expected and not an error**. Causes include:

- Different `random_state` seeds producing different train/test splits
- Hyperparameter default differences across scikit-learn versions
- Stochastic elements in some solvers

The **model ranking and overall conclusions are stable** across all runs.

---

## 📜 License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

---

## 👤 Author

**Abishek** — Data Analyst  
Freelance Analytics · [Upwork](https://upwork.com) · [Fiverr](https://fiverr.com)

---

*Built for CNBE News Channel as part of the 2024 General Election Exit Poll project.*
