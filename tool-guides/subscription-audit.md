# M365 Subscription Comparison

The M365 Subscription Comparison compares your active Pax8 subscriptions against Autotask contract services and flags anything that doesn't line up — clients being billed for the wrong quantity, services in Pax8 with no matching contract line, or vice versa.

## Running an Audit

1. Navigate to **Subscription Audit** in the left sidebar
2. Click **Run Audit**
3. The audit queries both Pax8 and Autotask — this takes 30–60 seconds depending on the number of clients
4. Results appear grouped by client

## Reading the Results

Each row shows a Pax8 subscription alongside its matched Autotask contract service. Discrepancies are highlighted:

- **Quantity mismatch** — Pax8 shows a different seat count than what's on the Autotask contract
- **Missing in Autotask** — Active Pax8 subscription with no corresponding contract service
- **Missing in Pax8** — Contract service exists in Autotask but no active Pax8 subscription found

## Exporting

Click **Export** to download the discrepancies as a spreadsheet for review or to share with the billing team.

## Subscription Cache (v2.4.0+)

The Pax8 subscription list is cached to SharePoint per billing month, since it's stable once the month is underway and re-fetching it on every run is one of the slower calls in the Hub. A banner above the log shows whether the current run loaded from cache or fetched live, along with when it was cached. Click **Refresh** to force a live re-fetch and overwrite the cache — useful if subscriptions changed mid-month. Company Directory changes (exclusions, mappings) are never cached and always apply fresh.

## Where mappings come from

The audit matches Pax8 clients to Autotask companies using the **Company Mapping** tool only — if a client shows as unmapped, fix it in Company Mapping and run the audit again. Products are matched through Company Mapping first; if a product has no mapping there, or its mapped service isn't found on that client's Autotask contracts, the audit also tries matching the product's name against the services already on those contracts. If the Company Directory can't be loaded when you click **Run Audit**, the audit stops before checking anything, with *"Could not load the Company Directory … Nothing was checked"*, and no tickets are created. Wait a minute and run it again.

---

> **Guide in progress.** More detail will be added here as the tool evolves.
