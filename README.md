# Pre-Sales POC Dashboard

A self-contained, interactive dashboard for tracking Proof-of-Concept (POC) pipeline health — grouping by stage, duration visibility, and duration-bucket breakdowns.

## What it does

- Groups in-scope POCs into **POC Closed** (Sign Off + Standby) and **POC Open** (Kicked Off + Integration Phase + Assessment/Validation), with stage-level detail nested underneath.
- Tracks **Duration** as POC Validated Date → POC Standby Date (falling back to POC Completed Date, then a manual override, then today's date).
- Breaks POCs into duration buckets: 0–60 / 61–90 / 91+ days, grouped by stage and group.
- Charts the 25 longest-running in-scope POCs, filterable by stage.
- Lets you **replace the data source** with a new `.xlsx` export directly in the browser (no rebuild needed).
- Lets you **edit Validated Date / End Date** per POC via an Edit button — overrides are saved in the browser's local storage.
- Lets you **filter the whole dashboard by month** of either Validated Date or End Date.

## Running it

This is a single static HTML file (`index.html`) with no build step. Open it directly in a browser, or serve it via GitHub Pages / Cloudflare Pages / any static host.

External dependencies are loaded from CDN (Chart.js for charts, SheetJS for reading uploaded `.xlsx` files) — an internet connection is required on first load.

## Data source format

The "Replace Data Source" button expects an `.xlsx` export with these columns: `ID`, `Project Name`, `Customer`, `Project Country`, `City`, `POC Validated Date`, `POC Completed Date`, `POC Standby Date`, `Project Manager`, `BDM`, `Project Stage`, `POC Status`.

## Notes

- Manual date edits and any replaced data source are stored in the browser's local storage, scoped to wherever this page is hosted/opened from. They do not sync across browsers or devices.
- To change which POC Statuses are excluded, or which stages roll up into which group, edit the `EXCLUDED_STATUSES` and `STAGE_TO_GROUP` constants near the top of the `<script>` block in `index.html`.
