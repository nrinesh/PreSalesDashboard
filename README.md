# Pre-Sales POC Dashboard

A self-contained, interactive dashboard for tracking Proof-of-Concept (POC) pipeline health — grouping by stage, duration visibility, and duration-bucket breakdowns.

## What it does

- Groups in-scope POCs into three groups: **POC Closed** (Sign Off + Standby), **POC Open** (Kicked Off + Integration Phase + Assessment/Validation), and **POC Booked** (any POC whose POC Status is "POC Booked", shown under its own "POC Booked" stage regardless of underlying Project Stage).
- Stages are always listed in a fixed order wherever they appear (stage detail table, duration-bucket sub-rows, stage filter): POC Standby, POC Sign Off, POC Kicked Off, POC Integration Phase, POC Assessment/Validation, POC Booked.
- Tracks **Duration** as POC Validated Date → POC Standby Date (falling back to POC Completed Date, then a manual override, then today's date).
- Breaks POCs into duration buckets: 0–60 / 61–90 / 91+ days, grouped by stage and group.
- Charts the 25 longest-running in-scope POCs (respects the whole-dashboard filters below).
- **"Consider Booking Only On/After"** date picker: when a date is set, POC Booked records whose Sales Order Date is earlier than that date (or blank) are removed from the dashboard entirely.
- **Booking Date column** in the POC Detail table: shows the Sales Order Date for POCs whose status is "POC Booked" (blank for all other POCs). The companion workbook mirrors this as a "POC Booking Date" formula column (col T) on the POC Data sheet.
- **Chart-local Stage filter** on the "POC Duration — Top 25 Longest Running" chart: a separate Stage multi-select above the chart that narrows only that chart, independent of the global Filter Whole Dashboard Stage filter. HTML dashboard only (no Excel equivalent).
- **Duration-bucket drill-down**: clicking a Group or Stage row inside any of the "Number of POC by Duration Bucket, Grouped by Group" boxes filters the POC Detail table below to just that duration bucket (+ stage, if a stage row was clicked) and scrolls down to it. Use the "Clear Drill-down" button next to POC Detail, or change the Group Filter dropdown, to clear it. HTML dashboard only (no Excel equivalent).
- **Country filters (widget-local)**: independent Country multi-selects on the "POC Grouping by Group" summary, the "POC Duration — Top 25 Longest Running" chart (alongside its Stage filter), and the "POC Detail" table. Each narrows only its own widget, independent of the global Filter Whole Dashboard Country filter and of each other. HTML dashboard only (no Excel equivalent).
- Lets you **replace the data source** with a new `.xlsx` export directly in the browser (no rebuild needed).
- Lets you **edit Validated Date / End Date** per POC via an Edit button — overrides are saved in the browser's local storage.
- Lets you **filter the whole dashboard** by month (of Validated Date or End Date), and by Country, PM, BDM, and Stage — each supporting multiple selections at once via a checkbox dropdown.

## Running it

This is a single static HTML file (`index.html`) with no build step. Open it directly in a browser, or serve it via GitHub Pages / Cloudflare Pages / any static host.

External dependencies are loaded from CDN (Chart.js for charts, SheetJS for reading uploaded `.xlsx` files) — an internet connection is required on first load.

## Data source format

The "Replace Data Source" button expects an `.xlsx` export with these columns: `ID`, `Project Name`, `Customer`, `Project Country`, `City`, `POC Validated Date`, `POC Completed Date`, `POC Standby Date`, `Sales Oder Date`, `Project Manager`, `BDM`, `Project Stage`, `POC Status`.

## Notes

- Manual date edits, filter selections, the booking-date filter, and any replaced data source are stored in the browser's local storage, scoped to wherever this page is hosted/opened from. They do not sync across browsers or devices.
- To change which POC Statuses are excluded, or which stages roll up into which group, edit the `EXCLUDED_STATUSES` and `STAGE_TO_GROUP` constants near the top of the `<script>` block in `index.html`. Stage display order is controlled by the `STAGE_ORDER` constant right below it.
- Each of the Country / PM / BDM / Stage filters defaults to "all selected" (no restriction). Unchecking values narrows the dashboard down to just those; clearing all boxes shows nothing for that dimension.
- The companion `POC_Dashboard.xlsx` workbook mirrors the same grouping/stage-order/booking-date logic using an editable "Consider Booking Only On/After (date)" cell (Stage Summary!O3).
