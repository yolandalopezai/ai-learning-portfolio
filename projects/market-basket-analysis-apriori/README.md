# Market Basket Analysis with Apriori

An association rule mining project focused on identifying meaningful product relationships, cross-selling opportunities and bundle patterns in transactional retail data.

## Business Objective

Retail transaction data can reveal which products customers tend to purchase together.

This project uses Market Basket Analysis to identify recurring product combinations, evaluate the strength of those relationships using support, confidence and lift, and translate the results into practical merchandising recommendations.

The analysis also distinguishes between:

- strong statistical associations,
- high-volume commercial relationships,
- multi-product bundle opportunities,
- and seasonal purchasing patterns.

## Dataset

The project uses the **Online Retail** dataset from the UCI Machine Learning Repository:

https://archive.ics.uci.edu/dataset/352/online+retail

The original dataset contains:

- 541,909 transaction rows
- 25,900 invoices
- 4,070 product codes
- 38 countries

The available variables include:

- Invoice number
- Product code
- Product description
- Quantity
- Invoice date
- Unit price
- Customer ID
- Country

The raw dataset is not stored in this repository.

## Data Quality and Cleaning

Initial data-quality analysis identified:

- 1,454 missing product descriptions
- 135,080 missing Customer IDs
- 5,268 duplicate rows
- 9,288 cancelled invoice rows
- 10,624 rows with non-positive quantity
- 2,517 rows with non-positive unit price

Customer ID was not required for this analysis because the unit of analysis is the transaction basket rather than the individual customer.

The cleaning process therefore:

- removed exact duplicates,
- excluded cancelled invoices,
- retained only positive quantities,
- retained only positive unit prices.

After cleaning:

- 524,878 transaction rows remained
- 19,960 invoices remained
- 3,922 products remained
- no product descriptions were missing

## Market Selection

The United Kingdom accounted for **90.28% of all cleaned invoices**, with:

- 479,985 transaction rows
- 18,019 invoices
- 3,916 products

The analysis therefore focuses on UK transactions to provide a large and coherent transaction base while avoiding the combination of potentially different purchasing behaviours across markets.

## Basket Structure

UK transaction baskets contained:

- Average unique products per basket: **26.35**
- Median unique products per basket: **15**
- 75% of baskets: **29 products or fewer**
- Single-product baskets: **1,497 (8.31%)**
- Largest basket: **1,110 unique products**

Single-product baskets were retained because they represent genuine transactions. Removing them would alter the transaction population and could artificially increase association metrics.

## Product Scope

A review of unusual product codes identified several invoice lines representing administrative or non-merchandise activity, including:

- postage,
- carriage,
- manual adjustments,
- bank charges,
- marketplace fees,
- bad-debt adjustments,
- samples,
- and gift vouchers.

These entries were excluded because they could create misleading associations between merchandise and transactional charges.

Physical products with non-standard stock-code formats were retained.

After product filtering:

- 478,874 product rows remained
- 17,901 invoices remained
- 3,902 merchandise products remained

## Basket Encoding

The cleaned transactions were transformed into a binary basket matrix:

- **17,901 transaction baskets**
- **3,902 product columns**
- Basket density: **0.68%**

Each value indicates whether a product was present in a transaction, regardless of quantity purchased.

## Minimum Support Selection

Product frequency was examined before running Apriori.

| Minimum Support | Approx. Invoices | Eligible Products |
|---:|---:|---:|
| 0.5% | 90 | 1,561 |
| 1.0% | 179 | 825 |
| 2.0% | 358 | 301 |
| 3.0% | 537 | 133 |
| 5.0% | 895 | 37 |

A **2% minimum support threshold** was selected as a balance between preserving meaningful relationships and avoiding an excessively noisy search space.

## Frequent Itemsets

Apriori produced **412 frequent itemsets**:

| Itemset Length | Number |
|---:|---:|
| 1 product | 301 |
| 2 products | 106 |
| 3 products | 5 |

Association-rule generation produced **242 rules**.

A minimum confidence threshold of **40%** was then used for deeper analysis, leaving **150 stronger rules**.

All of these rules already had lift values above 3.9, indicating that the 2% support threshold had produced a relatively strong set of associations.

## Product-to-Product Rules

Of the 150 selected rules:

- 132 were simple product-to-product relationships
- reciprocal rules were consolidated
- 92 unique product pairs remained

This prevents the same product combination from appearing twice simply because both directional rules exist.

## Strongest Product Associations

Several product families showed particularly strong relationships.

### Scandinavian Christmas Decorations

**Wooden Star Christmas Scandinavian → Wooden Heart Christmas Scandinavian**

- Support: **2.1%**
- Confidence: **76.8%**
- Lift: **27.02**

This was the strongest association identified by lift.

### Regency Tableware

**Pink Regency Teacup and Saucer → Green Regency Teacup and Saucer**

- Support: **3.2%**
- Confidence: **82.1%**
- Lift: **15.76**

**Green Regency Teacup and Saucer → Roses Regency Teacup and Saucer**

- Basket count: **700**
- Support: **3.9%**
- Confidence: **75.1%**
- Lift: **14.06**

### Lunch Boxes

**Dolly Girl Lunch Box → Spaceboy Lunch Box**

- Support: **2.4%**
- Confidence: **60.9%**
- Lift: **15.56**

### Garden Products

**Gardeners Kneeling Pad Cup of Tea → Gardeners Kneeling Pad Keep Calm**

- Basket count: **541**
- Support: **3.0%**
- Confidence: **72.1%**
- Lift: **14.38**

## Highest-Volume Associations

The strongest statistical rule is not necessarily the association with the greatest commercial reach.

The highest-volume unique association was:

**Jumbo Bag Pink Polkadot → Jumbo Bag Red Retrospot**

- Basket count: **785**
- Support: **4.4%**
- Confidence: **67.7%**
- Lift: **6.27**

Other high-volume relationships appeared repeatedly within:

- Jumbo Bag collections
- Lunch Bag collections
- Regency tableware
- Bakelike alarm clocks
- Christmas paper chains

This shows why lift and transaction volume should be considered together when identifying commercial opportunities.

## Three-Product Bundle Opportunities

Five three-product combinations exceeded the 2% support threshold.

The most frequent was:

**Green Regency Teacup and Saucer + Pink Regency Teacup and Saucer + Roses Regency Teacup and Saucer**

- Basket count: **492**
- Support: **2.7%**

Several three-product Jumbo Bag combinations also appeared frequently:

- 405 baskets
- 384 baskets
- 366 baskets

Another Regency combination linked:

- Regency Cakestand 3 Tier
- Green Regency Teacup and Saucer
- Roses Regency Teacup and Saucer

This combination appeared in **359 baskets**.

These patterns provide evidence for testing coordinated bundles and collection-based promotions.

## Association Strength vs. Commercial Volume

Association metrics provide different perspectives.

**Lift** measures how much more frequently two products appear together than would be expected if their purchases were independent.

**Support and basket count** show how widely the relationship occurs across actual transactions.

For commercial decision-making:

- high lift can identify particularly strong cross-selling relationships,
- high basket volume highlights opportunities with greater operational reach,
- combinations with both strong lift and meaningful volume are particularly attractive for recommendation and merchandising strategies.

## Business Recommendations

### 1. Regency Collection Cross-Selling

The Regency product family shows consistently strong relationships.

Product pages could recommend other Regency colours and designs when a customer views or purchases one item from the collection.

### 2. Regency Bundle Testing

The three Regency teacup variants appeared together in 492 baskets.

This supports testing a coordinated three-product bundle or collection promotion.

### 3. Jumbo Bag Cross-Selling

Multiple Jumbo Bag designs appeared among the highest-volume product associations.

These patterns could support:

- frequently bought together recommendations,
- multi-design promotions,
- or volume-based bundle offers.

### 4. Seasonal Christmas Merchandising

The Scandinavian Wooden Star and Wooden Heart products produced the highest lift in the analysis.

The pair is a strong candidate for coordinated seasonal placement and Christmas recommendation strategies.

### 5. Garden Product Pairing

The two Gardeners Kneeling Pads produced a high-confidence, high-lift relationship and appeared together in 541 baskets.

They could be tested as a paired recommendation or bundle.

## Seasonality Analysis

The strongest Christmas association was examined separately to determine whether its high lift represented a stable year-round relationship.

The Wooden Star Christmas Scandinavian and Wooden Heart Christmas Scandinavian appeared together in **368 baskets**.

Monthly distribution:

| Period | Joint Purchases |
|---|---:|
| December 2010 | 15 |
| January 2011 | 1 |
| July 2011 | 3 |
| August 2011 | 2 |
| September 2011 | 42 |
| October 2011 | 102 |
| November 2011 | 178 |
| December 2011 | 25 |

Overall, **98.4% of joint purchases occurred between September and December**.

This demonstrates that the exceptionally high lift of 27.02 reflects a strongly seasonal purchasing pattern rather than a relationship that should be assumed to perform consistently throughout the year.

## Limitations

Several limitations should be considered.

- Association rules identify co-occurrence rather than causality.
- The analysis focuses only on UK transactions.
- The 2% minimum support threshold may exclude meaningful long-tail relationships.
- Product presence is treated as binary and purchased quantity is not incorporated into rule generation.
- Revenue and product margin are not included in the association-ranking criteria.
- Some strong aggregate relationships are highly seasonal.

Association rules should therefore be interpreted alongside commercial context, volume, seasonality and business objectives.

## Next Steps

Potential extensions include:

- Evaluating rule stability across different time periods
- Comparing purchasing behaviour across countries
- Exploring lower support thresholds for niche products
- Incorporating quantity, revenue and margin
- Testing recommendation strategies based on high-confidence rules
- Measuring whether proposed bundles improve conversion or basket value
- Combining association rules with behavioural and temporal recommendation signals

## Technologies

- Python
- Pandas
- mlxtend
- Apriori
- Association Rule Mining
- Matplotlib
- UCI Machine Learning Repository
- Google Colab
- Git / GitHub

## Project Files

- [`market_basket_analysis_apriori.ipynb`](market_basket_analysis_apriori.ipynb) — complete analysis, Apriori modelling, association rules, visualisations and business recommendations
- [`data/README.md`](data/README.md) — dataset source and access documentation

## Conclusion

This project demonstrates how Market Basket Analysis can transform transactional purchasing data into actionable retail insights.

Apriori identified strong relationships across several coordinated product families, including Regency tableware, Jumbo Bags, Lunch Bags and seasonal Christmas products.

The analysis also demonstrated why association quality should not be judged through a single metric. Lift provides insight into relationship strength, while support and transaction volume indicate commercial reach.

Several identified patterns provide evidence for cross-selling, product bundling and recommendation strategies. However, the seasonality analysis also demonstrates that even very strong rules must be interpreted within their temporal and commercial context.

Overall, association rule mining provides a useful decision-support approach for discovering purchasing patterns when statistical relationships are combined with business interpretation rather than treated as automatic recommendations.
