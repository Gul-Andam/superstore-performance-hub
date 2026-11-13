# DAX Measures: Superstore Performance Hub

All measures are stored in a dedicated measures table. Format percentages as **Percentage** and money as **Currency** in Measure tools.

## Core KPIs

```DAX
Total Sales = SUM(Orders[Sales])
Total Profit = SUM(Orders[Profit])
Total Quantity = SUM(Orders[Quantity])
Total Shipping Cost = SUM(Orders[Shipping Cost])
Total Orders = DISTINCTCOUNT(Orders[Order ID])
```

`Total Orders` uses DISTINCTCOUNT because one order spans several rows (line items). Do not use COUNTROWS.

## Ratios

```DAX
Profit Margin % = 
DIVIDE(SUM(Orders[Profit]), SUM(Orders[Sales]), 0)

Avg Profit Per Unit = 
DIVIDE(SUM(Orders[Profit]), SUM(Orders[Quantity]), 0)
```

Ratios are summed first and divided once. Averaging row-level ratios with AVERAGEX gives a different, distorted number.

## SUMX (row-by-row calculations)

```DAX
Total Discount Given = 
SUMX(Orders, Orders[Sales] * Orders[Discount])

Gross Sales No Discount = 
SUMX(Orders, Orders[Sales] / (1 - Orders[Discount]))
```

## Returns

```DAX
Returned Orders = DISTINCTCOUNT(Returns[Order ID])

Return Rate % = 
DIVIDE([Returned Orders], [Total Orders], 0)

Returned Revenue = 
CALCULATE(
    SUM(Orders[Sales]),
    FILTER(Orders, RELATED(Returns[Returned]) = "Yes")
)

Returned Revenue % = 
DIVIDE([Returned Revenue], [Total Sales], 0)
```

## Shipping time

Calculated column on `Orders`:

```DAX
Days to Ship = DATEDIFF(Orders[Order Date], Orders[Ship Date], DAY)
```

Measure:

```DAX
Avg Days to Ship = AVERAGE(Orders[Days to Ship])
```

## Orders by date

```DAX
Orders To Date = 
CALCULATE(
    DISTINCTCOUNT(Orders[Order ID]),
    FILTER(
        ALL(Orders[Order Date]),
        Orders[Order Date] <= MAX(Orders[Order Date])
    )
)
```
