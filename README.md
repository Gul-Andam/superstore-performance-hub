# superstore-performance-hub
“Power BI dashboard finding where a global retailer loses money: sales, profit, shipping, returns”
# Superstore Performance Hub: Power BI Sales and Profit Analysis

An interactive Power BI dashboard built on the **Global Superstore** sample dataset (51,290 order lines, 25,728 orders, 4 years of data). The goal: go beyond "how much did we sell?" and find **where the business actually loses money**.

![Dashboard overview](<img width="1162" height="540" alt="dashboard SS" src="https://github.com/user-attachments/assets/5d31538e-e4f1-494b-aab6-7e16ba17ac94" />)

## Business questions

1. Which categories and sub-categories look strong on sales but weak on profit?
2. Which regions and countries lose money, and in which years?
3. How much profit do shipping costs and discounts consume?
4. How many orders are returned, and how much revenue is lost to returns?

## Dashboard pages

| Page | What it shows |
|------|---------------|
| Sales | KPI cards (sales, profit, quantity, orders), sales and profit by market, ship mode and category, trend over time |
| Category | Sales and profit by category and sub-category, ship mode, map by country and category |
| Regions | Regional performance and profit by country |
| Products | Product-level sales and profit |
| Orders | Order-level detail |

Slicers for **Year, Region and Category** and a one-click **Clear all slicers** button control every page.

## Key findings

- **$12.64M in sales, $1.47M in profit (about 11.6% margin)** on 178K units across 25,728 orders.
- **Furniture** brings about 32.5% of sales but only about 19% of profit (roughly 7% margin vs about 14% for Technology and Office Supplies). **Tables** lose money despite $0.76M in sales.
- **Shipping cost is $1.36M, about 93% of total profit.** In the Caribbean, shipping cost exceeds profit.
- **Loss-making pockets:** Central Asia (2013: about -$886 profit on $3.95K sales), Honduras (about -$22.7K), Haiti (about -$2.3K).
- **Same category, opposite results:** in 2014, Office Supplies earned $11.34K profit in Eastern Asia but lost about $91 in Central Asia.
- **Return rate is 4.19%** (1,079 of 25,728 orders), with about $526K in sales attached to returned orders.

> Figures are read from the dashboard and the source data; values marked "about" are rounded.

## Data model

Three tables: `Orders` (fact), `Returns` and `People`, plus a custom calendar table. `Returns[Order ID]` relates to `Orders[Order ID]` (one-to-many).

## DAX highlights

Full list in [`dax/measures.md`](dax/measures.md). A few examples:

```DAX
Total Orders = DISTINCTCOUNT(Orders[Order ID])

Profit Margin % = DIVIDE(SUM(Orders[Profit]), SUM(Orders[Sales]), 0)

Total Discount Given = SUMX(Orders, Orders[Sales] * Orders[Discount])

Days to Ship = DATEDIFF(Orders[Order Date], Orders[Ship Date], DAY)
```

**Lesson learned:** `DIVIDE(SUM(Profit), SUM(Quantity))` and `AVERAGEX(Orders, DIVIDE(Profit, Quantity))` give different answers. The first weights large orders correctly, the second treats every row equally.

## How to open

1. Download `Superstore_Performance_Hub.pbix`
2. Open it in **Power BI Desktop** (free)
3. If prompted, point the data source to `data/global_superstore_2016.xlsx`

## Repository structure

```
superstore-performance-hub/
├── README.md
├── Superstore_Performance_Hub.pbix
├── data/
│   └── global_superstore_2016.xlsx
├── dax/
│   └── measures.md
└── images/
    ├── overview.png
    ├── category-page.png
    └── logo.png
```

## Tools

Power BI Desktop • DAX • Excel

## Notes

The dataset is a public sample dataset used for practice. The numbers do not represent a real company.

## Author

**Gul Andam**: data analyst. Feedback and suggestions are welcome via Issues or LinkedIn.
<img width="1162" height="540" alt="dashboard SS" src="https://github.com/user-attachments/assets/57f0474d-02e2-4c23-b16b-2a74fce83a0e" />
