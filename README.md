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

### [Market Basket Analysis with Apriori](projects/market-basket-analysis-apriori)

An association rule mining project focused on identifying product relationships, cross-selling opportunities and multi-product bundle patterns in UK retail transactions.

**Highlights:**
- Cleaned and transformed 17,901 retail transactions into market baskets
- Applied Apriori to identify 412 frequent itemsets and 242 association rules
- Analysed support, confidence and lift across product relationships
- Consolidated reciprocal rules into 92 unique product pairs
- Identified three-product bundle opportunities
- Compared association strength with commercial transaction volume
- Evaluated seasonality in high-lift Christmas product relationships
- Translated association patterns into practical merchandising recommendations

**Key result:** Strong product relationships emerged within Regency tableware, Jumbo Bags, Lunch Bags and seasonal collections. The analysis also showed that high lift alone is not enough: commercial volume and seasonality materially affect how association rules should be interpreted.

**Technologies:** Python, Pandas, mlxtend, Apriori, Association Rule Mining, Matplotlib, Google Colab

[View complete project](projects/market-basket-analysis-apriori)
### [CNN Image Classification with CIFAR-10](./projects/cnn-image-classification-cifar10)

Deep learning image classification project comparing a baseline Convolutional Neural Network with a more regularised TensorFlow/Keras architecture.

**Highlights:**

- 60,000 CIFAR-10 RGB images across 10 balanced classes
- Baseline CNN with clear overfitting analysis
- Improved architecture using data augmentation, Batch Normalisation and Dropout
- Global Average Pooling to reduce model size
- Test accuracy improved from 72.57% to 76.58%
- Macro F1-score improved from 0.7246 to 0.7636
- F1-score improved in 9 of the 10 classes
- Detailed confusion matrix and prediction error analysis

**Key finding:** The improved CNN increased test accuracy by **4.01 percentage points** while reducing the model from **356,810 to 141,674 total parameters**.

**Technologies:** Python, TensorFlow, Keras, NumPy, Pandas, Scikit-learn, Matplotlib, Google Colab

➡️ [View the complete project](./projects/cnn-image-classification-cifar10)
