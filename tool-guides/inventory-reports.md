# Inventory Reports

Inventory Reports puts the three monthly Autotask inventory reports Accounting reconciles — **On Hand**, **Added**, and **Picked** — directly in the Hub, read live from Autotask, with an Excel export in the same layout as the reports you used to pull from Autotask by hand.

## Who sees it

Finance and admin roles. Grants are managed in Access Management.

## Picking a month

The month picker at the top right defaults to the most recently closed month. Month boundaries are Denver time.

The chip beside it tells you what the numbers are:

* **Live** — the current, open month. Figures are as of right now.
* **Live (month closed, no snapshot)** — a closed month. Added and Picked are recomputed from dates and are reliable. **On Hand is today's stock, not that month's** — Autotask only keeps current stock, and month-end snapshots arrive in Phase 2. A banner on the On Hand tab says so.

All three tabs load at once; a tab shows a small spinner until its own data is in. Autotask can take up to a minute.

If any Autotask lookup fails, the tab shows an error with a Retry button instead of a table — a partial result would read as a real balance. If the Hub's settings service cannot be reached, On Hand and Picked still load but show an amber **Using default settings** banner, because the excluded-location list and lot-match window in force may not be the ones you saved.

## On Hand

Every stocked item with available units above zero, outside the excluded locations (see Settings). Columns: Product Name, Serial Number, On Hand, **Unit Cost (per unit)**, **Ext. Cost (units × unit cost)**, Location, Added Date.

> In the old LiveReport export, the column labelled "Unit Cost" was actually the extended cost. The Hub shows both, labelled correctly; the Excel export keeps the old header for continuity (see Export).

Rows with a blank product name or a $0.00 cost are flagged inline and counted in the **Flags** tile — they are shown, not hidden.

## Added

Stocked items created in the month, every location. **Quantity Added is units received** — an item added and picked in the same month still counts. Rows created by a transfer between locations are excluded so a move is not counted as a purchase. Extra columns: Location, Vendor, and Vendor Invoice #, straight from Autotask, to help match against QuickBooks.

## Picked

Inventory that left stock against a charge in the month. Each row shows the charge's Date Charged, Account, Ticket or Project, Quantity, Total Cost, Unit Cost, Unit Price, and Sold By, plus:

* **Match** — *Exact* means a serialized item whose charge is recorded on the item itself. *By product/timing* means a non-serialized lot (cables, adapters, drives): Autotask does not record which charge a lot pick went to, so the Hub matches the charge to a lot of the same product picked within the lot-match window (15 minutes by default).
* **Stock Unit Cost** — the stocked item's own cost, highlighted when it differs from the charge's Unit Cost.

Each serialized unit is its own row with Quantity 1 and Total Cost equal to its unit cost — the same layout the old report used, so a charge for three laptops appears as three rows that add up to the charge. A non-serialized lot pick is one row per charge, with the charge's quantity and total.

### Needs review

Under the table, anything that did not line up. The first three groups are outside the totals; cost differences are counted, at the charge's cost, and listed here so the difference gets a look:

* **Charge for a stocked product with no matching pick** — usually a drop-ship.
* **Left stock with no matching charge** — worth a look.
* **Pick and charge dated in different months** — the old report dropped these silently; the Hub lists them.
* **Charge cost differs from stocked cost** — counted at the charge's cost; the difference is what to check.

## Export to Excel

Each tab exports one workbook named `MM-YY - <Report Name>.xlsx`, matching the old file names so it drops into the existing SharePoint folder. The first columns are the old report's columns, in the old order, under the old headers — including "Unit Cost" carrying the extended amount on On Hand and Added — with the Hub's extra columns to the right. The totals row uses real `SUM` formulas. The Picked export also carries an **Ext. Price** column (quantity × unit price) on the right, so the Total Price shown on screen is in the file too. Money cells keep the same precision Autotask holds, so the workbook's totals match the old report's to the penny. A Picked export carries its Needs-review rows on a second sheet, never among the data rows.

## Settings (gear icon)

* **Excluded from On Hand** — which Autotask inventory locations are left out of the On Hand report (seeded with the seven the old report excluded: Retired/RMA'd Datto, Retired/RMA'd Calyptix, Lost Items, Loaner Items, and the three Shelf Spare locations). Added always includes every location.
* **Lot-match window** — minutes between a lot pick and a charge for them to count as the same event. Wider matches more and mis-matches more; unmatched rows go to Needs review either way.
* **Month-end snapshot** — arrives in Phase 2; until then this section is a note.

## Coming in Phase 2

A scheduled month-end snapshot on the 1st, per-row Checked marks and notes, a QuickBooks total per tab, and a Summary tab with the roll-forward.
