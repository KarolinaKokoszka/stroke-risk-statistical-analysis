# Stroke Risk Factors – Statistical Analysis in Python

Statistical analysis of the **Stroke Prediction** dataset (5,110 patients, 12 variables), done for the *Statistical Data Analysis* course (MSc in Computer Science, Data Science specialisation, Cracow University of Technology).
The project goes from **exploratory data analysis** to **formal statistical inference**: which patient characteristics are associated with stroke, and are the differences statistically significant?

> Notebooks are in Polish. They are saved with their outputs, so all tables and charts can be viewed directly on GitHub.

![Age, glucose and BMI by stroke status](images/stroke_age_glucose_bmi.png)

## Tech stack
Python · pandas · NumPy · SciPy · statsmodels · scikit-posthocs · Matplotlib · seaborn · Jupyter Notebook

## 1. Exploratory data analysis
📓 [`01_stroke_eda.ipynb`](01_stroke_eda.ipynb)

- Description of all variables (type, cardinality, meaning).
- Data quality: missing BMI values (3.9%) and a check of whether they are random, extreme but plausible values, rare categories (`gender = Other`) and the `Unknown` smoking category (30% of records).
- Descriptive statistics and distributions (histograms, box and violin plots, bar and pie charts).
- **Class imbalance:** only 4.9% of patients had a stroke.
- Group comparisons and relationships: age, glucose and BMI by stroke status; smoking, hypertension, gender and marital status vs stroke.

<img src="images/hypertension_vs_stroke.png" width="380">

**Key findings**
- **Age** is the strongest factor: mean age 67.7 years in the stroke group vs 42.0 without a stroke.
- **Glucose level** is higher in patients with a stroke (mean 132.5 vs 104.8 mg/dL), with a second peak around 200–250 mg/dL that probably corresponds to diabetic patients.
- **Hypertension:** stroke is more than 3× more frequent (13.3% vs 4.0%).
- The apparent link between **marriage** and stroke disappears after controlling for age, so it is a **confounding effect**.

## 2. Hypothesis testing and estimation
📓 [`02_stroke_hypothesis_testing.ipynb`](02_stroke_hypothesis_testing.ipynb)

- **Point and interval estimation:** confidence intervals for means (normal approximation) and proportions (Wilson method).
- **Bootstrap** confidence interval for mean glucose (5,000 resamples), compared with the classical interval.
- **Choosing the test from the assumptions:** Shapiro–Wilk (normality, tested on subsamples because the test is oversensitive for large n) and Levene (equal variances).
- **Post-hoc analysis** with Dunn's test after Kruskal–Wallis.

| Research question | Test | Result |
|-------------------|------|--------|
| Does age differ between patients with and without a stroke? | Mann–Whitney U | p < 0.001, median ~71 vs ~43 years |
| Is smoking associated with stroke? | Chi-square test of independence | χ² = 11.49, p ≈ 0.003 |
| Does glucose level differ between work types? | ANOVA + **Kruskal–Wallis** (ANOVA assumptions violated) + Dunn post-hoc | H = 17.30, p ≈ 0.002; children differ from private-sector and self-employed workers |
| What share of patients had a stroke? | Wilson confidence interval | 4.87% (95% CI: 4.32–5.50%) |

<p>
<img src="images/bootstrap_glucose_mean.png" width="49%">
<img src="images/dunn_posthoc.png" width="45%">
</p>

The bootstrap and classical confidence intervals are almost identical (difference of about 0.01 mg/dL), which confirms the classical result is stable for a sample of this size.

## Possible next steps
- Multivariable analysis (e.g. logistic regression) with age, glucose, BMI, hypertension and smoking together.
- Testing the smoking–stroke association while controlling for age, since part of the effect may come from age differences between groups.

## Data
The dataset is **not included** in this repository. Download it from Kaggle and save it next to the notebooks as `healthcare-dataset-stroke-data.csv`:
[Stroke Prediction Dataset](https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset)

## How to run
```bash
git clone https://github.com/KarolinaKokoszka/stroke-risk-statistical-analysis.git
cd stroke-risk-statistical-analysis
pip install pandas numpy scipy statsmodels scikit-posthocs matplotlib seaborn jupyter
# download the dataset (see Data above)
jupyter notebook
```
