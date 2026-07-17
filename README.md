# Pre-Sales POC Dashboard

A self-contained, interactive dashboard for tracking Proof-of-Concept (POC) pipeline health — grouping by stage, duration visibility, and duration-bucket breakdowns.

## What it does

- Groups in-scope POCs into **POC Closed** (Sign Off + Standby + Booked) and **POC Open** (Kicked Off + Integration Phase + Assessment/Validation), with stage-level detail nested underneath.
- Any POC whose POC Status is "POC Booked" is shown under its own **"POC Booked"** stage everywhere in the dashboard, regardless of its underlying Project Stage — and always rolls up into the POC Closed group.
- Tracks **Duration** as POC Validated Date → POC Standby Date (falling back to POC Completed Date, then a manual override, then today's date).
- Breaks POCs into duration buckets: 0–60 / 61–90 / 91+ days, grouped by stage and group.
- Charts the 25 longest-running in-scope POCs (respects the whole-dashboard filters below).
- Lets you **replace the data source** with a new `.xlsx` export directly in the browser (no rebuild needed).
- Lets you **edit Validated Date / End Date** per POC via an Edit button — overrides are saved in the browser's local storage.
- Lets you **filter the whole dashboard** by month (of Validated Date or End Date), and by Country, PM, BDM, and Stage — each supporting multiple selections at once via a checkbox dropdown.

## Running it

This is a single static HTML file (`index.html`) with no build step. Open it directly in a browser, or serve it via GitHub Pages / Cloudflare Pages / any static host.

External dependencies are loaded from CDN (Chart.js for charts, SheetJS for reading uploaded `.xlsx` files) — an internet connection is required on first load.

## Data source format

The "Replace Data Source" button expects an `.xlsx` export with these columns: `ID`, `Project Name`, `Customer`, `Project Country`, `City`, `POC Validated Date`, `POC Completed Date`, `POC Standby Date`, `Project Manager`, `BDM`, `Project Stage`, `POC Status`.

## Notes

- Manual date edits, filter selections, and any replaced data source are stored in the browser's local storage, scoped to wherever this page is hosted/opened from. They do not sync across browsers or devices.
- To change which POC Statuses are excluded, or which stages roll up into which group, edit the `EXCLUDED_STATUSES` and `STAGE_TO_GROUP` constants near the top of the `<script>` block in `index.html`.
- Each of the Country / PM / BDM / Stage filters defaults to "all selected" (no restriction). Unchecking values narrows the dashboard down to just those; clearing all boxes shows nothing for that dimension.
