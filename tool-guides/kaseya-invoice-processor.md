# Kaseya Invoice Processor

The Kaseya Invoice Processor ingests your monthly Kaseya/Datto XLS invoices directly from SharePoint, generates ready-to-enter QuickBooks journal entries, and pushes seat counts back to Autotask contracts — all without manually touching a spreadsheet.

---

## Loading an Invoice

1. Navigate to **Kaseya Invoice Processor** in the sidebar
2. Select the **year folder** from the dropdown
3. Select the **invoice file** — grand totals from prior runs are shown next to each file
4. Click **Process Invoice**

Invoices are loaded directly from SharePoint (ANS-Vendors → Kaseya/Invoices). No local file storage or manual downloads needed.

The processor auto-detects the Details sheet and all column positions (Company, Module, Qty, Rate, Total, etc.) even if Kaseya changes their column layout.

---

## What You See After Processing

### Metric Cards
- **Invoice Total** — grand total of the entire invoice
- **Invoice Date** — billing period extracted from the filename
- **Clients** — number of distinct companies on the invoice

### QBO Journal Entries
Ready-to-enter lines for your QuickBooks journal entry. Each line shows:
- **Description** — product / service name
- **QBO Account** — full account path (e.g., `Cost of Services-Recurring Svcs:...`)
- **Class** — Strategic Services, Service Delivery, or Admin based on your settings
- **Amount** — calculated dollar amount

Click any column header to sort. Bundled billing splits (DWP Bundled, SaaS Bundled) appear automatically as separate lines when configured.

### Cost Breakdowns
Nine data tables give you a full picture of the invoice:

| Table | What it shows |
|---|---|
| Totals by Module | Costs across DWP, SaaS, RMM, PSA, Networking, etc. |
| Totals by Category | Breakdown by Kaseya category field |
| Anchor Tools | Unallocated / shared tool costs |
| Datto Workplace Costs | DWP + DFP by product with qty and avg rate |
| Datto Workplace Usage | Per-client seat counts, cross-tab by product |
| Datto SaaS Usage | SaaS license counts by client with bundled/billable split |
| Datto Backup & Networking | BCDR, Cloud Continuity, Azure Siris, Networking |
| Totals by Client | Full per-client breakdown across all modules |

---

## Bundled Billing

### Workplace Bundle Overrides
For clients with contracted bundled Workplace/DFP seats, configure overrides in **Settings → Workplace Bundle Overrides**:

- Add a row per client/product with the contracted bundled seat count
- Supported products: all six DWP and DFP license types
- Bundled seats are deducted from the billable count before pushing to Autotask
- QBO bundled amount = bundled seats × avg rate from that invoice (not a percentage)

### SaaS Bundled Clients
Mark clients as bundled in the **Datto SaaS Usage** table:

1. Check the **Bundled?** checkbox for a client — saves permanently to the hub directory
2. The tool reads that client's included seats from the **Managed Service Client MSC.xlsx** file in SharePoint
3. The table shows **Bundled Qty** (from SP) and **Billable** (Invoice Qty − Bundled Qty)
4. QBO entries automatically split into Standalone and Bundled lines with correct dollar amounts

**Manual override:** Click the **✎ pencil** next to any bundled qty to enter a custom value. Overridden values show in amber. Click **✕** to clear.

---

## Pushing to Autotask

After reviewing the data, push seat counts directly to Autotask contract service lines.

### Datto Workplace Push
Updates DWP/DFP seat counts on each client's Managed Cloud Services contract.

1. Click **Push to AT** in the Datto Workplace Usage card
2. Review the preview — all clients and products with their new quantities
3. Confirm to push

The tool finds each client's active contract, applies bundle deductions, and creates a ContractServiceAdjustment effective the first of the current month. If the current contract period is invalid it rolls to the next contract automatically.

### Datto SaaS Push
Updates SaaS license counts (service IDs 98 and 88) on each client's contract.

1. Click **Push to AT** in the Datto SaaS Usage card
2. Review the preview
3. Confirm to push

Bundled quantities are automatically deducted. Progress streams in real time — each company shows success, no change, skipped, or error.

---

## Organization Mapping

The processor needs to know which Kaseya company name maps to which Autotask company. This is handled automatically in the background using the centralized hub directory.

**Auto-matching** runs when you process an invoice. Matches with ≥85% confidence are accepted silently. Lower-confidence suggestions appear in the **Org Mapping** panel (admin-only) for review.

**Manual mapping:**
1. Open the Org Mapping panel
2. Find the unmatched client
3. Search for the correct Autotask company and accept the match

Once confirmed, the mapping persists in the hub directory and applies to all future invoice runs.

---

## Exporting

Click **Export Excel** to download a formatted multi-sheet workbook:
- **Sheet 1** — Invoice Summary with all nine data tables, color-coded headers, and alternating rows
- **Sheet 2** — QBO Entries ready to copy into QuickBooks

The file opens automatically in your default spreadsheet app.

---

## Snapshot Comparison

The tool saves each processed invoice as a snapshot (by month). Use the comparison feature to spot month-over-month changes:

1. Click **Compare Invoices** (or **Compare Snapshots** for saved data)
2. Select a baseline and a current invoice
3. Review the delta report: grand total change, modules up/down/new/dropped, per-client breakdowns

Unchanged items are filtered out — you only see what moved.

---

## Settings

Navigate to **Settings** within the tool to configure:

| Setting | What it controls |
|---|---|
| PSA splits | % split of PSA costs across Strategic Services, Service Delivery, Admin, Co-Managed QBO classes |
| RMM splits | % split of RMM costs across Strategic Services, Service Delivery |
| IT Glue splits | % split of IT Glue costs |
| Workplace Bundle Overrides | Per-client contracted bundled seats per product |

Splits must total 100% per product. The QBO journal entries recalculate immediately when you save settings.
