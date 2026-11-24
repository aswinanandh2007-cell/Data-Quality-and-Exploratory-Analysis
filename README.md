# Customer Churn: Data Quality & Exploratory Analysis

Audit and clean a customer churn dataset, then document quality decisions and initial patterns.

## Headline findings
- The raw file was already clean: **0 missing values, 0 duplicates, 0 outliers**, and `TotalCharges` reconciles exactly with `MonthlyCharges x TenureMonths`.
- Overall churn: **46.7% (7 of 15)**.
- Strongest signals: **Month-to-Month contract** (7/7 churned vs 0/8 on longer contracts), **short tenure**, and **3+ support tickets**.
- Caveat: only 15 rows and heavily intercorrelated variables, so these are hypotheses to test, not conclusions.

## Repository contents
| File | Purpose |
|---|---|
| `churn_data_quality_eda.ipynb` | Full executed notebook: profiling, cleaning, validation, statistics, charts |
| `churn_data_quality_eda.html` | Same notebook as a static page (readable without Jupyter) |
| `data_quality_report.md` | Short written summary of checks, assumptions, findings, limitations |
| `data/customer_churn_raw.csv` | Original supplied dataset |
| `data/customer_churn_clean.csv` | Cleaned, analysis-ready dataset (portfolio upgrade) |
| `data/data_dictionary.csv` | Column definitions (portfolio upgrade) |
| `figures/` | Exported charts |
| `requirements.txt` | Python dependencies |

## Reproduce
```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace churn_data_quality_eda.ipynb
```

## Method
1. **Profile:** dtypes, missing (explicit and placeholder), duplicates, IQR/z-score outliers, business-rule checks.
2. **Clean:** standardise text, typed/ordered categories, binary churn flag, derived bands; duplicate guard; no imputation or removal because nothing warranted it.
3. **Validate:** assertions on row count, uniqueness, nulls, ranges, charge reconciliation, churn encoding.
4. **Explore:** summary stats, distributions, churn by segment, correlation heatmap.
