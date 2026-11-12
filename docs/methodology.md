# Methodology

How the numbers were produced, in the order the work happened.

## 1 · Profile the extract

149,116 rows, 13 columns, 1 January to 30 June 2023. Before anything was built,
the extract was checked for the things that invalidate an analysis later:

| Check | Result |
| --- | --- |
| Nulls or blanks in any column | none |
| Distinct calendar dates vs days in range | 181 of 181 — no missing trading days |
| `transaction_id` uniqueness | 149,116 unique values in the range 1–149,456 (gaps, no duplicates) |
| Store cardinality | 3 (`store_id` 3, 5, 8) |
| Product cardinality | 80 ids, 29 types, 9 categories |
| Revenue reconciliation | row-level `qty × unit_price` sums to $698,812.33, matching every PivotTable total |

## 2 · Enrich in the sheet, not in the report

Two business rules were materialised onto the fact table:

```excel
revenue      =D2*H2
price_tier   =XLOOKUP(G2,'Lookup Table'!$A:$A,'Lookup Table'!$D:$D,"Unkown")
```

`price_tier` resolves through a 98-row reference sheet that bands unit price:

```excel
=IF(B5<=3,"Budget",IF(B5<=8,"Mid","Premium"))
```

The `"Unkown"` fallback in the XLOOKUP is a deliberate tripwire for unmatched
products. It is never hit on this data — all 80 transacted product ids resolve.

**Why here and not in DAX.** Materialising the rules in the sheet keeps the
enrichment visible to anyone who opens the workbook, and it survives being
handed to a colleague who does not use Power BI. The trade-off is that changing
a price band means a workbook edit and a report refresh rather than a measure
edit. For a six-month static extract that is the right side of the trade; for a
live feed it would not be.

Banding by unit price rather than by product also keeps the rule stable when a
product is repriced — the row moves tier on its own. The cost is that a product
sold at several price points can sit in different tiers across its rows; see
[Data quality](#data-quality) below.

## 3 · Aggregate with PivotTables

Five PivotTables over two pivot caches produce the summary sheets — revenue by
month, by store with average order value, by category, by product type with
transaction counts, and by price tier. These are the figures the Power BI
measures were checked against.

## 4 · Project Q3

The six-point monthly revenue series was extended three months with Excel's
exponential smoothing:

```excel
C11   =FORECAST.ETS(A11,$B$5:$B$10,$A$5:$A$10,1,1)
D11   =C11-FORECAST.ETS.CONFINT(A11,$B$5:$B$10,$A$5:$A$10,0.95,1,1)
E11   =C11+FORECAST.ETS.CONFINT(A11,$B$5:$B$10,$A$5:$A$10,0.95,1,1)
```

Arguments are explicit rather than defaulted: seasonality `1` lets Excel detect
a cycle, aggregation `1` averages duplicate timeline points, and the confidence
level is stated at 0.95.

| Month | Forecast | 95% lower | 95% upper |
| --- | ---: | ---: | ---: |
| 2023-07 | $189,406.22 | $163,417.92 | $215,394.53 |
| 2023-08 | $210,499.68 | $177,998.70 | $243,000.66 |
| 2023-09 | $231,593.13 | $193,668.86 | $269,517.40 |

**Stated limitation.** Six monthly observations is below what exponential
smoothing needs to separate trend from seasonality. The band widens to roughly
±16% by September, and the projection assumes the January–June trajectory
continues. Treat it as a planning range, not a committed number.

## 5 · Model and visualise in Power BI

The enriched table is imported as a single flat table. Four calculated columns
supply the time grain (`Month Start`, `Month_Year`, `Hour of Day`, `Day Name`)
and four measures carry the KPIs (`Total Revenue`, `Total Transactions`,
`Avg Order Value`, `Best Month`). See [data-model.md](data-model.md).

Page 1 deliberately carries no slicers so its KPI cards always describe the full
period. Page 2 carries month and store slicers that cross-filter every visual.

## Data quality

The source is clean:

| Check | Result |
| --- | --- |
| Nulls or blanks across all 13 columns, 149,116 rows | **0** |
| Missing trading days in the period | **0** — 181 distinct dates across 181 days |
| Duplicate `transaction_id` | **0** — 149,116 unique ids |
| Transacted products with no price tier | **0** — the `"Unkown"` fallback is never reached |
| Lookup rows inconsistent with the price-band rule | **0** of 98 |
| Row-level revenue vs PivotTable totals | agree to the cent at $698,812.33 |

**One known ambiguity.** `Lookup Table` holds 98 rows covering 80 products — a
product sold at several price points gets a row per price. `XLOOKUP` returns the
first match, so for **15** product ids the row order decides which unit price is
consulted.

For 14 of those the duplicate rows land in the same tier, so the resolved
`price_tier` is identical either way. For **one** — `product_id` 72, listed at
$2.65 (Budget), $3.25 (Mid) and $4.06 (Mid) — the rows disagree, so its tier
depends on sheet order rather than on the price actually paid.

The exposure is bounded: one product of eighty, feeding two visuals and no KPI
card. The correct fix is to band each transaction on its own `unit_price`
instead of a looked-up one, which removes the lookup from the path entirely. It
is recorded in Future enhancements rather than patched silently.

**Cosmetic.** The XLOOKUP's not-found argument is spelled `"Unkown"` in the
workbook. It is never returned, so no output is affected, but it is worth
correcting the next time the sheet is edited.
