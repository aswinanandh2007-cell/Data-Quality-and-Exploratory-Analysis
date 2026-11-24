# Data Quality Report: Customer Churn Sample

**Source:** `data/customer_churn_raw.csv` (15 rows x 11 columns, synthetic)
**Output:** `data/customer_churn_clean.csv` (15 rows x 16 columns: 11 original + 5 derived)

## 1. Audit results

| Check | Result | Action |
|---|---|---|
| Missing values (explicit NaN) | 0 in all 11 columns | None needed; no imputation |
| Hidden missing values ("", N/A, null, unknown, ?) | 0 | None |
| Whitespace / casing inconsistencies | 0 | Standardisation step kept as a guard |
| Exact duplicate rows | 0 | Guard in pipeline, 0 removed |
| Duplicate `CustomerID` | 0 (all match `CUST-####`) | None |
| Data types | Numeric columns correct; 5 text columns are categorical | Converted to `category` (ordered for plan & contract) |
| Outliers (IQR 1.5x and \|z\|>3) | 0 in Age, Tenure, MonthlyCharges, TotalCharges, SupportTickets | Nothing removed |
| `TotalCharges` = `MonthlyCharges` x `TenureMonths` | Holds for all 15 rows | Kept `TotalChargesGap` as reusable check |
| One price per `SubscriptionType` | 49.99 / 79.99 / 149.99 | None |
| Value ranges (Age 18-100, tenure/tickets >= 0, charges > 0) | All pass | None |

**Conclusion:** the file was already clean. I did not invent problems to "fix"; cleaning was limited to standardisation, typing and feature preparation.

## 2. Assumptions & decisions
1. **No outlier removal.** None detected, and with n=15 removing a row would discard 6.7% of the data.
2. **No imputation.** There was nothing missing.
3. **`Churn` encoded** to `ChurnFlag` (1/0); original column retained.
4. **Derived columns:** `AgeBand`, `TenureBand`, `ExpectedTotalCharges`, `TotalChargesGap`, `ChurnFlag`.
5. **Currency** is not stated in the source, so it is left unlabelled.

## 3. Validation (all asserted in the notebook)
Row count preserved (15), `CustomerID` unique, no nulls, ranges valid, charges reconcile within 0.01, churn encoding matches the raw counts (7 Yes / 8 No), payment labels unchanged.

## 4. Initial patterns (hypotheses, not conclusions)
- Overall churn rate **46.7%** (7 of 15); churned customers account for **32.3%** of monthly billing.
- **Contract type:** Month-to-Month 7/7 churned; One Year and Two Year 0/8.
- **Tenure:** churned mean 7.0 months vs 29.1 retained.
- **Support tickets:** churned 3-6 (mean 4.3) vs retained 0-2 (mean 1.0); correlation with churn +0.88.
- **Plan:** Basic 5/7, Pro 2/4, Enterprise 0/4 churned.
- Payment method and gender show no clear pattern.

## 5. Limitations
n=15 is far too small for reliable percentages; predictors are strongly intercorrelated (contract, plan, tenure, age), so no single driver's effect can be isolated; the perfect contract/churn split suggests synthetic data. Validate on a larger real extract before acting.
