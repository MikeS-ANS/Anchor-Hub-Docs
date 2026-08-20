# Duo Invoice Processor

Cisco Duo (ANS's MFA/two-factor platform) has no billing API, so ANS gets a flat monthly CSV export of every client sub-account's seat counts instead. This tool reads that CSV, matches every sub-account against Autotask, shows you exactly what changed since last month, and pushes quantity updates to the right contract line — without you touching a spreadsheet or opening Autotask by hand.

---

## Loading a File

**If the file is already in SharePoint:**

1. Navigate to **Duo Invoice Processor** in the sidebar
2. Select the **year** from the first dropdown
3. Select the **file** from the second dropdown
4. Click **Process File**

Both dropdowns are sorted newest first. Since Duo's own filenames don't include a date, each file in the list is labeled by its real billing month instead — read out of the CSV the first time that file is opened. Once read, a file's month label is remembered on your machine, so reopening the same file later doesn't re-read it again.

**If you have a new CSV on your computer:** drag it onto the upload zone (or click the zone to browse for it). This processes the file directly from your computer — it does not upload anywhere on its own.

**To keep a copy in SharePoint:** once a locally-loaded file has been processed, an **Archive to SharePoint** button appears next to it. This is optional and separate from processing — it uploads the same file into the correct year folder (creating that year's folder automatically if it's the first file archived for it), so the next person can pick it up from the SharePoint list instead of finding the CSV again.

Files live in SharePoint under **ANSVendors → DUO** (one folder per year) — the same library Kaseya and Cytracom use.

---

## Reading the Results

Once a file loads, each Duo sub-account gets its own row, matched to an Autotask company through the Hub Company Directory. A sub-account is matched by Duo's own **Account ID** first — a stable identifier that doesn't change even if the sub-account gets renamed — and only falls back to matching by name the first time a sub-account is mapped, or if Duo ever reissues its ID (the mapping is updated to the new ID automatically once that happens, so it stays matched by ID going forward).

Three sub-accounts are always excluded automatically and never shown as something to reconcile: **Anchor Network Solutions**, **Test account**, and **Anchor NFR** — these are ANS's own internal/test accounts, not client licensing.

Each reconciled client shows current vs. invoiced seat counts for both **Duo Essentials** and **Duo Premier** — the only two Duo editions this tool knows about. A client not yet mapped to an Autotask company is flagged as needing mapping, the same as any other invoice processor in the Hub — nothing is guessed.

---

## The Diff Table

For each matched client, the table shows one row per edition actually present on the invoice for that client:

| Column | Meaning |
|---|---|
| **Edition** | Duo Essentials or Duo Premier |
| **Current → Invoice** | The contract's current seat quantity vs. what this file shows |
| **Δ** | The difference |
| **Cost / Price** | Shown for every line; editable only for a brand-new line — see below |

Rows with a real change (or a brand-new line) are pre-selected for you; a row with no change is left unselected since there's nothing to push.

**Existing lines** — cost and price are never editable and are never sent to Autotask when you push. Only the seat quantity changes.

**Brand-new lines** — an edition the client's contract has never carried before shows a blank cost and price field. Nothing is pre-filled — type in the real values before this row can be selected for push.

---

## Company Matching

Duo is one of the platforms tracked in the Hub's **Company Mapping** tool, alongside Pax8, Kaseya, BlackPoint, Cytracom, and Meraki. If a sub-account shows **"needs mapping,"** it means this Duo sub-account has no confirmed match to an Autotask company yet.

Map it from **Company Mapping** the same way you would for any other platform — search for the Autotask company and confirm the match. Once mapped, the sub-account's Account ID is remembered, so future invoices match automatically even if the sub-account gets renamed later.

---

## Pushing to Autotask

1. Review the diff table and tick the rows you want to push (changed/new rows are pre-selected; you can add or remove any row)
2. For any brand-new line, type in the cost and price
3. Click **Push to Autotask**
4. Review the confirmation dialog — it lists every client, edition, and exact target contract/line, with **current → new** shown for each row
5. Confirm to push

A progress panel then streams each row's result in real time, the same way Cytracom and Kaseya's do — pushed, no change, or an error with the reason shown next to the row.

Pushing writes real, irreversible adjustments to live Autotask contracts. There is no undo button inside the Hub for a push — a mistaken quantity change has to be corrected with another adjustment (or by hand in Autotask).

**Process each month's file within that month.** Every adjustment is dated the **1st of the month you push in** — not the month the file covers. Processing promptly each month keeps those the same.

Every confirmation, status, and error message in this tool uses the Hub's own themed popup styling — never a plain browser dialog.

---

## Push History

Expand **Push history** on the results screen to see past pushes for this tool: when each one ran, who ran it, and what changed. This is pulled from the Hub's shared activity log, so it's visible to everyone, not just the person who ran it — the same shared log Cytracom, Kaseya, and BlackPoint push into.

---

## Good to Know

- **This tool has no AI or AI-generated content anywhere.** Every match and number on screen is computed directly from the CSV and live Autotask data.
- **Only two Duo editions are recognized** — Duo Essentials and Duo Premier. There's no free-text catalog matching the way Cytracom has, since Duo's product line is this simple.
- **Excluded sub-accounts** (ANS's own internal/test accounts) are filtered out before you ever see them — you won't find a way to reconcile or push against them, by design.
