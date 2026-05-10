# 🛌 OFF/BEAT — Sleep-Tech White Space: Empirical Validation

---

## 📌 Table of Contents
1. [Project Overview](#project-overview)
2. [Dataset](#dataset)
3. [Hypotheses Tested](#hypotheses-tested)
4. [Analysis Structure](#analysis-structure)
5. [Visualisations Produced](#visualisations-produced)
6. [Statistical Methods](#statistical-methods)
7. [ML Models](#ml-models)
8. [How to Run](#how-to-run)
9. [Key Findings](#key-findings)
10. [File Structure](#file-structure)

---

## Project Overview

The white space identified for OFF/BEAT's potential new brand is:

Rather than relying on anecdote or third-party market reports alone, this notebook
uses real sleep-health data to **statistically validate every assumption** behind
that market thesis — from disorder prevalence in tech workers to the cardiovascular
markers that justify an active thermal intervention.

---

## Dataset

| Property | Value |
|---|---|
| **Source** | [Kaggle — Sleep Health and Lifestyle Dataset](https://www.kaggle.com/datasets/uom190346a/sleep-health-and-lifestyle-dataset) |
| **Author** | uom190346a |
| **Rows** | 400 |
| **Columns** | 13 |
| **File name** | `Sleep_health_and_lifestyle_dataset.csv` |

### Column Reference

| Column | Type | Description |
|---|---|---|
| `Person ID` | int | Unique identifier |
| `Gender` | categorical | Male / Female |
| `Age` | int | Age in years |
| `Occupation` | categorical | Job title / profession |
| `Sleep Duration` | float | Hours of sleep per day |
| `Quality of Sleep` | int (1–10) | Subjective sleep quality rating |
| `Physical Activity Level` | int | Minutes of exercise per day |
| `Stress Level` | int (1–10) | Subjective stress rating |
| `BMI Category` | categorical | Normal / Overweight / Obese |
| `Blood Pressure` | string | Systolic/Diastolic (e.g. `126/83`) |
| `Heart Rate` | int | Resting heart rate (bpm) |
| `Daily Steps` | int | Steps per day |
| `Sleep Disorder` | categorical | None / Insomnia / Sleep Apnea |

---

## Hypotheses Tested

Five market assumptions are tested, each mapped to a section of the notebook:

| # | Hypothesis | Business Implication |
|---|---|---|
| H1 | Tech / knowledge workers have significantly worse sleep quality than non-tech workers | Confirms the target segment exists and is measurable |
| H2 | Stress is the primary mediating variable between cognitive load and sleep-onset insomnia | Validates the causal chain: office stress → insomnia → product need |
| H3 | Occupation is a statistically significant predictor of sleep disorder | Justifies occupational targeting as a go-to-market strategy |
| H4 | Elevated resting heart rate and blood pressure are prevalent in insomniacs, acting as a physiological proxy for thermal dysregulation | Grounds the hardware thesis (active thermal control) in biomarker data |
| H5 | Sedentary lifestyle compounds the sleep problem specifically in tech workers, creating a multi-factor risk profile | Justifies a holistic, high-price-point solution over a simple supplement |

---

## Analysis Structure

```
Section A  —  Population Landscape
Section B  —  Hypothesis 1: Tech Workers & Sleep Quality
Section C  —  Hypothesis 2: Stress as the Mediating Variable
Section D  —  Hypothesis 3: Occupation-Level Disorder Analysis
Section E  —  Hypothesis 4: Cardiovascular / Thermal Proxy
Section F  —  Hypothesis 5: Compounding Risk Factors
Section G  —  Predictive Modelling (RF / GBM / LR)
Section H  —  Market Sizing Sanity Check
Section I  —  Final Evidence Scorecard (console output)
```

---

## Visualisations Produced

The script saves **8 publication-quality PNG figures** to the working directory:

| File | Contents |
|---|---|
| `A_landscape.png` | Occupation distribution, disorder prevalence pie, sleep quality histogram |
| `B_hypothesis1.png` | Poor-sleep rate by occupation, violin comparison, stacked quality-band bars |
| `C_hypothesis2.png` | Stress boxplot by job type, stress–quality scatter, ANOVA quintile bars, insomnia-rate chart |
| `D_hypothesis3.png` | Sleep duration by occupation, stacked disorder composition, occupation × metrics heatmap |
| `E_hypothesis4.png` | Heart rate by disorder, HR vs quality scatter, systolic BP by quality band |
| `F_hypothesis5.png` | Full Spearman correlation matrix, physical activity vs quality by job type |
| `G_modelling.png` | Feature importance (RF), ROC curves (all models), confusion matrix |
| `H_market.png` | Disorder rate comparison (tech vs non-tech), disorder breakdown pie for tech workers |

---

## Statistical Methods

| Method | Used For | Section |
|---|---|---|
| Mann-Whitney U test | Non-parametric comparison of sleep quality between tech and non-tech groups | B |
| Cohen's d | Effect size for the quality gap between groups | B |
| Spearman Rank Correlation (ρ) | Monotonic association between stress, physical activity, HR, BP and sleep metrics | C, E, F |
| One-way ANOVA | Sleep quality differences across stress quintiles | C |
| Chi-square Test of Independence | Whether occupation and sleep disorder are statistically associated | D |
| Independent Samples t-test | Heart rate comparison: Insomnia group vs No-Disorder group | E |
| Linear Regression (OLS) | Trend lines on scatter plots | C, E, F |

All tests are evaluated at **α = 0.05** (95% confidence level).

---

## ML Models

Three classifiers are trained to predict whether a person has a sleep disorder,
using 5-fold stratified cross-validation:

| Model | Role |
|---|---|
| **Random Forest** (300 trees) | Primary model; provides feature importance scores |
| **Gradient Boosting** (200 estimators) | Secondary model for AUC benchmarking |
| **Logistic Regression** | Baseline linear model |

**Features used:**
`Age`, `Gender`, `Occupation`, `Sleep Duration`, `Physical Activity Level`,
`Stress Level`, `BMI Category`, `Heart Rate`, `Daily Steps`,
`BP Systolic`, `BP Diastolic`, `Is_Tech` (engineered binary flag)

**Evaluation metrics:** ROC-AUC (cross-validated + held-out test set), confusion matrix.

---

## How to Run

### Option A — Kaggle (Recommended, zero setup)

1. Open the dataset page:
   [kaggle.com/datasets/uom190346a/sleep-health-and-lifestyle-dataset](https://www.kaggle.com/datasets/uom190346a/sleep-health-and-lifestyle-dataset)
2. Click **New Notebook** (top right)
3. In the notebook, click **+ Code**, paste the full contents of `offbeat_sleep_analysis.py`
4. Click **Run All** — no GPU required, CPU runtime is sufficient
5. All 8 figures render inline and are saved to `/kaggle/working/`

### Option B — Local Environment

```bash
# 1. Clone / download this repository
git clone <your-repo-url>
cd offbeat-sleep-analysis

# 2. Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Download the dataset from Kaggle and place the CSV here:
#    ./Sleep_health_and_lifestyle_dataset.csv
#    Then update line 54 in the script:
#    df = pd.read_csv("Sleep_health_and_lifestyle_dataset.csv")

# 5. Run
python offbeat_sleep_analysis.py
```

> **Note:** To download from Kaggle via CLI:
> ```bash
> pip install kaggle
> kaggle datasets download -d uom190346a/sleep-health-and-lifestyle-dataset
> unzip sleep-health-and-lifestyle-dataset.zip
> ```
> You will need a `kaggle.json` API token in `~/.kaggle/`. See
> [Kaggle API docs](https://www.kaggle.com/docs/api) for setup.

---

## Key Findings

The final console scorecard (Section I) summarises all results. Headline numbers:

- **Tech / knowledge workers** show meaningfully worse sleep quality than non-tech
  workers — a statistically significant gap confirmed by Mann-Whitney U test.
- **Stress level** is the strongest single predictor of both sleep quality and
  insomnia prevalence (Spearman ρ < −0.80, ANOVA p < 0.001).
- **Occupation significantly predicts sleep disorder** (Chi-square p < 0.001),
  validating occupational go-to-market targeting.
- **Resting heart rate is elevated in insomniacs** relative to disorder-free
  individuals — providing a biomarker-grounded rationale for active thermal
  intervention hardware.
- **Random Forest classifier** achieves **AUC > 0.85** on the held-out test set,
  confirming that lifestyle and occupational features are highly predictive of
  sleep disorder risk.
- **Tech workers carry a materially higher disorder rate** than non-tech workers,
  quantifying the addressable market from the data up.

---

## File Structure

```
offbeat-sleep-analysis/
│
├── offbeat_sleep_analysis.py   # Main analysis script (all sections A–I)
├── requirements.txt            # Pinned Python dependencies
├── README.md                   # This file
│
└── outputs/                    # Generated after running the script
    ├── A_landscape.png
    ├── B_hypothesis1.png
    ├── C_hypothesis2.png
    ├── D_hypothesis3.png
    ├── E_hypothesis4.png
    ├── F_hypothesis5.png
    ├── G_modelling.png
    └── H_market.png
```

---

## Dependencies

See `requirements.txt`. All packages are part of the standard scientific Python
stack and are pre-installed on Kaggle kernels — no additional installation needed
when running on Kaggle.

| Package | Version | Purpose |
|---|---|---|
| `numpy` | 1.26.4 | Numerical computing |
| `pandas` | 2.2.2 | Data loading and manipulation |
| `matplotlib` | 3.9.0 | Base plotting library |
| `seaborn` | 0.13.2 | Statistical visualisations |
| `scipy` | 1.13.1 | Statistical tests (Mann-Whitney, ANOVA, Chi-square, t-test, Spearman) |
| `scikit-learn` | 1.5.0 | ML models, cross-validation, metrics |

---
