## Problem Statement

Primeflix, a streaming company, has approximately 500,000 customers who unsubscribed in the past 90 days. To win these customers back, Primeflix runs a re-engagement campaign testing three offers:

- **Discounted Subscription** - 20% off for 3 months
- **Extended Free Trial** - 3 months free on resubscription
- **Early Access** - exclusive content access for 3 months

10% of churned customers are held out as a pure control group — no offer, no marketing. The remaining 90% are randomly assigned one of the three offers in equal splits, blocked by customer segment.

Customers are segmented by average weekly watch hours in the 6 months prior to churning - **Low** (50%), **Medium** (35%), and **High** (15%). This unequal split reflects the natural composition of the churned population. Each segment receives all three offers in equal proportion.

**Campaign Timeline**

| Month | Phase |
|---|---|
| Month 1 | Advertising — offer communicated to customer |
| Months 2-4 | Promotion active |
| Months 5-7 | Measurement window |

**Analysis Priorities**

1. **Incremental resubscription rate** - how many more customers resubscribed vs the control group during the promo period
2. **Offer performance by segment** - which offer drives the highest resubscription rate, and does this differ by watch history segment
3. **Post-resubscription engagement** - do customers who returned under different offers show different watch behavior in the measurement window

The segmentation is central to this analysis because Primeflix has observed that low, medium, and high engagement customers respond differently to content and pricing incentives. A single overall conversion rate would mask these differences and lead to a one-size-fits-all offer strategy that underperforms.

---

## Data Structure

Three tables, each representing a distinct stage of the customer journey.

**customers.csv** - one row per churned customer

| Column | Type | Description |
|---|---|---|
| cust_id | int | Unique customer identifier |
| segment | string | Low / Medium / High — based on pre-churn watch hours |
| avg_weekly_hours_pre | float | Average weekly watch hours in 6 months before churning |
| offer | string | Offer assigned — Discount / FreeTrial / EarlyAccess / Control |

**responses.csv** — one row per customer

| Column | Type | Description |
|---|---|---|
| cust_id | int | Links to customers.csv |
| offer | string | Offer received |
| segment | string | Segment - repeated here for convenience |
| resubscribed_promo | int | Did they resubscribe during promo period? (1/0) |
| resubscribed_day | int | Day of resubscription (1–90, null if no resubscription) |

**views.csv** — one row per customer per active day, measurement window only

| Column | Type | Description |
|---|---|---|
| cust_id | int | Links to customers.csv |
| day | int | Day number in measurement window (1–90) |
| hours_watched | float | Hours watched that day |

Only customers with an active subscription in months 5–7 appear in this table - i.e. those who resubscribed and did not immediately churn again.

---

