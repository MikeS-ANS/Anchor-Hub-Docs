# Subscription Audit

The Subscription Audit compares your active Pax8 subscriptions against Autotask contract services and flags anything that doesn't line up — clients being billed for the wrong quantity, services in Pax8 with no matching contract line, or vice versa.

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

---

> **Guide in progress.** More detail will be added here as the tool evolves.
