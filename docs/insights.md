# What the data says

Every figure on this page is aggregated from the 149,116 rows of `Raw Data` and
reconciles with the PivotTable totals in the workbook.

## Headline

| KPI | Value |
| --- | ---: |
| Revenue, 1 Jan – 30 Jun 2023 | **$698,812.33** |
| Transactions | **149,116** |
| Units sold | **214,470** |
| Average order value | **$4.69** |
| Best month | **June 2023** ($166,485.88) |
| Top store | **Hell's Kitchen** ($236,511.17) |
| Top product type | **Barista Espresso** ($91,406.20) |

---

## 1 · Revenue more than doubled over the half-year

| Month | Transactions | Revenue | Change |
| --- | ---: | ---: | ---: |
| 2023-01 | 17,314 | $81,677.74 | — |
| 2023-02 | 16,359 | $76,145.19 | −6.8% |
| 2023-03 | 21,229 | $98,834.68 | +29.8% |
| 2023-04 | 25,335 | $118,941.08 | +20.3% |
| 2023-05 | 33,527 | $156,727.76 | +31.8% |
| 2023-06 | 35,352 | $166,485.88 | +6.2% |

Revenue grew **+103.8%** from January to June. February is the only month that
fell, and growth cooled sharply in June (+6.2% after +31.8% in May) — the curve
is flattening, not compounding.

**What drove it.** Transactions grew +104.2% over the same period while average
order value stayed flat. The growth is entirely volume: more visits, not larger
baskets. Nothing in this dataset explains *why* volume rose — there are no
marketing, weather, footfall or price-change fields — so the cause is outside
what can be claimed here.

**The forecast.** The `Forecast` sheet projects Q3 with Excel's `FORECAST.ETS`:

| Month | Forecast | 95% lower | 95% upper |
| --- | ---: | ---: | ---: |
| 2023-07 | $189,406.22 | $163,417.92 | $215,394.53 |
| 2023-08 | $210,499.68 | $177,998.70 | $243,000.66 |
| 2023-09 | $231,593.13 | $193,668.86 | $269,517.40 |

Treat this as an illustration of the method, not a plan number: exponential
smoothing fitted to six monthly points cannot see seasonality, and the band is
already ±16% by September.

---

## 2 · The morning rush is the whole business

| Window | Transactions | Share of all transactions |
| --- | ---: | ---: |
| 07:00 – 10:59 | 67,391 | 45.2% |
| 11:00 – 16:59 | 54,193 | 36.3% |
| 17:00 – 20:59 | 22,938 | 15.4% |
| 06:00 – 06:59 | 4,594 | 3.1% |

10:00 is the single busiest hour (18,545 transactions); 08:00 and 09:00 are
within 5% of it. The afternoon is a long flat plateau at roughly half the
morning rate, and trading effectively stops after 20:00.

Day of week barely matters. The busiest day (Friday, 21,701) is only **5.8%**
above the quietest (Saturday, 20,510) — a weekday commuter pattern rather than a
weekend destination one.

**So what.** Staffing and prep should be scheduled against the clock, not the
calendar. A uniform rota across a 15-hour trading day is mismatched to a demand
curve where 45% of transactions land in four morning hours.

---

## 3 · Two categories carry two-thirds of revenue

| Category | Revenue | Share |
| --- | ---: | ---: |
| Coffee | $269,952.45 | 38.6% |
| Tea | $196,405.95 | 28.1% |
| Bakery | $82,315.64 | 11.8% |
| Drinking Chocolate | $72,416.00 | 10.4% |
| Coffee beans | $40,085.25 | 5.7% |
| Branded | $13,607.00 | 1.9% |
| Loose Tea | $11,213.60 | 1.6% |
| Flavours | $8,408.80 | 1.2% |
| Packaged Chocolate | $4,407.64 | 0.6% |

Coffee and Tea together are **66.7%** of revenue. Barista Espresso alone is
$91,406.20 across 16,403 transactions — the single largest product type.

The retail tail (Coffee beans, Branded, Loose Tea, Flavours, Packaged
Chocolate) is 11.0% of revenue but sells in very small volumes: Premium Beans
turn over $14,583.50 across just 336 transactions. These are high-value,
low-frequency lines, which is a different inventory problem from the drinks menu.

---

## 4 · Premium products punch above their volume

| Tier | Transactions | Share of transactions | Revenue | Share of revenue |
| --- | ---: | ---: | ---: | ---: |
| Budget (≤ $3) | 84,218 | 56.5% | $320,340.75 | 45.8% |
| Mid ($3–$8) | 61,040 | 40.9% | $311,592.89 | 44.6% |
| Premium (> $8) | 3,858 | 2.6% | $66,878.69 | 9.6% |

Premium items are **2.6% of transactions and 9.6% of revenue** — a 3.7× revenue
to volume ratio. Budget runs the other way: 56.5% of transactions for 45.8% of
revenue.

**So what.** Premium is the clearest lever in the dataset. It is small enough
that a modest shift in mix moves the revenue line, and it is already proven to
sell. What the data cannot tell you is margin — there is no cost field — so this
is a revenue argument, not a profit one.

---

## 5 · The three stores are interchangeable

| Store | Transactions | Revenue | Average order value |
| --- | ---: | ---: | ---: |
| Hell's Kitchen | 50,735 | $236,511.17 | $4.66 |
| Astoria | 50,599 | $232,243.91 | $4.59 |
| Lower Manhattan | 47,782 | $230,057.25 | $4.81 |

The spread between the best and worst store is **2.8%** of revenue. Hell's
Kitchen leads on volume; Lower Manhattan has the highest average order value at
$4.81, 4.9% above Astoria, which points to a slightly richer mix rather than
more customers.

There is no underperforming location to fix here. A benchmarking exercise across
these three stores has very little signal to work with — the interesting
variation in this dataset is by hour and by product, not by site.

---

## What this data cannot answer

Worth stating plainly, because the fields simply are not present:

- **No customer identifier** — no repeat-visit rate, no cohort analysis, no
  basket affinity across visits.
- **No cost or margin** — every conclusion above is about revenue, not profit.
- **No staffing, weather, promotion or footfall data** — the growth trend can be
  measured but not attributed.
- **Six months, one half-year** — no year-over-year comparison and no way to
  separate genuine growth from seasonality. The Q3 forecast inherits that limit.
