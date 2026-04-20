## Project Summary

Primeflix is trying to win back ~500K churned users using three offers: Discount, Free Trial, and Early Access.

Customers are split into Low / Medium / High engagement segments, with a 10% control group.
This project evaluates which offers actually drive incremental resubscriptions and whether results differ by segment.

**Campaign Timeline**

| Month | Phase |
|---|---|
| Month 1 | Advertising - offer communicated to customer |
| Months 2-4 | Promotion active |
| Months 5-7 | Measurement window |

**Analysis Priorities**

1. **Incremental resubscription rate** - how many more customers resubscribed vs the control group during the promo period
2. **Offer performance by segment** - which offer drives the highest resubscription rate, and does this differ by watch history segment
3. **Post-resubscription engagement** - do customers who returned under different offers show different watch behavior in the measurement window

The segmentation is central to this analysis because Primeflix has observed that low, medium, and high engagement customers respond differently to content and pricing incentives. A single overall conversion rate would mask these differences and lead to a one-size-fits-all offer strategy that underperforms.

---

## Key Findings

1. Marketing Effectiveness

The campaign significantly increased resubscriptions across all segments. Resubscription rates improved by 91% to 192% relative to the control group, confirming strong incremental lift from targeted offers.

2. Offer Performance is Segment-Specific

No single offer performs best across all customers. Instead, effectiveness varies by prior engagement:
| Segment | Best Offer              | Lift vs Next Best |
| ------- | ----------------------- | ----------------- |
| High    | Early Access            | +22%              |
| Medium  | Discounted Subscription | +17%              |
| Low     | Free Trial              | +31%              |

This highlights the importance of tailoring offers according to customer behavior - different users respond to different incentives.

## Recommendation
Adopt a segment-based targeting strategy for re-engagement campaigns:

-   High engagement → Early Access
-   Medium engagement → Discounted Subscription
-   Low engagement → Free Trial

This approach maximizes resubscription rates by aligning incentives with customer preferences. Since post-return engagement is unaffected by offer type, optimization should focus purely on conversion lift, not downstream usage.


## Repository Structure

├── data/
│   ├── raw/
│   │   ├── customers.csv
│   │   ├── responses.csv
│   │   └── views.csv
│
├── notebooks/
│   ├── 01_DataSimulation.py        # Generates synthetic dataset
│   ├── 02_EDA.ipynb                # EDA on the generated data
│   ├── 03_Analysis.ipynb           # Core analysis (lift, segmentation, inference)
│   └── 04_Findings.ipynb           # Final results and business insights
│
├── .gitignore
├── requirements.txt
├── README.md

## How to Run

All data in this project is simulated. There is no real customer data.

### 1. Generate Data

The simulation is designed to reflect realistic patterns observed in re-engagement campaigns - conversion rates, watch behavior, and segment differences are grounded in plausible assumptions rather than random noise.

To regenerate the data, run the simulation script from the `notebooks/` folder:
Open in Jupyter `01_DataSimulation.py`

This will write three tables to `data/raw/`:
- `customers.csv` - one row per churned customer (500K rows)
- `responses.csv` - one row per customer with offer assignment and resubscription outcome
- `views.csv` - one row per customer per active viewing day in the measurement window (~2.3M rows)

The ground truth conversion probabilities used in the simulation are documented in the simulation script itself. The analysis notebooks treat these as unknown and attempt to recover them statistically.

Three tables, each representing a distinct stage of the customer journey.

**customers.csv** - one row per churned customer

| Column | Type | Description |
|---|---|---|
| cust_id | int | Unique customer identifier |
| segment | string | Low / Medium / High - based on pre-churn watch hours |
| avg_weekly_hours_pre | float | Average weekly watch hours in 6 months before churning |
| offer | string | Offer assigned - Discount / FreeTrial / EarlyAccess / Control |

**responses.csv** - one row per customer

| Column | Type | Description |
|---|---|---|
| cust_id | int | Links to customers.csv |
| offer | string | Offer received |
| segment | string | Segment - repeated here for convenience |
| resubscribed_promo | int | Did they resubscribe during promo period? (1/0) |
| resubscribed_day | int | Day of resubscription (1–90, null if no resubscription) |

**views.csv** - one row per customer per active day, measurement window only

| Column | Type | Description |
|---|---|---|
| cust_id | int | Links to customers.csv |
| day | int | Day number in measurement window (1–90) |
| hours_watched | float | Hours watched that day |

Only customers with an active subscription in months 5–7 appear in this table - i.e. those who resubscribed and did not immediately churn again.

### 2. Run EDA

This helps the user get an understanding of the simulated data and how it is distributed.

open in Jupyter `02_EDA.ipynb`

### 3. Run Analysis

open in Jupyter `03_Analysis.ipynb`


### 4. Review Findings
open in Jupyter `04_Findings.ipynb`


## Skills Demonstrated
`Experimental Design` · `Causal Inference` · `Segmentation Analysis` · `Logistic Regression` · `Kruskal-Wallis` · `Hurdle Model` · `Python` · `statsmodels` · `scipy` · `Business Translation`