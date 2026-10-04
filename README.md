# Olist E-Commerce Analytics

**End-to-end analysis of the Olist Brazilian E-Commerce public dataset**  
Python (pandas) · MySQL · Power BI  

**Author:** Favour Onyenike  
**Type:** Portfolio / course final project  
**Status:** Complete  

---

## 1. Project overview

### Business context
Olist is a Brazilian marketplace network that connects small and medium sellers to customers. It does not hold inventory. Sales volume alone does not guarantee a strong customer experience.

### Goal
Apply a full analytics workflow — clean data, structure it in a database, and build an interactive dashboard — to answer business questions on **revenue**, **delivery performance**, **customer retention**, and **seller concentration**, then recommend practical next steps.

### Business questions
1. What drives revenue over time, by category, and by payment method?  
2. Which sellers and product categories perform best?  
3. How often are deliveries late, and does that change by state?  
4. Is longer delivery linked to lower customer review scores?  
5. What should the business prioritise to improve performance?  

---

## 2. Dataset

| Item | Detail |
|------|--------|
| **Source** | [Kaggle — Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) |
| **Period** | ~2016–2018 |
| **Currency** | Brazilian Real (BRL / R$) |
| **Orders** | 99,441 |
| **Order items** | 112,650 |
| **Sellers** | 3,095 |
| **Products** | 32,951 |
| **States** | 27 (all Brazilian federative units) |
| **Categories** | ~71 English names after translation |
| **Tables** | 9 related CSV files |

### Source tables
1. `olist_orders_dataset`  
2. `olist_order_items_dataset`  
3. `olist_order_payments_dataset`  
4. `olist_order_reviews_dataset`  
5. `olist_customers_dataset`  
6. `olist_products_dataset`  
7. `olist_sellers_dataset`  
8. `olist_geolocation_dataset`  
9. `product_category_name_translation`  

> Raw data is **not** stored in this repository (size + license). Download from Kaggle and place files under `data/raw/`.

---

## 3. Tools & why each was used

| Tool | Role | Why not skip it |
|------|------|------------------|
| **Python (pandas)** | Cleaning, joins, feature engineering | Excel struggles with 9 tables and 100K+ rows; cleaning should be repeatable |
| **MySQL** | Structured storage, types, validation | Enforces schema; mirrors real analytical databases |
| **Power BI** | Interactive 3-page dashboard + DAX | Best layer for exploration and stakeholder communication |

**Pipeline:** Raw CSVs → Python cleaning → MySQL → Power BI dashboard → findings & recommendations

---

## 4. Methodology

### 4.1 Python cleaning
- Loaded all 9 CSV files  
- Converted order/shipping timestamps to datetime  
- **Merged category translation** (Portuguese → English) with a left join on `product_category_name`  
- Aggregated items and payments at the correct grain to avoid double-counting revenue  
- Engineered `delivery_days`, `delivery_delay_days`, and `is_late` (0/1)  
- Exported SQL-ready tables: `order_master`, `item_level`, `customers_clean`, `products_clean`, `sellers_clean`  

### 4.2 MySQL
- Created typed tables and primary keys  
- Imported cleaned CSVs  
- Validated row counts and simple distributions (e.g. `is_late`) before BI  

### 4.3 Power BI
- Built relationships between cleaned tables  
- Created a date dimension / date column for monthly trends  
- Wrote DAX measures (examples below)  
- Designed three report pages with slicers and page navigation  

### Example measures
```dax
Total Revenue = SUM('order_master'[order_value])

Late Delivery Rate =
DIVIDE(
    CALCULATE(COUNTROWS('order_master'), 'order_master'[is_late] = 1),
    CALCULATE(COUNTROWS('order_master'), 'order_master'[order_status] = "delivered"),
    0
)

Repeat Customer Rate =
DIVIDE(
    [Repeat Customers Count],
    DISTINCTCOUNT('order_master'[customer_unique_id]),
    0
)
```
*Note: Repeat rate uses `customer_unique_id` because Olist assigns a new `customer_id` per order.*

---

## 5. Dashboard structure

| Page | Audience need | Main content |
|------|----------------|--------------|
| **Overview** | Leadership summary | KPIs, revenue trend, payment mix, top categories |
| **Delivery** | Operations & experience | Late rate, delay by review score, late rate by state |
| **Sellers** | Marketplace health | Active sellers, top sellers, concentration, geography |

---

## 6. Key findings

| Finding | Evidence (live build) |
|---------|------------------------|
| Revenue grew over the period | ~R$13.59M total; peak around mid-2018 |
| Late delivery hurts ratings | Score 1 ≈ 17 days wait; Score 5 ≈ 10 days |
| Delivery quality varies by state | Late rates differ across 27 states |
| Weak retention | ~3.1% of customers place a second order |
| Seller performance is uneven | Top seller ~R$459K vs avg ~R$8.8K; top 10 ≈ 13% of revenue |

---

## 7. Recommendations

1. **Track delivery by seller and state** — one scorecard for late rate, reviews, and revenue.  
2. **Act before orders go late** — flag at-risk orders using estimated delivery dates.  
3. **Learn from top sellers** — compare fulfilment practices of high performers vs the rest.  
4. **Improve retention** — pilot offers for one-time buyers (especially 4–5 star customers).  

Recommendations are framed for historical 2016–2018 patterns, not as live operational orders for Olist today.

---

## 8. Repository structure

```text
olist-ecommerce-analytics/
├── README.md                 # This file
├── docs/
│   └── PROJECT_SUMMARY.md    # Short business summary
├── notebooks/               # Add your cleaning notebook here
├── sql/                     # Optional schema / check scripts
├── dashboard/               # Screenshots of Power BI pages
│   ├── overview.png
│   ├── delivery.png
│   └── sellers.png
└── data/
    ├── raw/                  # Place Kaggle CSVs here (gitignored)
    └── processed/            # Clean exports (gitignored if large)
```

---

## 9. How to reproduce

1. Download the dataset from Kaggle and put CSVs in `data/raw/`.  
2. Run the Python cleaning notebook (export processed CSVs).  
3. Load processed tables into MySQL.  
4. Connect Power BI to MySQL (or to the processed CSVs).  
5. Recreate measures and the three report pages.  

---

## 10. Skills demonstrated

- Multi-table data cleaning and feature engineering (Python)  
- Relational modeling and validation (MySQL)  
- KPI design and DAX (Power BI)  
- Translating analysis into business recommendations  
- Documentation suitable for portfolio and stakeholder review  

---

## 11. License & attribution

- **Dataset:** Olist Brazilian E-Commerce Public Dataset (Kaggle) — follow Kaggle/Olist terms of use.  
- **This analysis code/docs:** Available for portfolio review; attribute if you reuse the structure.  

---

## Contact

**Favour Onyenike**  
[GitHub](https://github.com/Favour-Onyenike) · [Email](mailto:onyenikefavour8@gmail.com)  

Part of my [Data Analytics Portfolio](https://github.com/Favour-Onyenike/data-analytics-portfolio).
