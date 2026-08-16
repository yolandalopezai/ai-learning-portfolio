# Projects

This folder contains selected Artificial Intelligence and data projects developed as part of my technical learning and professional portfolio.

Each project is designed to demonstrate not only the application of analytical or machine learning techniques, but also the reasoning behind the methodology, data preparation, model evaluation and interpretation of results.

## Completed Projects

### [Customer Segmentation with K-Means](./customer-segmentation-kmeans)

Customer segmentation using unsupervised machine learning and RFM behavioural analysis.

The project transforms more than 540,000 retail transaction records into customer-level profiles and applies K-Means clustering to identify four distinct customer segments.

**Key areas demonstrated:**

- Exploratory data analysis
- Data cleaning and quality assessment
- RFM feature engineering
- Distribution transformation and feature scaling
- K-Means clustering
- Cluster selection using Inertia and Silhouette Score
- Customer profiling
- Data visualisation
- Business interpretation and actionable recommendations

**Technologies:** Python, Pandas, NumPy, Scikit-learn, Matplotlib, Google Colab

### [Fraud Anomaly Detection](fraud-anomaly-detection)

An unsupervised machine learning project evaluating anomaly detection for highly imbalanced credit card fraud data.

**Key areas:**
- Data quality and preprocessing
- Extreme class imbalance
- Isolation Forest
- Local Outlier Factor
- Precision, recall, F1-score and Average Precision
- Alert-rate sensitivity analysis
- Validation on unseen data
- Business and ethical implications

**Key finding:** Isolation Forest substantially outperformed Local Outlier Factor in both the initial comparison and unseen-data validation, demonstrating stronger fraud prioritisation while also highlighting the operational trade-off between fraud detection and false-positive workload.

**Technologies:** Python, Pandas, NumPy, Scikit-learn, Matplotlib, KaggleHub, Google Colab
### [Market Basket Analysis with Apriori](market-basket-analysis-apriori)

An association rule mining project focused on discovering product relationships, cross-selling opportunities and multi-product bundle patterns in UK retail transactions.

**Key areas:**
- Transaction cleaning and basket construction
- Apriori frequent itemset mining
- Support, confidence and lift
- Product-to-product association rules
- Bundle opportunity analysis
- Commercial volume vs. association strength
- Seasonality analysis
- Business recommendations

**Key finding:** The analysis identified 92 unique product-pair relationships and several commercially meaningful three-product bundles. Strong patterns emerged within Regency tableware, Jumbo Bags and seasonal Christmas products, while the seasonality check showed that high lift must be interpreted alongside temporal purchasing behaviour.

**Technologies:** Python, Pandas, mlxtend, Apriori, Association Rule Mining, Matplotlib, Google Colab
### [CNN Image Classification with CIFAR-10](./cnn-image-classification-cifar10)

Deep learning image classification project comparing a baseline Convolutional Neural Network with a more regularised architecture using TensorFlow and Keras.

**Key areas:**
- Convolutional Neural Networks
- Image classification
- Data augmentation
- Batch Normalisation
- Dropout regularisation
- Global Average Pooling
- Overfitting analysis
- Class-level evaluation
- Confusion matrix and prediction error analysis

**Key result:** Improved test accuracy from **72.57% to 76.58%** while reducing the model from **356,810 to 141,674 total parameters**. F1-score improved in nine of the ten CIFAR-10 classes.

**Technologies:** Python, TensorFlow, Keras, NumPy, Pandas, Matplotlib, scikit-learn
### [Structured Information Extraction with NLP](./structured-information-extraction-nlp)

Hybrid NLP project for converting unstructured operational text into structured, machine-readable information using spaCy, domain-specific rules and JSON output.

**Key areas:**
- Named Entity Recognition
- spaCy EntityRuler
- Rule-based information extraction
- Schema-level field selection
- Regular expressions
- Error analysis
- Robustness testing
- Independent holdout evaluation
- Structured JSON output

**Key result:** The frozen extraction pipeline achieved **93.75% field-level accuracy on an independent final holdout set**, correctly extracting **45 of 48 fields** across previously unseen operational texts.

**Technologies:** Python, spaCy, EntityRuler, Regular Expressions, Pandas, JSON, Google Colab
---

## Portfolio Development

Future projects will be selected across areas including:

- Machine Learning
- Deep Learning
- Natural Language Processing
- AI evaluation and quality
- Data analysis
- Process automation
- Structured information extraction

The portfolio is intentionally selective: projects are included when they demonstrate a distinct technical capability or a meaningful applied use case.
