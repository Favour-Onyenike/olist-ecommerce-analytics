# Olist E-Commerce Analytics

**End-to-end marketplace analysis** · Python · MySQL · Power BI  
[Portfolio](https://favour-onyenike.github.io/PORTFOLIO/) · [LinkedIn](https://www.linkedin.com/in/favour-onyenike)

---

## Problem

Olist connects small Brazilian sellers to buyers. Revenue can look strong while **delivery, retention, and seller balance** are weak. This project measures those gaps on ~**99,000** real orders (2016–2018, BRL).

**Goal:** clean multi-table data, store it properly, build a dashboard, and recommend actions tied to evidence.

---

## Business questions

1. How did revenue move over time, by category, and by payment type?  
2. How often are orders late, and does that vary by state?  
3. Do longer waits link to worse review scores?  
4. How concentrated is revenue among sellers?  
5. What should the business prioritise next?

---

## Pipeline

```text
Kaggle CSVs (9 tables)
    → Python (pandas) — clean, join, engineer metrics
    → MySQL — typed tables, keys, validation
    → Power BI — 3-page dashboard + DAX
    → Findings & recommendations
```

| Tool | What I did |
|------|------------|
| **Python** | Dates, `is_late` (0/1), grain-safe aggregates, Portuguese→English category merge |
| **MySQL** | Schema, import, row-count checks |
| **Power BI** | Relationships, date model, KPIs, Overview / Delivery / Sellers pages |

---

## Data (public Olist / Kaggle)

| Metric | Value |
|--------|------:|
| Orders | 99,441 |
| Order items | 112,650 |
| Sellers | 3,095 |
| Products | 32,951 |
| States | 27 |
| Categories (after translation) | ~71 |
| Currency | BRL (R$) |

**Source:** [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)  
Raw files are not in this repo (size + license). Place them in `data/raw/` to reproduce.

**Cleaning highlights**
- Merged `product_category_name_translation` (left join) so categories are English in the model  
- Built `delivery_days`, `delivery_delay_days`, `is_late`  
- Aggregated payments/items at order grain so revenue is not double-counted  
- Used `customer_unique_id` for retention (Olist issues a new `customer_id` per order)

---

## Dashboard

Three pages, slicers, page navigation.

### Overview
KPIs (revenue, orders, late rate, repeat rate) · revenue trend · payment mix · top categories

![Overview page](dashboard/01_overview.png)

### Delivery
Late rate by state · average delay by review score (1 = worst, 5 = best)

![Delivery page](dashboard/02_delivery.png)

**Key visual:** delay by review score  
![Delay by review score](dashboard/03_delay_by_review.png)

### Sellers
Top sellers · concentration · geography

![Sellers page](dashboard/04_sellers.png)

### Model (optional)
Tables and relationships

![Power BI model](dashboard/05_model.png)

---

## Results (from the live build)

| Finding | Number |
|---------|--------|
| Total revenue | ~R$13.6M |
| Late delivery rate | ~6.8% |
| Repeat customer rate | ~3.1% |
| Wait at review score 1 vs 5 | ~17 days vs ~10 days |
| Top seller vs average seller revenue | ~R$459K vs ~R$8.8K |
| Top 10 sellers’ share of revenue | ~13% |

**Takeaway:** volume grew, but **late delivery tracks with worse scores**, **few customers return**, and **revenue is uneven across sellers**.

---

## Recommendations

1. **Scorecard by state and seller** — late rate, reviews, and revenue in one view  
2. **Flag at-risk orders** before the estimated delivery date  
3. **Study top sellers’ fulfilment** and share practices with the long tail  
4. **Pilot retention** for one-time buyers (especially high-review customers)

---

## Example DAX

```dax
Total Revenue = SUM('order_master'[order_value])

Late Delivery Rate =
DIVIDE(
    CALCULATE(COUNTROWS('order_master'), 'order_master'[is_late] = 1),
    CALCULATE(COUNTROWS('order_master'), 'order_master'[order_status] = "delivered"),
    0
)
```

---

## Repo layout

```text
olist-ecommerce-analytics/
├── README.md
├── dashboard/          # screenshots (see list below)
├── notebooks/          # cleaning notebook
├── sql/                # optional schema scripts
├── docs/
└── data/raw/           # Kaggle CSVs (gitignored)
```

---

## Skills shown

Multi-table cleaning · Feature engineering · SQL modelling · DAX / KPIs · Dashboard design · Business recommendations

---

## Contact

**Favour Onyenike** · First-class B.Sc. Computer Science, Baze University  
[Website](https://favour-onyenike.github.io/PORTFOLIO/) · [GitHub](https://github.com/Favour-Onyenike) · [LinkedIn](https://www.linkedin.com/in/favour-onyenike) · [Email](mailto:onyenikefavour8@gmail.com)
