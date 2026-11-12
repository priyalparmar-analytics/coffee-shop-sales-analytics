# Report page exports

This folder holds PNG exports of the two Power BI report pages.

## Expected files

| File | Page |
| --- | --- |
| `page-1-sales-analysis.png` | Page 1 · Sales Analysis |
| `page-2-deep-dive-analysis.png` | Page 2 · Deep Dive Analysis |

## How to export them

1. Open `powerbi/coffee-shop-sales-dashboard.pbix` in Power BI Desktop.
2. Select the page tab you want.
3. Take a screenshot of the report canvas at 100% zoom — or use
   **File → Export → Export report to PDF** and crop each page to PNG.
4. Save into this folder using the file names above.
5. In the root `README.md`, delete the `<!-- DASHBOARD PAGE EXPORTS ... -->`
   comment wrapper. The image markdown inside it is already written and points
   at these paths.

Export at roughly 1600 px wide so the KPI cards stay legible on GitHub.

Page 1 is 1700 × 1000 and page 2 is 1900 × 1300 in report units, so page 2 comes
out taller — that is the page, not the export.
