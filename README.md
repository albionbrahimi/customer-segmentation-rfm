# Customer Segmentation with RFM and K-means

**Which customers keep an online retailer alive, and how should it treat each group?**

I grouped 4,338 customers of a UK online gift retailer by how **recently**, how **often** and how **much** they buy (RFM), then used K-means clustering to find four segments a marketing team can act on.

![Share of customers vs share of revenue](figures/03_customers_vs_revenue.png)

## Key findings

- **16% of customers bring in 65% of revenue.** The "Champions" order about 14 times a year and spend £8,000 on average.
- **27% of customers are slipping.** They used to order regularly (about 4 orders, £1,800 each) but their last order was about 10 weeks ago. They are the biggest win-back opportunity: 24% of revenue.
- **37% of customers bought once and never came back.** Together they bring only 6% of revenue.
- **19% are new and recent.** They have bought only 1–2 times so far, and the next few weeks decide whether they become regulars.

## Recommendations

| Segment | Customers | Revenue | Action |
|---|---:|---:|---|
| Champions | 16% | 65% | Protect: loyalty perks, early access, personal contact for the biggest accounts |
| Slipping regulars | 27% | 24% | Win-back campaign now, before they are lost |
| New / promising | 19% | 5% | Second-purchase offer to build the habit |
| Lost / one-off | 37% | 6% | Low-cost email only; don't spend ad budget here |

![Average customer in each segment](figures/02_segment_profiles.png)

## Data

[UCI Online Retail dataset](https://archive.ics.uci.edu/dataset/352/online+retail): 541,909 transactions from 1 Dec 2010 to 9 Dec 2011. A compressed copy is in `data/`.

**Cleaning** (541,909 → 392,692 rows):
| Step | Rows left |
|---|---:|
| Raw data | 541,909 |
| Drop rows without CustomerID | 406,829 |
| Drop cancelled invoices ("C…") | 397,924 |
| Drop zero/negative quantity or price | 397,884 |
| Drop exact duplicates | 392,692 |

## Method

1. **RFM table:** one row per customer, with Recency (days since last order), Frequency (number of orders) and Monetary (total spend).
2. **Transform:** spend is extremely skewed (median £670, max £280,000), so I log-transformed all three features and standardised them, so each one counts equally in the distance calculation.
3. **Choosing k:** I compared k = 2 to 8 using the elbow method and silhouette scores. The elbow flattens at k = 4, and four segments are simple enough to act on.
4. **K-means** with k = 4, then named each cluster from its average R, F and M.

![Choosing k](figures/01_choosing_k.png)
![Customers by recency and spend](figures/04_recency_vs_spend.png)

## Limitations

- The data covers only one year, so seasonal customers (for example, only buying for Christmas) may look "lost".
- About 25% of rows have no CustomerID and had to be dropped.
- 90% of customers are in the UK, so the segments mostly describe UK buyers.

## Run it yourself

```bash
pip install -r requirements.txt
python src/segmentation.py          # cleans data, clusters, saves charts to figures/
```
Or open `notebooks/customer_segmentation.ipynb` for a step-by-step walkthrough.

## Project structure

```
data/        raw data (compressed) and output tables
figures/     charts used in this README
notebooks/   step-by-step walkthrough
src/         download_data.py, segmentation.py
```

**Tools:** Python · pandas · scikit-learn · matplotlib · seaborn
