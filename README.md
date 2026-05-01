# Digital Impact on Teen Minds

This repository contains an exploratory analysis of a student mental health dataset. The main work is in the Jupyter notebook `digital-impact-on-teen-minds.ipynb` which loads the dataset, cleans it, computes prevalence and treatment gaps, performs simple cross-tab analyses, and trains a RandomForest classifier to identify features associated with depression.

## Project structure

- `digital-impact-on-teen-minds.ipynb` - Primary analysis notebook. Load, clean, analyze, and model the data interactively.
- `Student Mental health.csv` - Dataset (loaded via Kaggle adapter inside the notebook).
- `requirements.txt` - (If present) Python package requirements for reproducing the environment.

## Goals

- Compute prevalence of depression, anxiety, and panic attacks among students.
- Check relationships between anxiety, panic attacks, gender, year of study and depression.
- Measure whether students with depression are receiving treatment and quantify the treatment gap.
- Train a RandomForest to surface features most predictive of depression.

## Key analyses performed in the notebook

1. Data loading
   - The notebook uses a `kagglehub` adapter to fetch `Student Mental health.csv` from the Kaggle dataset `shariful07/student-mental-health`.

2. Column renaming and basic cleaning
   - Columns are renamed for readability: `timestamp, gender, age, course, year_of_study, cgpa, marital_status, depression, anxiety, panic_attack, treatment`.
   - The `timestamp` column is dropped.

3. Normalizing binary response columns (depression/anxiety/panic_attack/treatment)
   - Common string values such as `Yes/No`, `True/False`, `1/0` (and case variants) are mapped to numeric 1/0.
   - Any remaining non-convertible values are coerced to `NaN` with `pd.to_numeric(..., errors='coerce')`.
   - Prevalence is calculated as the mean of these 0/1 columns (proportion positive).

4. Cross-tab and group analyses
   - Crosstabs and pivot tables to study relationships (e.g., anxiety vs depression, panic attacks vs depression, depression rates by gender and year of study).
   - Treatment gap: grouped counts and treated counts (treated = sum of normalized treatment), then untreated = count - treated.

5. Modeling: RandomForest feature importances
   - Preprocessing before modeling:
     - Convert `cgpa` to numeric.
     - One-hot encode categorical predictors with `pd.get_dummies(..., drop_first=True)`.
     - Fill missing values with column medians.
   - Train a `RandomForestClassifier` to identify features associated with depression.
   - The top features identified in a recent run included:
     - `marital_status_Yes`
     - `age`
     - `anxiety`
     - `panic_attack`
     - `treatment`
     (The notebook output contains a table of the top 20 features and importances.)

## Reproducing the analysis (macOS / zsh)

1. Create and activate a virtual environment (recommended):

```bash
python -m venv .venv
source .venv/bin/activate
```

2. Install dependencies. If there is a `requirements.txt` file, use it; otherwise install the core packages used in the notebook:

```bash
pip install -r requirements.txt  # if present
# or
pip install pandas numpy scikit-learn kagglehub jupyterlab
```

3. Start Jupyter Lab or Notebook from the repository root:

```bash
jupyter lab
# or
jupyter notebook
```

4. Open `digital-impact-on-teen-minds.ipynb` and run the cells in order. If you change upstream variables (e.g., re-run cleaning cells), re-run dependent cells afterwards.

## Notes & tips

- The notebook normalizes the `depression`, `anxiety`, `panic_attack`, and `treatment` columns by mapping common yes/no-like strings to 1/0 and coercing the rest to numeric. If your dataset contains other response categories (e.g., `Sometimes`, `Often`, Likert scales), you may want to map them explicitly or convert to an ordinal scale.

- The RandomForest step expects only numeric features. The notebook performs one-hot encoding and median imputation for missing values before training. If you want a reproducible pipeline, consider using scikit-learn's `ColumnTransformer` and `Pipeline` with explicit train/test splits and cross-validation.

- The dataset is relatively small; treat model results as exploratory rather than definitive. Add cross-validation and performance metrics (AUC, confusion matrix) before drawing firm conclusions.

## Suggested next steps

- Add model evaluation: cross-validation, classification report, ROC-AUC.
- Visualize prevalence and treatment gap using bar charts and stacked bars (matplotlib/plotly/seaborn).
- Add explicit mapping for other categorical response values (e.g., `Sometimes`) or create an ordinal scale.
- Save the cleaned dataset to a CSV for reproducibility (e.g., `df.to_csv('cleaned_student_mental_health.csv', index=False)`).
- Document assumptions and any values coerced to NaN (how many rows lost per column) to support transparency.

## Contact / Author

This notebook and repository were developed during an exploratory analysis of a public Kaggle dataset. If you want improvements or help extending the analysis (visualizations, better preprocessing, model evaluation), open an issue or contact the repository owner.
