# V&D Project 1 — Customer Retention Contact List

This repository contains the Case 1 analysis for MKTG 6620. It compares a contract-category rule, logistic regression, and boosted trees for identifying a top-20% customer contact list.

## Selected method

The validation-stage choice was **boosted trees** (`trees`). The choice and reason were recorded in `analysis.md` on September 26, 2026, before the final-test evaluation was run.

## Required files

- `VD1_analysis.py` — instructor-supplied analysis script
- `VD1_requirements.txt` — required package versions
- `VD1_STUDENT_PACK.html` — assignment handout and dataset attribution
- `churn.csv` — instructor-supplied course dataset (obtain from the Week 3 assignment package)
- `analysis.md` — Q1–Q4 analysis, tables, and student-authored interpretations
- `outputs/` — saved validation, split, final-test, interval, calibration, scenario, prediction, and run-record files
- `AI_USE_RECORD.md` — record of AI assistance and relevant exchanges
- One-page memo PDF — added separately before submission

## Dataset source

Use the instructor-supplied `churn.csv` so the analysis uses the assigned extract. It contains 7,043 rows and 21 columns. The handout attributes the public example to the [scikit-learn/churn-prediction dataset mirror](https://huggingface.co/datasets/scikit-learn/churn-prediction), which declared CC BY 4.0 when checked by the instructor on September 9, 2026. A public upload of the CSV is not required.

## Environment

- Python 3.11.3
- NumPy 1.24.2
- pandas 2.0.0
- scikit-learn 1.2.2

Install the supplied requirements in a Python 3.11 environment:

```bash
python -m pip install -r VD1_requirements.txt
```

## Reproduce the analysis

Place the four instructor-supplied files in the same folder. Run the comparison first:

```bash
python VD1_analysis.py compare --csv churn.csv --out outputs
```

Review `outputs/validation.csv`, then record the method choice and reason in `analysis.md` before evaluating the final test. This project recorded `trees` as its choice. Run:

```bash
python VD1_analysis.py evaluate --csv churn.csv --out outputs --choice trees
```

The script uses random seed 0 and records the dataset fingerprint, package versions, split sizes, cleaning rule, and selected method in the JSON run records. The recorded source SHA-256 is:

```text
16320c9c1ec72448db59aa0a26a0b95401046bef5d02fd3aeb906448e3055e91
```

## Main generated outputs

- `outputs/validation.csv` — validation comparison used for model selection
- `outputs/split_rows.csv` — source-row partition assignments
- `outputs/compare_run.json` — comparison environment and run record
- `outputs/test_metrics.csv` — final-test ranking and contact-list metrics
- `outputs/intervals.csv` — AUC and paired-difference bootstrap intervals
- `outputs/probability_groups.csv` — calibration checks for boosted trees
- `outputs/scenarios.csv` — hypothetical value scenarios
- `outputs/test_predictions.csv` — saved final-test predictions and selected-list indicator
- `outputs/evaluate_run.json` — final evaluation environment and run record

The analysis predicts churn risk; it does not estimate whether an offer causes a customer to stay.
