# PawParcel — PDD Kitty Tracker

A cute, local-first Pinduoduo order tracker built with React, TypeScript and Vite. It supports batch organization, multiple-image uploads, in-browser OCR, editable orders, shipment timelines, CNY→PHP estimates, and CSV/XLSX/JSON/PDF/ZIP exports.

## Requirements
- Node.js 20+ and npm
- Modern browser (Chrome, Safari, Firefox or Edge)

## Install and run
```bash
npm install
npm run dev
```
Open the local URL printed by Vite. To create a production build:
```bash
npm run build
npm run preview
```
Run automated calculation/consolidation tests with `npm test`.


## Deploy to Cloudflare Pages (important)

**Do not upload this source folder directly using the Pages “Upload and deploy” static-file uploader.** This project contains TypeScript/TSX and must be built first; that uploader cannot compile it.

### Recommended: Cloudflare Pages with Git
1. Push this project to a GitHub/GitLab repository.
2. In Cloudflare, open **Workers & Pages → Create → Pages → Connect to Git**.
3. Set **Build command** to `npm run build`.
4. Set **Build output directory** to `dist`.
5. Deploy. Cloudflare will install dependencies and build the TypeScript/React app.

### Deploy from your computer with Wrangler
Run these commands from the project directory (Node.js 20+ required):

```bash
npm install
npm run build
npx wrangler pages deploy dist --project-name pdd-kitty-tracker
```

The first Wrangler run may ask you to log in and create the Pages project. Deploy the **`dist` output**, not the source files. If you prefer the Cloudflare dashboard's direct upload, run `npm run build` first, then upload the **contents of `dist/`**. If `dist/` does not exist, the build has not completed successfully.

Cloudflare Pages serves the built frontend only. This project is local-first and does not require a backend for its current basic features. Optional live OCR vision, exchange-rate API, or carrier tracking services would need separate server-side configuration.

## How data is stored
PawParcel saves app data in `localStorage` on the current browser/device. Screenshot data URLs are included in local app state and JSON/ZIP backups. This is local-only; it does not sync between devices. Back up often using **Data Export & Backup**. Clearing site/browser data can erase your records.

## OCR behavior and limitations
The screenshot uploader accepts JPG/JPEG, PNG and WEBP, multiple images at once. Tesseract.js runs OCR in the browser using `chi_sim+eng`. The app automatically saves candidate records and flags uncertain fields. OCR extraction is heuristic: Pinduoduo screenshot layouts vary, and the parser cannot guarantee accurate order/item segmentation or exact prices. Always review flagged fields and totals. For production-grade visual item recognition, configure a secure backend endpoint using a vision model; never put a private API key in frontend code. The current project intentionally does not pretend to include a paid vision API.

## Currency
The default exchange rate is an explicitly labeled editable placeholder, not a live quote. Set a rate in **Currency Settings**. PHP values are estimates calculated as CNY × saved rate. Live exchange-rate fetching is not configured in this build.

## Shipment tracking
Statuses and event timelines are editable manually. The app does not claim live carrier tracking. Add tracking number and carrier to an item and manually record updates. A live tracking integration would require an account/API and a secure server-side proxy.

## Exports
- CSV: flat item/order rows
- XLSX: Orders and Batches worksheets
- JSON: complete backup, including batches, items, settings and screenshot data URLs
- PDF: printable item report
- ZIP: JSON, CSV and uploaded screenshots

## Security / privacy
No login or cloud backend is included. Data remains in this browser. Do not use a shared device without understanding that local browser storage is accessible to anyone using the same browser profile. Do not add secrets to `.env` variables exposed to Vite client code.

## Project structure
```
src/App.tsx       Main UI and local workflows
src/model.ts      Data types, statuses and calculations
src/style.css     Responsive cat-themed UI
src/main.tsx      React entry point
tests/model.test.ts  Calculation and duplicate tests
```

## Acceptance test notes
The code includes automated tests for item subtotals, duplicate merging, same-order distinct items, batch totals and screenshot-independent calculations. A real acceptance run with 22 unique records requires actual Pinduoduo screenshots and must be done in the target browser; this repository does not claim that specific OCR accuracy test has passed. Export actions are implemented in-app and require browser download permission.
