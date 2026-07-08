# Pax8 Invoice Processor

The Pax8 Invoice Processor is the core billing tool. It loads your monthly Pax8 invoice, breaks it down by service and client, and pushes updated pricing and seat counts directly to Autotask contracts — replacing the manual process of copying numbers between systems.

## Loading an Invoice

Navigate to **Invoice Processor** in the left sidebar.

### Load Most Recent Invoice
Click **Load Most Recent Invoice** to automatically fetch and process the latest Pax8 invoice. This is what you'll use every month.

### Browse Past Invoices
Click **Browse Past Invoices** to select a specific month from a dropdown. Each entry shows the month and the Pax8 invoice total.

> **Caching:** The first time you load a past invoice via the API, the raw line items are saved to SharePoint automatically. The next time anyone on the team opens the same invoice, it loads from that cache instantly — no API call required. A banner appears at the top of the results showing when the data was last fetched, with a **↺ Reprocess** button to force a fresh pull from Pax8 if needed.

### Browse CSV
Fallback option if the Pax8 API is unavailable. Export a CSV from the Pax8 portal and load it manually here. CSV loads are not cached.

---

## Reading the Results

After loading, the results show:

**Metrics strip** — Total partner cost (the sum of all partner pricing across every line item) and the total number of invoice lines.

**QBO Breakdown** — The dollar amounts to enter in QuickBooks Online, organized by account. The TOTAL row at the bottom matches the metric strip.

**Azure** — Per-client Azure costs with your calculated sell price and margin. You can adjust margins inline before pushing.

**Service Quantities** — Per-client seat counts for Nerdio, Exclaimer, Ironscales, and Printix.

**One-Time Charges** — Listed for reference. These are handled manually.

---

## Company Mapping

Invoice results rely on mapping each Pax8 company to its Autotask counterpart. All mappings are managed through the **Company Directory** and stored in SharePoint — any company you link there is automatically recognised the next time an invoice is processed.

When you load an invoice, the processor checks for any unmapped Pax8 companies and tries to match them automatically:

- **High-confidence match (≥ 85%)** — saved silently to the Company Directory.
- **Medium-confidence suggestion** — shown in the **Needs Confirmation** panel at the bottom of the results. Review the suggested match and click **Confirm** to save it.

Confirmed matches persist to SharePoint and apply to all future invoices automatically.

---

## Pushing to Autotask

> **Requires** your personal Autotask write key to be configured in Settings.

### Azure Pricing
1. Review the Azure table — adjust any margins if needed
2. Click **Push to Autotask** in the Azure section
3. Confirm the **Effective Date** (defaults to the 1st of next month — Azure pricing applies to the next billing cycle)
4. Click **Push**

### Seat Counts (Nerdio, Exclaimer, Ironscales, Printix)
1. In the Services section, click the push button for the service (e.g. **↑ Nerdio**)
2. Confirm the **Effective Date** (defaults to the 1st of the current month)
3. Click **Push**

The app reads the current seat count from the contract, calculates the difference, and posts only the change — so if a client already has the right count, it's skipped automatically.

---

## Reading Push Results

After each push, a results modal shows the outcome per client:

| Result | Meaning |
|---|---|
| ✓ Updated | Successfully pushed to Autotask |
| – No change | Seat count or price already matches — nothing to do |
| – No AT mapping | This client isn't linked to an Autotask company yet |
| ⚠ No contract found | Client is mapped but no matching contract exists in Autotask |
| ✗ Error | Something went wrong — the error message will say what |

Results are saved to **Push History** (bottom of the results page) so you can review them any time.

---

## Push History

The last 20 push runs are shown at the bottom of the page. Click any row to expand it and see per-client detail. Use this to identify which clients are consistently showing "no contract" or "no mapping" — those need to be fixed in Autotask or Company Mapping.

---

## Exporting

Click **Export to Excel** to download the full invoice breakdown as a formatted spreadsheet — useful for sharing with the accounting team or keeping records.

---

## Troubleshooting

**Results show "Loaded from cache" but mappings look outdated**
Click **↺ Reprocess** in the banner at the top. This deletes the cached copy and fetches fresh line items from Pax8, then re-runs mapping against the current Company Directory.

**Push buttons are grayed out / missing**
Your Autotask write key isn't configured. Go to Settings → Autotask PSA and add your credentials.

**Client shows "no contract found" every month**
The contract in Autotask is probably named differently than expected. Check that the contract name contains "Azure" (for Azure/Nerdio) or "Managed Cloud" (for Exclaimer, Ironscales, Printix). Contact Mike if it needs to be updated.

**Client shows "no AT mapping"**
The Pax8 company isn't linked to Autotask yet. Open **Company Directory**, find the company, and link it to the correct Autotask account. The link is picked up automatically on the next invoice load or reprocess.

**Invoice fails to load**
Check your internet connection. If the error mentions credentials, the Pax8 API keys in Azure Key Vault may need to be refreshed — contact Mike.
