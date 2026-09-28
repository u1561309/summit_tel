# Case 1 — Which Customers Should the Retention Team Contact?

> AI-use note: An AI assistant helped debug python-package version error, run the supplied script, verify the generated files, and produce the factual tables below. The student must write the assessed interpretations, method-choice reasons, and decision memo in their own words.

## Q1 — Generate

### Step 1: Supplied comparison run

The supplied comparison was run without changing `VD1_analysis.py`:

```bash
python VD1_analysis.py compare --csv churn.csv --out outputs
```

The saved run record reports Python 3.11.3, NumPy 1.24.2, pandas 2.0.0, and scikit-learn 1.2.2. It also records split seed 0 and the assigned `churn.csv` SHA-256 fingerprint.

The comparison used the same seven inputs for logistic regression and boosted trees: `tenure`, `MonthlyCharges`, `TotalCharges`, `Contract`, `InternetService`, `PaperlessBilling`, and `PaymentMethod`. The contract rule used training-set churn rates for each contract category as the baseline.

### Data and split verification

| Check | Verified result |
|---|---:|
| Source rows | 7,043 |
| Source columns | 21 |
| Churn = Yes | 1,869 |
| Churn = No | 5,174 |
| Unique customer IDs | Yes |
| Blank `TotalCharges` entries | 11 |
| Blank-charge rows with zero tenure | All 11 |
| Rows removed | 0 |
| Training rows | 4,225 |
| Validation rows | 1,409 |
| Final-test rows | 1,409 |
| Duplicate source rows in `split_rows.csv` | 0 |
| All source rows assigned exactly once | Yes |

The supplied cleaning rule converted the 11 blank `TotalCharges` values to zero. The final-test outcomes have not been evaluated.

### Validation results

| Method | Validation AUC | Contact-list size | Top-20% observed churn rate | Top-20% mean prediction | Overall mean prediction | Overall observed churn rate |
|---|---:|---:|---:|---:|---:|---:|
| Contract rule | 0.742724 | 281 | 41.64% | 42.75% | 25.83% | 26.54% |
| Logistic regression | 0.838417 | 281 | 61.21% | 65.10% | 27.13% | 26.54% |
| Boosted trees | 0.845558 | 281 | 65.12% | 66.28% | 27.28% | 26.54% |

The script reported boosted trees as the method with the highest validation AUC. The difference in validation AUC between boosted trees and logistic regression was approximately 0.00714. These are validation results only, not final-test results.

### Student-authored validation choice — required before evaluation

- **Selected method:** `trees`
- **Date recorded:** September 26, 2026
- **Student's reason (recorded verbatim):**
  1. AUC was higher
  2. it showed that trees created more high-risk contact list. from 281 selected customers boosted trees selected 65.12% ~= 183 where as contract rule selected 41.64% ~= 117 customers.
  3. Churn prediction does not tell us whether the offer will change a customer's mind who is likely to churn otherwise.

## Q2 — Validate the analysis

### Final-test ranking and contact-list results

After the validation choice was recorded, the final evaluation was run with `--choice trees`. The script reconstructed the three methods using the same training rows and settings. In `VD1_analysis.py`, `fit_methods` receives the training-row indexes on line 74; the contract rates use `y[train]` on line 76, and both fitted model pipelines use `df.iloc[train]` and `y[train]` on line 89.

| Method | Final-test AUC | 95% AUC interval | Contact-list size | Churners in list | Top-20% observed churn rate | Top-20% mean prediction | Overall observed churn rate |
|---|---:|---:|---:|---:|---:|---:|---:|
| Contract rule | 0.737265 | [0.716259, 0.755731] | 281 | 112 | 39.86% | 42.75% | 26.54% |
| Logistic regression | 0.847207 | [0.825442, 0.870030] | 281 | 195 | 69.40% | 64.02% | 26.54% |
| Boosted trees | 0.849665 | [0.828267, 0.873164] | 281 | 195 | 69.40% | 64.79% | 26.54% |

The test set contained 1,409 customers, including 374 who churned. Each method's fixed-size contact list contained 281 customers. The final-test outcomes were used for evaluation only, after the `trees` choice was recorded.

### Student-authored data and separation explanation

We received the correct file, where we found 11 customer with zero tenure and set blank charge to 0. Three partition 60-20-20 for train validation and test. Confirmed that the partitions do not overlap split_rows.csv. The final test matters because it verifies that it works on the customoers that it has never seen before.

### Student-authored ranking and contact-list interpretation

After running final test, AUC observed was, Contract: 0.7373, Logistic: 0.8472, Trees: 0.8497 ie. Boosted Tree was slightly better overall, followed closely by logistic regression. Top 20% churn rates were: Contract: 39.86%, Logistic: 69.40% and Boosted trees: 69.40%. Boosted tree and Logistic showed the same churn rate. Tree vs Logistic interval also included 0 so we cannot confidently rank one better than other. We should be aware that these result do not clearly prove that contacting customers prevent churn we need separate independent test for that. The top-20% churn rate answers the contact list question because we can only contact 20% of the customer.

## Q3 — Assess uncertainty and value

### Paired AUC comparisons

The script completed all 1,000 requested bootstrap samples. Each paired comparison used the same resampled test rows for both methods.

| Comparison | AUC difference | 95% paired interval |
|---|---:|---:|
| Logistic minus contract | 0.109942 | [0.093153, 0.128505] |
| Trees minus contract | 0.112400 | [0.095891, 0.131117] |
| Trees minus logistic | 0.002458 | [-0.004875, 0.010193] |

### Student-authored uncertainty interpretation

Bootstrap sample work by randomly resampling customers from the existing 1409 customers, comparing them on same resampled customer and subtracting their AUC. We do this to fairly compare AUC because different sample create different AUC. Here, tree-vs-contract interval is [0.0959, 0.1311] which is greater than 0, means tree performed better but incase of tree-vs-logistic the interval includes 0 so it cannot show which is better. The limitation is that it does not accounts for the future change.

### Probability checks for the selected boosted-trees method

| Test group | Customers | Mean predicted churn | Observed churn | Observed minus predicted |
|---|---:|---:|---:|---:|
| Entire test set | 1,409 | 27.05% | 26.54% | -0.50 percentage points |
| Selected top-20% list | 281 | 64.79% | 69.40% | +4.61 percentage points |
| 0.0–0.2 probability group | 724 | 7.96% | 7.18% | -0.78 percentage points |
| 0.2–0.4 probability group | 272 | 29.78% | 25.37% | -4.41 percentage points |
| 0.4–0.6 probability group | 236 | 49.63% | 52.12% | +2.49 percentage points |
| 0.6–0.8 probability group | 142 | 67.69% | 69.01% | +1.33 percentage points |
| 0.8–1.0 probability group | 35 | 83.45% | 91.43% | +7.98 percentage points |

### Student-authored probability-accuracy interpretation

The overall predicted rate was 27.05 and observed rate was 26.54 which is very close, so the model performed well. In top-20% contact list predicted rate was 64.79% and observed rate was 69.40%. Which means, out of 281 customer, 195 churned. Therefore, trees identified high-risk group successfully. Difference in predicted and observed churn rate (69.40 - 64.79 = 4.61%) means that the model under-predicted churn in contact list. But In case of 20-40% group, predicted and observed values are 29.78% and 25.37% which means model overpredicted the risk for this group.
80-100% group which has 35 customers is a small sample therefore, it is too small to conclude confidently because, churn rate would change significantly only if 2-3 customer had different outcome.
AUC measures whether customers are ranked in the correct order whereas calibration measures whether predicted probabilities match observed frequency. Hence, a model can have strong AUC but imperfect calibration. Therefore, the model ranked customers well but slightly underestimated churn risk in its selected list.

### Hypothetical value scenarios for the selected list

The selected boosted-trees list had a historical final-test churn rate of 69.395%. These are hypothetical planning calculations, not measured savings.

| Assumed save rate | Net value per 1,000 contacts |
|---:|---:|
| 10% | -$1,619.93 |
| 15% | $670.11 |
| 20% | $2,960.14 |

The break-even save rate was 13.54%. One row was independently checked using the 15% assumption:

```text
1,000 × (0.6939501779 × 0.15 × $66 − $6.20) = $670.11
```

### Student-authored value interpretation

At 10% save rate, the campaign loses $1619.93, at 15% the campaign generated positive value of $670.11 and at 20% save rate it generated larger positive value of $2,960.14. Based on 69.40% churn rate break-even rate is 13.54% which means below 13.54% it loses money.  At 13.54%, small pilot can be considered and around 20% campaign appear promising and below 13.54% campaign might be risky.
There values are hypothetical because dataset shows "who churned", it does not show what would happen if those customer had received an offer.

I would suggest to run a randomized pilot to test whether contacting customer truly reduces churn.

## Q4 — Explain your choice

### Required comparison table

| Method | Final-test AUC | ΔAUC vs. contract | 95% interval | Carry forward? | Student-authored reason |
|---|---:|---:|---:|---|---|
| Contract rule | 0.737265 | — | AUC: [0.716259, 0.755731] | Benchmark | Its ranking and contact-list were weakest, so it could be used to judge other models. |
| Logistic regression | 0.847207 | 0.109942 | [0.093153, 0.128505] | Deferred Option | It found same number of churners as boosted trees, and trees-vs-regression interval included 0 so it could be used as an alternative. |
| Boosted trees | 0.849665 | 0.112400 | [0.095891, 0.131117] | Pilot candidate | It was the method selected using validation AUC and had the highest final-test AUC. |

### Student-authored evidence that would change the recommendation

If observed save rate is below 13.54%, the campaign loses money so it would be pointless. If the campaign cost alters, we may need to re-consider.
