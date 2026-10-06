# Automated-Analysis-Pipeline

# Automated Analysis Pipeline: Diabetes Prediction (Pima Indians Diabetes Database)

Colab Link: (https://colab.research.google.com/drive/1vghxQ6fQZAcZX5pmrux9sK3e0CKY7k3R#scrollTo=OVaf3I1zyE70)

**Author:** Kundan Rao

## Purpose

This Google Colab notebook demonstrates an automated, reproducible analysis pipeline in health data science. It uses conditional logic to adapt the analysis to the characteristics of the data and to user-defined settings. Its workflow covers:

1. **Automated data ingestion:** detects the file format (CSV, TSV, TXT, Excel, JSON, Parquet) and picks the right reader.
2. **Data quality checks:** replaces impossible zeros with missing values and reports missing data.
3. **Exploratory data analysis:** summaries, correlations (Pearson, Spearman, Phi-K), and many plots.
4. **Inferential statistics:** one-sample t-test, Welch's independent t-test, one-way ANOVA, and chi-square test of independence.
5. **Supervised machine learning:** logistic regression and a cross-validated decision tree predict diabetes status (binary outcome), evaluated with accuracy, recall, precision, F1, ROC AUC, and confusion matrices.

### My additions
Four new automated blocks (A-D), plus modified settings and extra documentation. The full list is in the **ADDED MODIFICATIONS** section at the end of the notebook.

| Block | What it does |
|---|---|
| A. Data quality report | Labels every column OK, Minor, or PROBLEM from zeros and missingness |
| B. Variable-type detection | Treats a column as categorical or numeric, then recommends mean or median |
| C. Risk groups | Groups BMI, Age, or Glucose and computes diabetes rates, with small-group warnings |
| D. Outlier check | IQR rule with a summary/detail switch |

## Repository contents

| File | Description |
|---|---|
| `Automated_Analysis_Pipeline_Notebook.ipynb` | The completed notebook |
| `diabetes.csv` | The dataset as a CSV. **Replace the name with your file's name.** |
| `diabetes.<original extension>` | The dataset in its original format, if it differs from CSV. **Delete this row if the original is already CSV.** |
| `README.md` | This file |

## How to run the analysis

1. Click the **Open in Colab** badge above (or upload the `.ipynb` file to [Google Colab](https://colab.research.google.com)).
2. Select **Runtime → Restart session and run all**.
3. When the first cell asks for a file, click **Choose files** and upload the dataset from this repository (`diabetes.csv`). Upload exactly one file.
4. Wait for all cells to finish. The decision tree grid search takes several seconds.

To try the adaptive behavior, change the settings at the top of cells and re-run them:
- Block B: `max_categories`, `skew_limit`
- Block C: `GROUP_VARIABLE` (`"BMI"`, `"Age"`, `"Glucose"`)
- Block D: `REPORT_MODE` (`"summary"` or `"detail"`), `multiplier`, `outlier_limit`
- Decision tree cell: `scoring_metric`

## Required software and libraries

The notebook runs in Google Colab, where most libraries are already installed.

| Library | Used for |
|---|---|
| Python 3 | Language |
| pandas, numpy | Data handling |
| matplotlib, seaborn | Plots |
| scipy, statsmodels | Statistical tests |
| scikit-learn | Machine learning pipelines and metrics |
| phik | Phi-K correlation (installed automatically by the notebook with `pip`) |
| openpyxl / pyarrow | Only needed if you load Excel or Parquet files |

Exact versions are printed in the final cell of the notebook.

**Running locally:** the first cell uses `google.colab.files.upload()`, which only works in Colab. To run outside Colab, replace that cell with `df = pd.read_csv("diabetes.csv")`.

## Dataset

**Pima Indians Diabetes Database** (National Institute of Diabetes and Digestive and Kidney Diseases). It has 768 women of Pima Indian heritage aged 21 or older, 8 numeric predictors (Pregnancies, Glucose, BloodPressure, SkinThickness, Insulin, BMI, DiabetesPedigreeFunction, Age), and a binary `Outcome` (1 = diabetes, 0 = no diabetes).

*Smith, J.W., Everhart, J.E., Dickson, W.C., Knowler, W.C., & Johannes, R.S. (1988). Using the ADAP learning algorithm to forecast the onset of diabetes mellitus. Proceedings of the Symposium on Computer Applications and Medical Care, 261-265.*

## Assumptions

- The data file is a single table with an `Outcome` column coded 0/1 and numeric predictors.
- Zeros in Glucose, BloodPressure, SkinThickness, Insulin, and BMI are physically impossible and treated as missing. The dataset used here appeared already cleaned, so the data quality report found no such zeros.
- Missing predictor values are filled with the median learned from the training set only, then applied to the test set to avoid data leakage.
- A 70/30 stratified train/test split with `random_state=42` is used throughout.
- Statistical tests use a significance level of alpha = 0.05.

## Limitations

- This is a single, small, instructional dataset. Results may not generalize to other populations.
- Results come from one train/test split, so the model performance numbers vary with the split.
- Findings are associations, not causal effects.
- Grouping cutoffs (BMI, Age, Glucose) and the mean-median skew rule are simple screening rules, not clinical standards.
- Outliers are flagged for review only. No values are removed or changed.
- **This project is for instruction and demonstration only. It must not be used for clinical decisions.**
