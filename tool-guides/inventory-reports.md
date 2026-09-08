# Inventory Reports

Inventory Reports puts the three monthly Autotask inventory reports Accounting reconciles — **On Hand**, **Added**, and **Picked** — directly in the Hub, read live from Autotask (or from a stored month-end snapshot once one exists for that month), with an Excel export in the same layout as the reports you used to pull from Autotask by hand.

## Who sees it

Finance and admin roles. Grants are managed in Access Management.

## Picking a month

The month picker at the top right defaults to the most recently closed month. Month boundaries are Denver time.

The chip beside it tells you what the numbers are:

* **Live** — the current, open month. Figures are as of right now.
* **Live (month closed, no snapshot)** — a closed month with no snapshot yet. Added and Picked are recomputed from dates and are reliable. **On Hand is today's stock, not that month's** — Autotask only keeps current stock. A banner on the On Hand tab says so.
* **Snapshot taken <date, time>** — a month-end snapshot exists for this month. Every tab shows the figures that were stored at snapshot time, not a fresh read from Autotask, and the "On Hand is today's stock" banner does not appear, because the number shown really is that month's. Hover the chip to see whether it was a scheduled or a replaced snapshot.
* **Snapshot taken late on <date>** — a snapshot for this month exists, but it wasn't taken on the 1st. Its figures are the best record available; its On Hand specifically is stock as of the day it was actually taken, not a guaranteed true month-end count. Hover the chip to see exactly when and by whom.

All three tabs load at once; a tab shows a small spinner until its own data is in. Autotask can take up to a minute for a live month; a snapshotted month loads faster since nothing needs to be re-read from Autotask.

If any Autotask lookup fails, the tab shows an error with a Retry button instead of a table — a partial result would read as a real balance. If the Hub's settings service cannot be reached, On Hand and Picked still load but show an amber **Using default settings** banner, because the excluded-location list and lot-match window in force may not be the ones you saved.

## On Hand

Every stocked item with available units above zero, outside the excluded locations (see Settings). Columns: Product Name, Serial Number, On Hand, **Unit Cost (per unit)**, **Ext. Cost (units × unit cost)**, Location, Added Date.

> In the old LiveReport export, the column labelled "Unit Cost" was actually the extended cost. The Hub shows both, labelled correctly; the Excel export keeps the old header for continuity (see Export).

Rows with a blank product name or a $0.00 cost are flagged inline and counted in the **Flags** tile — they are shown, not hidden.

## Added

Stocked items created in the month, every location. **Quantity Added is units received** — an item added and picked in the same month still counts. Rows created by a transfer between locations are excluded so a move is not counted as a purchase. Extra columns: Location, Vendor, and Vendor Invoice #, straight from Autotask, to help match against QuickBooks.

## Picked

Inventory that left stock against a charge in the month. Each row shows the charge's Date Charged, Account, Ticket or Project, Quantity, Total Cost, Unit Cost, Unit Price, and Sold By, plus:

* **Match** — *Exact* means a serialized item whose charge is recorded on the item itself. *By product/timing* means a non-serialized lot (cables, adapters, drives): Autotask does not record which charge a lot pick went to, so the Hub matches the charge to a lot of the same product picked within the lot-match window (15 minutes by default). *By product/units moved* is a third way a lot pick can be matched: Autotask only keeps a lot's most recent pick date, so if the same lot was picked twice in a month, the earlier pick has no timestamp to match against. Once two consecutive month-end snapshots exist, the Hub also knows how many units that lot released during the month, and can use that count to find the earlier pick even without a matching time. Rows matched this way carry this pill so you know the match came from the unit count, not the clock. This signal needs two scheduled month-end snapshots in a row to work, so the earliest month it can appear in is the one following the November 1 snapshot.
* **Stock Unit Cost** — the stocked item's own cost, highlighted when it differs from the charge's Unit Cost.

Each serialized unit is its own row with Quantity 1 and Total Cost equal to its unit cost — the same layout the old report used, so a charge for three laptops appears as three rows that add up to the charge. A non-serialized lot pick is one row per charge, with the charge's quantity and total.

### Needs review

Under the table, anything that did not line up. The first three groups are outside the totals; cost differences are counted, at the charge's cost, and listed here so the difference gets a look:

* **Charge for a stocked product with no matching pick** — usually a drop-ship. A charge recorded for zero quantity lands here too, since nothing was actually picked. A lot that already gave out everything it had to earlier charges this month shows up here as "lot budget spent," so you can see it wasn't missed — it ran out.
* **Left stock with no matching charge** — worth a look. Once two consecutive month-end snapshots exist, this can show the exact number of units and dollars a lot released with no charge to explain them, rather than just flagging that something is unaccounted for.
* **Pick and charge dated in different months** — the old report dropped these silently; the Hub lists them.
* **Charge cost differs from stocked cost** — counted at the charge's cost; the difference is what to check.

## Checked and Notes

Every row on On Hand, Added, and Picked has a **Checked** box (first column) and a **Note** field (last column), so you can mark what you've already reviewed without leaving the Hub.

* **Checked** — click the box to mark a row as reviewed. It fills in with your initials; hover over it to see who checked it and when. Click it again to clear it.
* **Note** — type a note and it saves automatically when you click away from the field or press Enter. While it's saving you'll see "Saving…"; once it's stored you'll see "Saved" with the time. If a save fails you'll see "Not saved · Retry" — your typed text is kept, so nothing is lost, and you can retry right there.

Checked marks and notes belong to the specific month and row, so they survive a snapshot being replaced later (for example, if a late snapshot is redone). Both Checked and Note are included in the Excel export, as the last two columns on each sheet.

If marks can't be loaded for some reason, an amber banner says so and the Checked/Note columns are temporarily disabled with a Retry button — an empty box in that state means "unknown," not "not reconciled."

## QuickBooks total

Each tab — On Hand, Added, and Picked — has its own **QB total** field on the totals bar at the top. Type in the total from QuickBooks for that report and month; it saves automatically when you click away from the field.

**Variance vs QB** shows Autotask's total minus the QuickBooks total you entered — green when they match exactly, amber when they don't. Until a QuickBooks total is entered, it reads "not entered." Underneath the field, the Hub shows who entered the QuickBooks total and when.

## Summary

Once a month-end snapshot exists for a month, the Summary tab shows that month's roll-forward — comparing this month's snapshot to last month's. For a still-open month with no snapshot yet, Summary explains that the roll-forward will appear once the month-end snapshot is taken.

The roll-forward is a simple ledger, line by line:

1. **Prior month-end On Hand** — the stock and dollar value from last month's snapshot.
2. **+ Added** — everything received into stock this month, at Ext. Cost, every location.
3. **− Picked** — everything that left stock against a charge this month, valued at inventory cost, not the price charged to the client.
4. **− Removed** — items whose removed count went up since last month's snapshot (write-offs, not picks).
5. **− Net moved into excluded locations** — inventory transferred into a location Settings excludes from On Hand (Retired/RMA'd, Shelf Spare, etc.), which takes it out of what counts as "on hand" without it being a pick or a removal.
6. **= Expected On Hand** — what the lines above say should be on the shelf.
7. **Actual month-end On Hand** — the real count from this month's snapshot.
8. **= Unexplained variance** — the gap between expected and actual. This line is informational; it does not have to come out to zero, and a nonzero number in a given month is normal.

If this is the first month with a snapshot (no snapshot exists for the prior month), the ledger says "No prior snapshot — the roll-forward starts next month," and every line that depends on a prior month (Prior On Hand, Removed, Expected, Unexplained variance) shows a dash — Added, Picked, and Actual On Hand still show. If either of the two snapshots behind a month's roll-forward was taken late, a banner says so; treat the variance with that in mind, since a late snapshot's On Hand reflects whatever day it was actually taken, not necessarily the true month end.

Below the ledger:

* **QuickBooks totals** — the Autotask total, the QuickBooks total you entered, and the variance, for each of On Hand, Added, and Picked.
* **Reconciled** — how many rows on each tab are currently checked, out of the total rows on screen for that month.
* **Where the variance usually hides** — six expandable lists of the rows most likely to explain a difference between the two snapshots:
  * **Unit cost changed between snapshots** — items whose Autotask unit cost changed since last month, and what that changed the extended cost by. Autotask doesn't tell the Hub who made a cost change or when, so there's no "Edited by" column here — that detail simply isn't something Autotask exposes.
  * **Items sitting in excluded locations** — stock that's physically present but in a location that doesn't count toward On Hand.
  * **Lots that moved without a charge** — non-serialized inventory that left stock with nothing to explain it.
  * **Negative-unit items** — items whose available count in Autotask has gone below zero, worth a look regardless of dollar value.
  * **Blank product names** — on-hand rows with no product name recorded in Autotask.
  * **Rows a transfer excluded from Added** — items moved between locations rather than newly purchased, so they don't count as Added even though Autotask records an Added date for them.

## Export to Excel

Each tab exports one workbook named `MM-YY - <Report Name>.xlsx`, matching the old file names so it drops into the existing SharePoint folder. The first columns are the old report's columns, in the old order, under the old headers — including "Unit Cost" carrying the extended amount on On Hand and Added — with the Hub's extra columns to the right, and **Checked** and **Note** as the last two columns on every sheet. The totals row uses real `SUM` formulas. The Picked export also carries an **Ext. Price** column (quantity × unit price) on the right, so the Total Price shown on screen is in the file too. Money cells keep the same precision Autotask holds, so the workbook's totals match the old report's to the penny. A Picked export carries its Needs-review rows on a second sheet, never among the data rows. If the month you're exporting has a month-end snapshot, the export uses that snapshot's stored rows rather than re-reading Autotask, so the file matches exactly what's on screen.

## Settings (gear icon)

* **Excluded from On Hand** — which Autotask inventory locations are left out of the On Hand report (seeded with the seven the old report excluded: Retired/RMA'd Datto, Retired/RMA'd Calyptix, Lost Items, Loaner Items, and the three Shelf Spare locations). Added always includes every location.
* **Lot-match window** — minutes between a lot pick and a charge for them to count as the same event. Wider matches more and mis-matches more; unmatched rows go to Needs review either way.
* **Month-end snapshot** — the Hub automatically takes a snapshot of the month that just closed at 6:00 AM Denver time on the 1st, retrying every hour through the rest of that day if Autotask isn't reachable right away. Once a month has a snapshot, that month's figures come from the snapshot instead of a fresh Autotask read every time you open it.

  If a snapshot was missed, or you need a corrected one, this section has a **Snapshot now** button for whichever closed month is selected in the month picker — it's disabled while the still-open current month is selected, since an open month can't be snapshotted yet. If that month already has a snapshot, the button reads **Replace <month> snapshot** instead and asks you to confirm first, showing when the existing snapshot was taken and its totals, before overwriting it. Checked marks and notes on that month are kept through a replacement. Any snapshot taken after the 1st — whether the automatic job catching up later that same day, or a human taking one on a later date — is labeled **late**; its On Hand reflects stock as of whenever it was actually taken, not a guaranteed true month-end count.
