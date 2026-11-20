# Vahan Market Share Portal

A responsive two-page dashboard built for the supplied Vahan workbook.

## Workbook requirement

Place the Excel workbook in the repository root with this exact filename:

`Vahan Portal Data.xlsx`

The portal reads these worksheet names and columns directly:
- `NEXA RTO Wise MS` — cluster/city/RTO overview, MTD and YTD
- `OEM WIse RTO WIse Market Share` — selected RTO's OEM volumes, market share and change over LY
- `RTO Wise Fuel Wise` — selected RTO's fuel-wise market share

When the workbook is updated, commit/replace the same file in the repository. The portal reads the workbook in the browser and will use the latest committed version on refresh. An open page refreshes itself every five minutes.

## Publish with GitHub Pages

1. Open **Settings → Pages** in this repository.
2. Under **Build and deployment**, choose **Deploy from a branch**.
3. Select branch **main** and folder **/(root)**, then Save.
4. Add `Vahan Portal Data.xlsx` to the repository root (same level as `index.html`).

After Pages finishes publishing, the site will be available at:
https://fbnzm9rzy9-cpu.github.io/Vahan-Market-Share/

## Notes

- Page 1 shows cluster and city summary rows; selecting a city expands its RTO rows. MTD and YTD are shown side by side.
- Page 2 lets the viewer select a city and RTO, then switch between MTD and YTD for OEM-wise and fuel-wise market share.
- The workbook remains the source of truth; do not rename the three worksheet tabs or the workbook file unless the loader is updated.
- This is a static GitHub Pages site. Do not put confidential data in a public repository. GitHub Pages does not provide private dealer login or access control.
