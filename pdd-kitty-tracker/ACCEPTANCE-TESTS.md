# Acceptance test checklist

These checks are intended to be run after `npm install` and `npm run dev` in a browser with real screenshots. Do not claim OCR passed without actually uploading the target images and checking the recognized records.

- [ ] Create a batch named `Batch 1`.
- [ ] Upload two screenshots containing 22 unique items; check OCR results against the screenshots.
- [ ] Confirm duplicate order/item overlap is not counted twice.
- [ ] Rename the batch to `My First PDD Haul`.
- [ ] Rename an item to `White Sneakers` and change its quantity.
- [ ] Set status to `Arrived at China warehouse` and confirm a new timeline event appears.
- [ ] Verify CNY totals and PHP estimates against hand calculations at the saved rate.
- [ ] Export CSV and XLSX and open them to inspect their contents.
- [ ] Export a ZIP backup and inspect that screenshots are present.
- [ ] Refresh the browser and confirm the batch, item edits and shipment events persist.

## Current verification status

The test suite source is included. In the generation environment, dependency installation timed out, so a production build and Vitest execution could not be completed here. The 22-item real-screenshot OCR acceptance test has not been run because no target screenshots were supplied. Treat these checks as unverified until run locally.
