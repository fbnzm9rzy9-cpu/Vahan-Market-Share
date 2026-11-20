# Vahan Market Share Portal

A responsive, two-page NEXA market-share dashboard for GitHub Pages.

## Required workbook

Upload the Excel workbook to the repository root using this exact filename:

`Vahan Portal Data.xlsx`

It must contain these worksheet tabs (spelling and spaces matter):

- `NEXA RTO Wise MS` — cluster, city and RTO overview with November MTD and Till November YTD data.
- `OEM WIse RTO WIse Market Share` — OEM-wise volume, market share and change over last year.
- `RTO Wise Fuel Wise` — CNG, Diesel, EV, Petrol and Total fuel-wise market share.

The portal reads the Excel workbook directly in the browser using SheetJS. It does not require a manual JSON conversion or code edits when figures change.

## Updating the portal data

1. Open this repository on GitHub.
2. Upload/replace `Vahan Portal Data.xlsx` in the repository root (the same folder as `index.html`).
3. Commit the change to the `main` branch.
4. Refresh the portal. An already-open portal checks for a new workbook every five minutes.

Keep the workbook filename and worksheet tab names unchanged. If the workbook is renamed or the worksheet names change, update the loader in `index.html` as well.

## Dashboard pages

### Page 1 — Vahan Market Share
- Cluster and city summary in a compact table.
- November MTD and Till November YTD displayed side by side.
- Click a city row to expand its RTO-level figures.
- The Detailed View button opens Page 2.

### Page 2 — Detailed View
- Select a city and then an RTO.
- OEM-wise volume, market share and change over LY from Tab 2.
- MTD/YTD switch applies to both OEM and fuel-wise sections.
- Fuel-wise market share for CNG, Diesel, EV, Petrol and Total from Tab 3.

## GitHub Pages

In **Settings → Pages**, select **Deploy from a branch**, branch `main`, folder `/(root)`. The expected URL is:

https://fbnzm9rzy9-cpu.github.io/Vahan-Market-Share/

## Important access note

This repository is public and GitHub Pages is a static website. Do not commit confidential or dealer-restricted data. GitHub Pages does not provide private dealer login or password-based access control.
