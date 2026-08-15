# Dataset

## Source

This project uses the **Online Retail** dataset from the UCI Machine Learning Repository.

Dataset page:

https://archive.ics.uci.edu/dataset/352/online+retail

The dataset contains transactional data from a UK-based online retailer and includes information such as:

- Invoice number
- Product code
- Product description
- Quantity
- Invoice date
- Unit price
- Customer ID
- Country

## Why this dataset

The dataset is well suited to Market Basket Analysis because individual invoices contain multiple purchased products, allowing transactions to be transformed into baskets and analysed for recurring product combinations.

The project will use association rule mining to identify products that frequently occur together and evaluate relationships using metrics such as:

- Support
- Confidence
- Lift

## Data Access

The raw dataset is not stored in this repository.

The project notebook will document how the data is loaded, cleaned and transformed into transaction baskets before applying the Apriori algorithm.
