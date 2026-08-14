# Fraud Anomaly Detection

An unsupervised machine learning project focused on detecting unusual patterns in highly imbalanced credit card transaction data and evaluating their relationship with known fraudulent activity.

## Business Objective

Fraud detection presents a particularly difficult machine learning problem because fraudulent transactions represent only a very small proportion of total activity.

This project explores whether unsupervised anomaly detection can help prioritise potentially fraudulent transactions without using fraud labels during model training.

The analysis focuses not only on model performance, but also on the operational trade-off between detecting more fraud and generating additional false-positive investigations.

## Dataset

The project uses the **Credit Card Fraud Detection** dataset available on Kaggle:

https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud

The original dataset contains:

- 284,807 transactions
- 31 variables
- 492 known fraudulent transactions
- Fraud prevalence of approximately 0.17%

The variables `V1` to `V28` are anonymised numerical features. The dataset also includes `Time`, `Amount` and the known fraud label `Class`.

The raw dataset is not stored in this repository.

## Data Preparation

Initial data-quality analysis identified:

- No missing values
- 1,081 exact duplicate rows
- 19 duplicated fraudulent observations

After duplicate removal:

- 283,726 transactions remained
- 283,253 were legitimate
- 473 were fraudulent

The `Class` variable was excluded from model training and retained exclusively as ground truth for evaluation.

The original `Amount` variable showed extreme positive skewness:

- Original skewness: **16.98**
- Log-transformed skewness: **0.16**

A logarithmic transformation was therefore applied and the modelling features were standardised.

The final modelling dataset contained **30 numerical features**:

- `Time`
- `V1` to `V28`
- `LogAmount`

## Modelling Approach

Two unsupervised anomaly detection algorithms were compared:

- **Isolation Forest**
- **Local Outlier Factor (LOF)**

A fixed random sample of **50,000 transactions** was used for the initial model comparison.

The sample contained:

- 49,920 legitimate transactions
- 80 fraudulent transactions
- Fraud prevalence: **0.16%**

Neither model received fraud labels during fitting.

Both models initially used the same expected anomaly rate of **0.2%**, producing 100 alerts each and allowing a direct comparison under equivalent conditions.

## Initial Model Comparison

| Model | True Positives | False Positives | False Negatives | Precision | Recall | F1-score |
|---|---:|---:|---:|---:|---:|---:|
| Isolation Forest | 18 | 82 | 62 | 0.180 | 0.225 | 0.200 |
| Local Outlier Factor | 2 | 98 | 78 | 0.020 | 0.025 | 0.022 |

Under the same alert-rate assumption, **Isolation Forest detected nine times more known fraud cases than LOF**.

The comparison also demonstrated an important distinction:

> Statistical unusualness does not necessarily correspond to fraudulent behaviour.

## Anomaly Overlap

The two algorithms identified substantially different transactions as anomalous.

- 15 transactions were flagged by both models
- 85 were detected only by Isolation Forest
- 85 were detected only by LOF

None of the 15 anomalies identified by both models were known fraud.

All 18 fraud cases identified by Isolation Forest were found exclusively by that model, while the two fraud cases found by LOF were also unique to LOF.

This suggests that the two algorithms capture very different forms of unusual behaviour.

## Characteristics of Detected Anomalies

Additional analysis was used to understand how strongly each group deviated from typical standardised feature values.

| Detection Group | Transactions | Frauds | Fraud Rate | Median Amount | Mean Absolute Z |
|---|---:|---:|---:|---:|---:|
| Both models | 15 | 0 | 0.000% | 1,210.00 | 6.612 |
| Isolation Forest only | 85 | 18 | 21.176% | 240.38 | 3.853 |
| LOF only | 85 | 2 | 2.353% | 4.45 | 0.701 |
| Not flagged | 49,815 | 60 | 0.120% | 22.00 | 0.666 |

The transactions detected exclusively by Isolation Forest showed a particularly strong concentration of known fraud.

Importantly, the most statistically extreme transactions — those detected by both models — were not fraudulent. This reinforces that extreme behaviour alone should not be treated as evidence of fraud.

## Alert-Rate Sensitivity

Fraud monitoring is also an operational capacity problem. Increasing the number of alerts may improve fraud coverage while substantially increasing false-positive investigations.

Isolation Forest was therefore evaluated at several predefined alert rates:

| Alert Rate | Alerts | True Positives | False Positives | Precision | Recall | F1-score |
|---:|---:|---:|---:|---:|---:|---:|
| 0.1% | 50 | 14 | 36 | 0.280 | 0.175 | 0.215 |
| 0.2% | 100 | 18 | 82 | 0.180 | 0.225 | 0.200 |
| 0.5% | 250 | 27 | 223 | 0.108 | 0.338 | 0.164 |
| 1.0% | 500 | 44 | 456 | 0.088 | 0.550 | 0.152 |

At a **0.1% alert rate**, only 50 transactions required investigation and precision reached 28%.

At a **1.0% alert rate**, recall increased to 55%, but 500 transactions required investigation and 456 were false positives.

There is therefore no universally optimal threshold. The appropriate alert rate depends on investigation capacity, financial risk and tolerance for missed fraud.

## Ranking Performance

Average Precision was used to evaluate how effectively each algorithm ranked known fraud toward the most anomalous end of its scoring distribution independently of a single decision threshold.

On the 50,000-transaction comparison sample:

- Fraud prevalence baseline: **0.0016**
- Isolation Forest Average Precision: **0.1030**
- LOF Average Precision: **0.0046**

Isolation Forest therefore ranked fraudulent transactions substantially more effectively across different possible thresholds.

## Detected vs. Missed Fraud

Isolation Forest was particularly effective when fraudulent transactions were strongly different from normal transaction behaviour.

| Fraud Status | Fraud Cases | Median Amount | Mean Absolute Z | Median Max Absolute Z | Median Anomaly Score |
|---|---:|---:|---:|---:|---:|
| Detected fraud | 18 | 73.955 | 5.216 | 21.188 | 0.674 |
| Missed fraud | 62 | 2.110 | 1.775 | 8.015 | 0.560 |

Fraud cases that more closely resembled legitimate activity were considerably harder for the unsupervised model to detect.

## Validation on Unseen Data

A separate robustness check was performed using a **70/30 train-test split**.

The scaler was fitted only on the training data, and both models were trained without fraud labels.

The unseen test set contained:

- 15,000 transactions
- 24 known fraud cases

### Isolation Forest

- True positives: 7
- False positives: 17
- False negatives: 17
- Precision: **0.292**
- Recall: **0.292**
- F1-score: **0.292**

### Local Outlier Factor

- True positives: 0
- False positives: 28
- False negatives: 24
- Precision: **0.000**
- Recall: **0.000**
- F1-score: **0.000**

The model-ranking comparison on unseen data showed the same overall pattern:

- Test fraud prevalence: **0.0016**
- Isolation Forest Average Precision: **0.2894**
- LOF Average Precision: **0.0053**

Because the test set contains only 24 fraud cases, the exact metric values should not be treated as stable estimates of production performance. However, the large difference between the two models provides consistent evidence in favour of Isolation Forest for this dataset.

## Model Selection

**Isolation Forest** was selected as the preferred anomaly detection method.

It consistently outperformed Local Outlier Factor in:

- Precision
- Recall
- F1-score
- Average Precision
- Fraud prioritisation
- Validation on previously unseen transactions

The model should nevertheless be considered a **screening and prioritisation tool**, not a standalone fraud classification system.

## Business Implications

The results illustrate the operational trade-off inherent in fraud monitoring.

A useful anomaly detection system must balance:

- Fraud detection coverage
- False-positive workload
- Available investigation capacity
- Customer friction
- Financial cost of missed fraud

Isolation Forest can help concentrate fraudulent activity within a relatively small set of transactions requiring further review.

In a production environment, alert thresholds should therefore be aligned with business risk and operational capacity rather than selected using model metrics alone.

## Limitations and Ethical Considerations

Several limitations must be considered:

- Fraud represents only a very small proportion of the dataset.
- Evaluation metrics can vary when only a limited number of positive cases are available.
- `V1` to `V28` are anonymised, limiting feature-level business interpretation.
- Statistical anomalies are not necessarily fraudulent.
- Fraudulent transactions that resemble normal behaviour may remain undetected.

The model should not be used as the sole basis for automatically blocking transactions, penalising customers or making irreversible decisions.

A real fraud-monitoring environment should combine anomaly scores with additional behavioural signals, supervised models, business rules and human review.

False positives also have both operational and customer consequences, making responsible threshold management essential.

## Next Steps

Potential extensions include:

- Evaluating performance across multiple train-test splits
- Exploring threshold stability under different investigation capacities
- Comparing anomaly detection with supervised fraud classifiers
- Combining anomaly scores with behavioural and rule-based signals
- Investigating feature-level explanations for individual alerts
- Monitoring transaction behaviour for model drift

## Technologies

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- KaggleHub
- Google Colab
- Git / GitHub

## Project Files

- [`fraud_anomaly_detection.ipynb`](fraud_anomaly_detection.ipynb) — complete analysis, modelling, evaluation and visualisations
- [`data/README.md`](data/README.md) — dataset source and access documentation

## Conclusion

This project demonstrates how unsupervised anomaly detection can support fraud-screening workflows in an extremely imbalanced environment.

Isolation Forest consistently provided stronger fraud prioritisation than Local Outlier Factor, both in the initial model comparison and in validation on previously unseen transactions.

The analysis also showed why anomaly detection must be evaluated beyond accuracy and why model performance cannot be separated from operational considerations such as investigation capacity and false-positive cost.

Most importantly, the project demonstrates that anomaly detection is most valuable as part of a broader decision-support process rather than as an automated substitute for fraud investigation.
