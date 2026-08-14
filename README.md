# ai-learning-portfolio
Projects and exercises in Python, Machine Learning, Deep Learning and NLP developed during my AI Master's programme.

## About this repository

This repository documents my learning journey in Artificial Intelligence, combining theoretical knowledge with practical exercises and projects.

## Current areas of study

- Python
- Machine Learning
- Deep Learning
- Natural Language Processing (NLP)

## Featured Project

### [Customer Segmentation with K-Means](./projects/customer-segmentation-kmeans)

An end-to-end customer analytics project using unsupervised machine learning and RFM behavioural modelling to identify actionable customer segments from retail transaction data.

**Highlights:**

- 541,909 original transaction records
- Data quality assessment and cleaning
- RFM feature engineering
- Log transformation and feature standardisation
- K-Means model selection using Inertia and Silhouette Score
- Four interpretable customer segments
- Business-focused profiling and recommendations

A key finding was that **16.44% of customers generated 64.89% of total monetary value**, highlighting a strong concentration of value within the customer base.

**Technologies:** Python, Pandas, NumPy, Scikit-learn, Matplotlib, Google Colab

➡️ [View the complete project](./projects/customer-segmentation-kmeans)


### [Fraud Anomaly Detection](projects/fraud-anomaly-detection)

An unsupervised machine learning project comparing Isolation Forest and Local Outlier Factor for fraud prioritisation in highly imbalanced credit card transaction data.

**Highlights:**
- Compared Isolation Forest and Local Outlier Factor under equivalent alert conditions
- Evaluated precision, recall, F1-score and Average Precision
- Analysed alert-rate sensitivity and false-positive workload
- Validated model behaviour on previously unseen transactions
- Examined business implications, operational trade-offs and ethical limitations

**Key result:** Isolation Forest consistently outperformed Local Outlier Factor and provided substantially stronger fraud prioritisation, while also demonstrating the trade-off between fraud coverage and investigation workload.

**Technologies:** Python, Pandas, NumPy, Scikit-learn, Matplotlib, KaggleHub, Google Colab

[View complete project](projects/fraud-anomaly-detection)
