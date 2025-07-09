# Predicting Power Outage Severity

**By Kate Feng and Tracy Vu**

## 🧠 Introduction

This project explores how power outage data can be leveraged to **predict the number of affected customers** during an outage across different regions in the United States.

We used [Purdue University's Major Power Outage dataset](https://engineering.purdue.edu/LASCI/research-data/outage-risk) (2000–2016), which contains 1,534 major outage events with detailed attributes such as:

- Duration
- Geographic region
- Cause
- Climate
- Energy demand loss
- State-level demographic and consumption data

Our goal: **Predict customer impact size from outage characteristics** using a multiclass classification model.

---

## 🧼 Data Cleaning

We selected and renamed relevant features. Key columns included:

- `OUTAGE.DURATION` — minutes outage lasted
- `CUSTOMERS.AFFECTED` — number of people impacted
- `CAUSE.CATEGORY`, `CLIMATE.REGION`, `CLIMATE.CATEGORY`
- `TOTAL.SALES`, `POPULATION`, `DEMAND.LOSS.MW`

We also created a new target variable `AFFECTED_BUCKET`, which grouped outages into:

| Customers Affected | Label                    |
| ------------------ | ------------------------ |
| 0 - 10,000         | Small Customer Base      |
| 10,000 - 100,000   | Medium Customer Base     |
| 100,000 - 500,000  | Large Customer Base      |
| 500,000+           | Very Large Customer Base |

---

## 📊 Exploratory Data Analysis

### Univariate Analysis

- Most outages affected **Medium (10k–100k)** or **Large (100k–500k)** customer bases.
- States with most outages: **California, Texas, and Washington**

### Bivariate Analysis

- **Outage duration increases** with customer base size.
- **Energy consumption decreased** over time (2001–2016), with peak outages early in the dataset.

### Pivot Table Highlights

- Southwest U.S. generally showed **lower total energy consumption**.
- Cold/Normal climate states like **California and New York** had **consistently high usage** across months.

---

## ❓ Missingness Analysis

### NMAR Evaluation

- `'CUSTOMERS.AFFECTED'` likely Not Missing at Random (NMAR) due to differences in utility reporting.

### Dependency Tests

- `'DEMAND.LOSS.MW'` missingness **depends on `TOTAL.SALES`** (p = 0.001)
- No dependency between `'DEMAND.LOSS.MW'` and `'OUTAGE.DURATION'` (p = 0.34)

---

## 🧪 Hypothesis Testing

We tested whether `'Very Large Customer Base'` outages had significantly longer durations:

- **Null Hypothesis**: No difference in mean durations
- **Result**: p ≈ 0.0 → **Reject null**, Very large outages **last significantly longer**

---

## 🔮 Prediction Problem Framing

- **Prediction target**: `AFFECTED_BUCKET` (multiclass)
- **Features used**: Duration, cause, climate, population, energy stats
- **Model type**: Random Forest Classifier
- **Metric**: F1-score (balances precision + recall)

---

## 🏗 Baseline Model

**Features used**:

- `CAUSE.CATEGORY`, `CLIMATE.REGION`, `CLIMATE.CATEGORY`, `OUTAGE.DURATION`

**F1-score**: 0.55

- High performance for **Small** outages (F1: 0.89)
- **Zero accuracy** for Very Large outages (likely due to class imbalance)

---

## 🚀 Final Model

**Selected features**:

- `CAUSE.CATEGORY`, `OUTAGE.DURATION`, `POPDEN_URBAN`, `TOTAL.CUSTOMERS`, `POPULATION`, `TOTAL.SALES`, `DEMAND.LOSS.MW`

**Best Parameters**: Default `RandomForestClassifier` (better than GridSearch-tuned)

**Final F1-score**: 0.80

- Very Large Customer Base F1-score **improved to 0.83**
- High performance across all customer size groups

---

## ⚖️ Fairness Analysis

**Tested group**: Outages caused by **System Operability Disruption** vs. others

- Null Hypothesis: Accuracy is equal between groups
- Observed diff: -0.21
- **p = 0.079** → Fail to reject null → **No statistically significant unfairness** detected

---

## ✅ Summary

- Power outage impact can be **reliably predicted** from outage metadata
- Duration and cause are **strong predictors**
- Feature-rich models provide better support for **underrepresented classes**
- More granular data (e.g., local demographics) could further improve fairness and accuracy

---

## 📎 References

- [Purdue University - Major Power Outages Dataset](https://engineering.purdue.edu/LASCI/research-data/outage-risk)
- U.S. Energy Information Administration (EIA)
