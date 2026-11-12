# Data dictionary

## Provenance

`coffee-shop-sales.xlsx` holds the full dataset. It covers a three-store coffee
retailer in New York City trading as **Maven Roasters**, and contains no
customer, staff or payment details.

| Property | Value |
| --- | --- |
| Rows | 149,116 transaction lines |
| Columns | 13 |
| Period | 1 January 2023 – 30 June 2023 (181 days, no gaps) |
| Stores | Astoria, Hell's Kitchen, Lower Manhattan |
| Products | 80 distinct `product_id` values across 29 product types and 9 categories |
| Units sold | 214,470 |
| Revenue | $698,812.33 |
| Currency | USD |
| Grain | one row per product line on a transaction |

## Workbook sheets

| Sheet | Role |
| --- | --- |
| `Raw Data` | The fact table. Columns A–M, 149,116 data rows. |
| `Lookup Table` | Price-band reference for the `price_tier` column. 98 rows. |
| `Revenue Overview` | PivotTables: revenue by month, and revenue and average order value by store. |
| `Product Analysis` | PivotTables: category revenue mix, and revenue and transaction count by product type. |
| `Price Tier Analysis` | PivotTable: transactions and revenue by price tier. |
| `Forecast` | The monthly revenue series plus a `FORECAST.ETS` projection for Jul–Sep 2023 with 95% bounds, and the chart built on it. |

## Data dictionary — `Raw Data`

| # | Column | Type | Source | Notes |
| --- | --- | --- | --- | --- |
| A | `transaction_id` | integer | source | Unique per row; 1 – 149,456 with gaps. |
| B | `transaction_date` | date | source | Excel serial, 1900 date system. |
| C | `transaction_time` | time | source | Stored as a fraction of a day. |
| D | `transaction_qty` | integer | source | Units on the line. |
| E | `store_id` | integer | source | 3, 5, 8. |
| F | `store_location` | text | source | Astoria, Hell's Kitchen, Lower Manhattan. |
| G | `product_id` | integer | source | Joins to `Lookup Table`. 80 distinct values in the fact table. |
| H | `unit_price` | decimal | source | USD, price at time of sale. |
| I | `revenue` | decimal | **derived** | `=D2*H2`. |
| J | `product_category` | text | source | 9 values, led by Coffee and Tea. |
| K | `product_type` | text | source | 29 values, e.g. Barista Espresso, Brewed Chai tea. |
| L | `product_detail` | text | source | 80 values, the sellable SKU. |
| M | `price_tier` | text | **derived** | `=XLOOKUP(G2,'Lookup Table'!$A:$A,'Lookup Table'!$D:$D,"Unkown")`. |

## Data dictionary — `Lookup Table`

| Column | Type | Notes |
| --- | --- | --- |
| `Product ID` | integer | Not unique: 98 rows cover 80 products. |
| `Unit Price` | decimal | One row per product and price point. |
| `Product Type` | text | Descriptive only. |
| `Price Tier` | text | `=IF(price<=3,"Budget",IF(price<=8,"Mid","Premium"))`. |

## Known data-quality characteristics

Full discussion in [`../docs/methodology.md`](../docs/methodology.md#data-quality).

- No nulls, and every transacted `product_id` resolves to a price tier — the
  `"Unkown"` fallback in the XLOOKUP is never reached.
- The lookup table lists 15 products at more than one unit price. `XLOOKUP`
  returns the first match, so the tier is decided by row order.
- For exactly one product (`product_id` 72) those rows disagree about the tier,
  which makes its classification order-dependent.
- Every lookup row is internally consistent with the stated price-band rule.
