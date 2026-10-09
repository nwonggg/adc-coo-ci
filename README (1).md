# Twist Shipping Docs Tools (Beta)

Browser-based tools that cut down manual data entry for international shipping paperwork. Upload a PDF, check the pulled-in fields, fill in whatever is still manual, and export a finished document.

Everything runs in your browser. PDFs are read locally and nothing is uploaded to a server, so pick lists, packing slips and customer addresses never leave your machine.

**Live tool:** https://nwonggg.github.io/ *(update this link if the page lives at a sub-path)*

## Status at a glance

| Tool | Status | Input | Output |
|---|---|---|---|
| ADC Generator | Working (beta) | Pick list PDF | ADC `.xlsx` |
| COO Generator | Working (beta) | Packing slip PDF | Certificate of Origin `.docx` |
| CI Generator | Not started | Planned | Commercial Invoice |

## How to use it

Each tool follows the same three-button pattern. **Upload** picks a PDF (you can also drag one onto the drop zone), **Clear** removes it, and **Submit** parses it. Nothing is parsed until you press Submit. After parsing, every field stays editable, and you can add or remove rows before exporting.

Fields highlighted in amber need manual entry. Fields in green were pulled from the PDF and should still be checked before export.

### ADC Generator

Reads a pick list PDF and pulls out the Order Number (Q-number format, such as Q-707584) plus the Part Number, Quantity and Batch/Lot # for every line item. Export produces an `.xlsx` that matches the existing ADC template, named `ADC_<order number>.xlsx`.

**Filling Manufacturing and Expiration Dates:** each row has a **Look up** button that opens a Confluence search for that lot in the QC space. Open the lot release page, press Ctrl+A then Ctrl+C (Cmd on Mac), come back to the tool and paste, either into the "Fill dates from Confluence" box or anywhere on the page. The tool reads the Lot Number, Manufacturing Date and Expiration Date from the Kit Information table and fills the matching row. Rows can be done in any order. Dates use Confluence's format, YYYY-MM-DD, and dates typed by hand in other common formats are converted automatically. Export asks for confirmation if any row is still missing a date.

**Faster option, the Grab lot dates bookmark:** drag the green **Grab lot dates** button from the tool onto your bookmarks bar once (Cmd+Shift+B shows the bar). After that, click **Look up**, open the lot release page, and click the bookmark. The dates go straight into the tool and the Confluence tab closes. If you opened the page some other way, the bookmark copies the dates instead, and you paste them into the tool.

You need your own Confluence access to open the lot release pages; the tool itself never connects to Confluence.

### COO Generator

Reads a packing slip PDF and pulls out only the top-level kit lines (the bold rows) as Marks and Numbers, along with their descriptions and the Ship To address. Phone and email lines are dropped from the address automatically. The Date field defaults to today.

Export fills in the real master COO Word template, so the Affidavit of Origin and signature blocks are preserved exactly. Long addresses are set in a smaller font so they fit the fixed-size boxes on the form. The file is named `COO_<order number>.docx`.

**Still manual:** The Undersigned, Shipped on S/S, No. of Pkgs, and Weight. The S/S (carrier) value is not written into the Word file yet, so fill it in on the exported document for now.

## Known limitations and work in progress

- **S/S carrier dropdown (COO):** will become a dropdown with FedEx, UPS, DHL and an Other option for freight forwarders, and the chosen value will be written into the exported document.
- **Mfg/Exp dates (ADC):** currently one Look up and paste per lot. A fully automatic lookup would need a small server-side proxy with a Confluence API token.
- **CI Generator:** Commercial Invoice tool, planned as the third section on the page.
- **PDF format sensitivity:** parsing relies on the current layout of the pick list and packing slip PDFs. If a document template changes, rows may stop being detected. The tool will show a warning, and rows can still be added by hand.

## Reporting problems

If a PDF doesn't parse correctly, note the order number and what was missing or wrong, then open an issue in this repo or message the maintainer. Please don't attach customer PDFs to public issues.

## Technical notes

The whole tool is a single self-contained `index.html` with no build step and no server. It loads three libraries from cdnjs: pdf.js 3.11.174 for reading PDFs, SheetJS 0.18.5 for writing Excel files, and JSZip 3.10.1 for editing the Word template. The master COO template is embedded in the page as base64.

To run it locally, open `index.html` in a browser. To deploy, push to the `main` branch of the GitHub Pages repo.

See [CHANGELOG.md](CHANGELOG.md) for version history.
