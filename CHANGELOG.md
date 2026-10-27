# Changelog

## v0.5.0 (Beta)

### ADC Generator
- Multiple pick lists can be uploaded together (or added one after another) and are combined into one ADC.
- New **Order #** column: each row keeps the order it came from, and that's what goes into the export's Order Number column.
- With more than one order loaded, the top Order Number field shows all of them (read-only) and the file is named with every order, e.g. `ADC_Q-734886_Q-730584.xlsx`.
- Grab lot dates and paste-to-fill fill every row with that lot, across all orders.

## v0.4.0 (Beta)

### Shared
- Table columns in both tools can be resized by dragging the dividers in the header (double-click a divider to reset). Hovering a cell shows its full text.

### COO Generator
- Added **Total Box Count**. The number entered fills "No. of Pkgs, Boxes or Cases" on every row and is written into the exported COO.
- Multiple packing slips can be uploaded together (or added one after another) and are combined into one COO.
- Each top-level (bold) part number appears once, even if it repeats within a slip or across slips.
- Descriptions are cut at the first ";" (for example, "Twist EF Library Prep with amp mix, 96 Samples").
- Weight, when entered, is now written into the exported COO too.
- Warns if combined slips have different Ship To addresses.
- Fixed: Clear All now works even when all rows were already deleted.
- Fixed: bold (top-fill) rows are now detected from the actual font, so slips with more bold than regular rows (like Q-730584) parse correctly.
- Fixed: rows on page 2+ of a packing slip are now read.
- Fixed: tall multi-line descriptions no longer bleed into the neighbouring row.
- Fixed: no more double comma in Address Line 2.

## v0.3.0 (Beta)

### ADC Generator
- Added the **Grab lot dates** bookmarklet. Drag it to the bookmarks bar once; on a lot release page, one click sends the dates back to the tool and closes the Confluence tab. If the tab wasn't opened from the tool, it copies the dates so they can be pasted instead.
- Look up now opens Confluence in a single reused tab linked back to the tool.

### Shared
- Submit buttons turn dark once a file is selected, so it's clear they're ready to press.

## v0.2.0 (Beta)

### ADC Generator
- Added a **Look up** button on each row that opens the Confluence QC-space search for that lot.
- Added paste-to-fill: pasting a copied lot release page fills that lot's Manufacturing and Expiration Dates, reading only the Kit Information table so component lots and dates further down the page are ignored.
- Warns if the page's part number doesn't match the row.
- Dates now use YYYY-MM-DD to match Confluence; hand-typed dates in other common formats are converted.
- Shows how many rows have both dates, and asks before exporting with dates missing.

## v0.1.0 (Beta) — first shared release

### ADC Generator
- Parses pick list PDFs for Order Number, Part Number, Quantity and Batch/Lot #.
- Exports an `.xlsx` matching the existing ADC template.
- Manufacturing and Expiration Date are left for manual entry.
- Fixed: Submit silently did nothing because the pdf.js worker URL pointed to `pdf.min.js` instead of `pdf.worker.min.js`.

### COO Generator
- Parses packing slip PDFs for top-level (bold) kit lines and the Ship To address.
- Exports a Certificate of Origin by editing the master Word template directly, keeping the Affidavit of Origin and signature blocks intact.
- Long addresses shrink to fit the form's fixed-size text boxes.
- "For" is fixed to Twist Bioscience.

### Shared
- Upload / Clear / Submit pattern; parsing only runs on Submit.
- Light and dark theme toggle, defaulting to the system setting.
- All processing happens in the browser.

## Planned
- COO: S/S carrier dropdown (FedEx, UPS, DHL, Other) written into the exported document.
- ADC: fully automatic Mfg/Exp lookup via a Confluence API proxy.
- CI Generator.
