# Pax8 Invoice Processor

The Pax8 Invoice Processor is the core billing tool. It loads your monthly Pax8 invoice, breaks it down by service and client, and pushes updated pricing and seat counts directly to Autotask contracts — replacing the manual process of copying numbers between systems.

## Loading an Invoice

Navigate to **Invoice Processor** in the left sidebar.

### Load Most Recent Invoice
Click **Load Most Recent Invoice** to automatically fetch and process the latest Pax8 invoice. This is what you'll use every month.

### Browse Past Invoices
Click **Browse Past Invoices** to select a specific month from a dropdown. Each entry shows the month and the Pax8 invoice total.

> **Caching:** The first time you load a past invoice via the API, the raw line items are saved to SharePoint automatically. The next time anyone on the team opens the same invoice, it loads from that cache instantly — no API call required. A banner appears at the top of the results showing when the data was last fetched, with a **↺ Reprocess** button to force a fresh pull from Pax8 if needed.

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

Invoice results rely on mapping each Pax8 company to its Autotask counterpart. All mappings are managed through the **Company Directory** — any company you link there is automatically recognised the next time an invoice is processed.

When you load an invoice, the processor checks for any unmapped Pax8 companies and tries to match them automatically. Every match is a **suggestion**. Nothing is written to the Company Directory until you click **Confirm** — including a 97% match.

- **"N Companies Need Confirmation"** — every suggested match, strongest first, in a panel at the bottom of the results. Review the suggested match and click **Confirm** to save it.
- **"Could not check"** — shown above the suggestions if the Autotask lookup itself failed for a company (a timeout, a rate limit). That is *not* the same as "no match found" — those companies were never searched. Reprocess the invoice to try again.

> **Why confirmation is required.** The matcher strips words like "Group", "Technologies", "Solutions" and "Inc" before comparing, so "Anchor Group" scores 97% against "Anchor Technologies" — and they are different companies. A high score is a reason to look, not proof.

Confirmed matches persist to the Company Directory and apply to all future invoices automatically.

---

## Pushing to Autotask

> **Needs your personal Autotask key** (Hub **Settings** → Autotask PSA). Nothing is gated on it — the push buttons appear either way — but without one the Hub falls back to a shared read-only key and every row comes back `✗ Error`.

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

### What the confirmation shows

Before anything is written, the confirmation reads the current values straight from Autotask and shows you, per client:

- the exact **contract and service line** being written to
- **current → new** — the seat count, or the cost and price
- the **change**, so a seat count that moved further than you expected is obvious

It also calls out separately:

- **rows writing a zero** — a $0 price or a quantity of 0. Both are legitimate and both are destructive, so they are shown rather than blocked.
- **fractional quantities** on anything other than Nerdio (where they are normal). The tool passes the value through exactly as it arrived and never rounds or truncates it — but it does not promise what Autotask will then do with it. Autotask's quantity-adjustment field takes whole numbers, so a fraction on one of these services is more likely to be rejected or truncated at their end than applied. That is precisely why it is put in front of you before you push rather than sent quietly.
- **rows that will be skipped** — already matching Autotask, not mapped to a company, or no matching contract
- **rows that will NOT be pushed — ambiguous mapping** — more than one Pax8 account resolves to the same Autotask company, so the push refuses to guess how to merge them
- **rows that could not be checked** — the Autotask lookup failed. These are not "unchanged"; pushing will still attempt them.

Reading these values can take up to a minute. The Push button stays disabled until they arrive, and if they cannot be read the panel says so rather than showing an empty table.

> A client who is **absent** from the invoice keeps their current Autotask quantity — nothing is written for them. A client who appears on the invoice **at zero** is pushed as zero, because that is evidence the service ended.

---

## Reading Push Results

After each push, a results modal shows the outcome per client:

| Result | Meaning |
|---|---|
| ✓ Updated | Successfully pushed to Autotask |
| – No change | Seat count or price already matches — nothing to do |
| – No AT mapping | This client isn't linked to an Autotask company yet |
| ⚠ No contract found | Client is mapped but no matching contract exists in Autotask |
| ⚠ Ambiguous mapping | More than one Pax8 account maps to this Autotask company — not pushed |
| ✗ Error | Something went wrong — the error message will say what |

Results are saved to **Push History** (bottom of the results page) so you can review them any time.

---

## Push History

The last 20 push runs are shown at the bottom of the page. Click any row to expand it and see per-client detail. Use this to identify which clients are consistently showing "no contract" or "no mapping" — those need to be fixed in Autotask or Company Mapping.

---

## Exporting

Click **Export to Excel** to download the full invoice breakdown as a formatted spreadsheet — useful for sharing with the accounting team or keeping records.

---

## Settings

The tool has its own **Settings** tab, next to **Invoice** at the top.

**Default Azure margin %** — the margin every client's Azure line starts at when an invoice is processed. Prices are rounded **up** to the next $5 from it. You can still change the margin or the price on any individual row before pushing; this only sets where each row starts.

> **Global setting — shared by everyone.** Stored centrally, so it is the same number for every member of staff. Saving needs the Hub admin role; without it the save is refused and the current value is kept. A blank or out-of-range value is also refused rather than being read as zero.

---

## Troubleshooting

**Results show "Loaded from cache" but mappings look outdated**
Click **↺ Reprocess** in the banner at the top. This deletes the cached copy and fetches fresh line items from Pax8, then re-runs mapping against the current Company Directory.

**A service's push button is missing**
Nothing in this tool hides a push button when credentials are missing, so this is never a key problem. The Azure section's button always appears. A per-service button — Nerdio, Exclaimer, Ironscales, Printix — appears only when that service actually has lines on the invoice you loaded. If Nerdio has no button this month, this month's invoice has no Nerdio lines.

**Every row comes back ✗ Error when you push**
This is what a missing personal Autotask key really looks like. The push is not blocked; the Hub falls back to a shared read-only Autotask key, runs the push, and Autotask refuses each write. Go to the Hub's own **Settings** page (sidebar, not this tool's Settings tab) → Autotask PSA and add your credentials. An error naming a particular contract or service is a different problem — see the two entries below.

**Client shows "no contract found" every month**
The contract in Autotask is probably named differently than expected. Check that the contract name contains "Azure" (for Azure/Nerdio) or "Managed Cloud" (for Exclaimer, Ironscales, Printix). Contact Mike if it needs to be updated.

**Client shows "no AT mapping"**
The Pax8 company isn't linked to Autotask yet. Open **Company Directory**, find the company, and link it to the correct Autotask account. The link is picked up automatically on the next invoice load or reprocess.

**Invoice fails to load**
Check your internet connection. If the error mentions credentials, the Pax8 API keys in Azure Key Vault may need to be refreshed — contact Mike.

**A total looks about a third of what it should be**
This is the bug the September 2026 hardening pass fixed. If you see it on a build older than that, reprocess on a current build: the invoice fetch used to swallow a failed page and report success with a partial total, and the partial number was then cached.
