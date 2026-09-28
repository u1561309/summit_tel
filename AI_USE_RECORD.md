# AI-Use Record

## Disclosure

Codex was used from September 26–27, 2026, to read and explain the assignment, set up the required Python environment, run the instructor-supplied script, inspect and verify output files, generate factual calculation tables, and insert student-authored responses into `analysis.md` verbatim.

Codex did not make the validation choice without student approval and did not write or polish the marked student-authored interpretations or decision memo. The student selected boosted trees, supplied the assessed explanations, assigned the Q4 method roles, and recommended a randomized pilot. A factual scaffolding and student-authored passages are labeled in `analysis.md`.

This file is the relevant-exchanges text record requested by the handout because a standalone raw chat-export file was not available through the working interface. It includes unsuccessful technical attempts as well as successful work.

## What AI helped with

- Located and read `VD1_STUDENT_PACK.html`, `VD1_analysis.py`, `VD1_requirements.txt`, and `churn.csv`.
- Verified dataset dimensions, outcome counts, unique IDs, blank-charge treatment, and non-overlapping partitions.
- Recorded the student's `trees` choice and student-written reason before running evaluation.
- Inspected all saved CSV and JSON outputs and independently checked the 15% value calculation.
- Added factual tables and verbatim student-authored passages to `analysis.md`.
- Created the reproducibility instructions in `README.md`.

## Decisions and writing supplied by the student

- Selected `trees` after reviewing validation results.
- Explained that the selected list concentrates more historical churners but does not show whether an offer changes behavior.
- Interpreted final-test AUCs and top-20% churn rates.
- Explained paired bootstrap comparisons and their future-change limitation.
- Interpreted calibration results and the small 80–100% probability group.
- Interpreted the three value scenarios and the 13.54% break-even save rate.
- Assigned the contract rule as a benchmark, logistic regression as a deferred option, and boosted trees as a pilot candidate.
- Identified a save rate below break-even or changed campaign costs as evidence that could alter the recommendation.

## Two checks personally understood

The following checks were explained by the student during the working conversation and were retained in the student's own wording.

### 1. Data separation and the unseen final test

> We received the correct file, where we found 11 customer with zero tenure and set blank charge to 0. Three partition 60-20-20 for train validation and test. Confirmed that the partitions do not overlap split_rows.csv. The final test matters because it verifies that it works on the customoers that it has never seen before.

### 2. Contact-list metric and causal limitation

> The top-20% churn rate answers the contact list question because we can only contact 20% of the customer.

> Churn prediction does not tell us whether the offer will change a customer's mind who is likely to churn otherwise.

## Relevant prompt and exchange record


### Debugging assistance

**Student prompt:**  
“I received a Python package-version error. What does this error mean?”

**AI assistance:**  
Explained the error and suggested checking the required package versions listed in the assignment.


### Assignment review

**Student prompt:** “carefully analyse VD1_STUDENT_PACK.html explain me the assignment like im 5”

**AI assistance:** Read the handout and companion files, explained the three-method comparison, the 60/20/20 workflow, required checks, value scenarios, deliverables, grading, and the course restriction on AI-authored interpretations and the memo.

### Validation comparison

**Student prompt:**  “VALIDATION RESULTS
  method    n      auc  contact_n  top20_churn_rate  top20_mean_prediction  mean_prediction  observed_churn_rate
contract 1409 0.742724        281          0.416370               0.427530         0.258283             0.265436
logistic 1409 0.838402        281          0.612100               0.651202         0.271429             0.265436
   trees 1409 0.845558        281          0.651246               0.662771         0.272776             0.265436
Highest validation AUC: trees
Record a choice and reason in analysis.md before running evaluate.
(base) asmita@MacBookAir VD1_project % 
why The overall validation churn rate was only 26.54%?”, “but we only called 281”, and “how 374?”


**AI assistance:** Explained the difference between the 1,409-row validation population, its 374 churners, and each 281-customer simulated contact list. Clarified that no real customers were called.



### Statistical concept clarification

**Student prompt:**  
“What does it mean if the confidence interval for the AUC difference includes zero?”

**AI assistance:**  
Explained that the observed difference is not sufficiently clear to confidently conclude that one model has better AUC than the other based on that interval.

### Recorded validation choice

**Student-authored reason:**

> 1. AUC was higher
> 2. it showed that trees created more high-risk contact list. from 281 selected customers boosted trees selected 65.12% ~= 183 where as contract rule selected 41.64% ~= 117 customers.
> 3. Churn prediction does not tell us whether the offer will change a customer's mind who is likely to churn otherwise.

**AI action:** Recorded `trees` and the student reason before running the final evaluation.

### Final evaluation

**AI action:** Ran `python VD1_analysis.py evaluate --csv churn.csv --out outputs --choice trees`. Reported all three final-test AUCs and contact-list results, paired intervals, boosted-trees probability groups, value scenarios, and break-even rate.

**Student-authored interpretation:**

> After running final test, AUC observed was, Contract: 0.7373, Logistic: 0.8472, Trees: 0.8497 ie. Boosted Tree was slightly better overall, followed closely by logistic regression. Top 20% churn rates were: Contract: 39.86%, Logistic: 69.40% and Boosted trees: 69.40%. Boosted tree and Logistic showed the same churn rate. Tree vs Logistic interval also included 0 so we cannot confidently rank one better than other. We should be aware that these result do not clearly prove that contacting customers prevent churn we need separate independent test for that. The top-20% churn rate answers the contact list question because we can only contact 20% of the customer.

### Uncertainty

**Student-authored interpretation:**

> Bootstrap sample work by randomly resampling customers from the existing 1409 customers, comparing them on same resampled customer and subtracting their AUC. We do this to fairly compare AUC because different sample create different AUC. Here, tree-vs-contract interval is [0.0959, 0.1311] which is greater than 0, means tree performed better but incase of tree-vs-logistic the interval includes 0 so it cannot show which is better. The limitation is that it does not accounts for the future change.

### Probability accuracy

**Student-authored interpretation:**

> The overall predicted rate was 27.05 and observed rate was 26.54 which is very close, so the model performed well. In top-20% contact list predicted rate was 64.79% and observed rate was 69.40%. Which means, out of 281 customer, 195 churned. Therefore, trees identified high-risk group successfully. Difference in predicted and observed churn rate (69.40 - 64.79 = 4.61%) means that the model under-predicted churn in contact list. But In case of 20-40% group, predicted and observed values are 29.78% and 25.37% which means model overpredicted the risk for this group.
>
> 80-100% group which has 35 customers is a small sample therefore, it is too small to conclude confidently because, churn rate would change significantly only if 2-3 customer had different outcome.
>
> AUC measures whether customers are ranked in the correct order whereas calibration measures whether predicted probabilities match observed frequency. Hence, a model can have strong AUC but imperfect calibration. Therefore, the model ranked customers well but slightly underestimated churn risk in its selected list.

### Value and pilot recommendation

**Student-authored interpretation:**

> At 10% save rate, the campaign loses $1619.93, at 15% the campaign generated positive value of $670.11 and at 20% save rate it generated larger positive value of $2,960.14. Based on 69.40% churn rate break-even rate is 13.54% which means below 13.54% it loses money. At 13.54%, small pilot can be considered and around 20% campaign appear promising and below 13.54% campaign might be risky.
>
> These are hypothetical values, because dataset shows "who churned", it does not show what would happen if those customer had received an offer.
>
> I would suggest to run a randomized pilot to test whether contacting customer truly reduces churn.

### Q4 roles and change evidence

**Student decisions:**

- Contract rule: Benchmark — “Its ranking and contact-list were weakest, so it could be used to judge other models.”
- Logistic regression: Deferred Option — “It found same number of churners as boosted trees, and trees-vs-regression interval included 0 so it could be used as an alternative.”
- Boosted trees: Pilot candidate — “It was the method selected using validation AUC and had the highest final-test AUC.”

**Student-authored change evidence:**

> If observed save rate is below 13.54%, the campaign loses money so it would be pointless. If the campaign cost alters, we may need to re-consider.

## Files checked

- `outputs/compare_run.json` records the required environment and comparison stage.
- `outputs/evaluate_run.json` records `trees` as the evaluation choice and 1,000 valid bootstrap samples.
- `outputs/split_rows.csv` contains all 7,043 source rows exactly once with no duplicate row assignments.
- The 15% boosted-trees scenario was independently recalculated as $670.11 per 1,000 contacts.
