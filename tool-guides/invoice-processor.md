# Pax8 Invoice Processor

The Pax8 Invoice Processor is the core billing tool. It loads your monthly Pax8 invoice, breaks it down by service and client, and pushes updated pricing and seat counts directly to Autotask contracts — replacing the manual process of copying numbers between systems.

## Loading an Invoice

Navigate to **Invoice Processor** in the left sidebar.

### Load Most Recent Invoice
Click **Load Most Recent Invoice** to automatically fetch and process the latest Pax8 invoice. This is what you'll use every month.

### Browse Past Invoices
Click **Browse Past Invoices** to select a specific month from a dropdown. Each entry shows the month and total dollar amount. Use this to review historical invoices.

### Browse CSV
Fallback option if the Pax8 API is unavailable. Export a CSV from the Pax8 portal and load it manually here.

---

## Reading the Results

After loading, the results show:

**Metrics strip** — Total partner cost and key category totals at a glance.

**QBO Breakdown** — The dollar amounts to enter in QuickBooks Online, organized by account.

**Azure** — Per-client Azure costs with your calculated sell price and margin. You can adjust margins inline before pushing.

**Service Quantities** — Per-client seat counts for Nerdio, Exclaimer, Ironscales, and Printix.

**One-Time Charges** — Listed for reference. These are handled manually.

---

## Company Auto-Match

When you load an invoice, Anchor Hub automatically tries to match each Pax8 client to their Autotask company — this is what makes the push buttons work.

- **Green banner** — Companies that were matched automatically (high confidence). No action needed.
- **Yellow "Need Confirmation" panel** — Companies where the match is likely but not certain. Review the suggested match and click **Confirm** to save it. The push button for that client becomes active immediately.
- **No banner** — All companies were already mapped from a previous session.

Confirmed matches are saved permanently so they apply to all future invoices automatically.

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

**Push buttons are grayed out / missing**
Your Autotask write key isn't configured. Go to Settings → Autotask PSA and add your credentials.

**Client shows "no contract found" every month**
The contract in Autotask is probably named differently than expected. Check that the contract name contains "Azure" (for Azure/Nerdio) or "Managed Cloud" (for Exclaimer, Ironscales, Printix). Contact Mike if it needs to be updated.

**Client shows "no AT mapping"**
The Pax8 company isn't linked to Autotask yet. Run **Company Mapping** → Sync, or wait for auto-match on the next invoice load.

**Invoice fails to load**
Check your internet connection. If the error mentions credentials, the Pax8 API keys in Azure Key Vault may need to be refreshed — contact Mike.
