# Pax8 Invoice Comparison

The Pax8 Invoice Comparison lets you compare invoices side by side — current month vs. last month, or across the last two or three months — to see exactly what changed for each client. Use it to catch licenses that may have been purchased in error, explain billing increases to clients, or investigate unexpected cost changes before the invoice goes out.

---

## Running a Comparison

1. Navigate to **Pax8 Invoice Comparison** in the left sidebar
2. Select the invoices you want to compare:
   - **Current vs. Last Month** — quick month-over-month view
   - **Last 2 Invoices** — compares the two most recent invoices
   - **Last 3 Invoices** — shows a three-month trend
3. Click **Compare**

Anchor Hub fetches the selected invoices from Pax8 and runs the comparison. Results appear grouped by client.

---

## Reading the Results

Each client section shows their subscription line items side by side across the selected months. Rows are color-coded to highlight what changed:

| Indicator | Meaning |
|---|---|
| **Green** | New subscription — not on the previous invoice |
| **Red** | Removed subscription — was on the previous invoice, now gone |
| **Orange** | Quantity or price changed between months |
| No highlight | Unchanged — same quantity and price as before |

For each line item you can see the product name, quantity, unit price, and total for each month in the comparison.

---

## Common Use Cases

**Answering "why did my bill change?"**
Run a current vs. last month comparison for the client in question. Orange and green rows directly explain the difference.

**Catching accidental license purchases**
A green row for an unexpected product, or an orange row showing a seat count jump, flags a potential error. Review with the client before the next billing cycle.

**Spotting trends**
The 3-month view shows whether a client's spend is consistently growing, shrinking, or fluctuating — useful for renewal conversations or forecasting.

---

## AI Prompt

When changes are detected, Anchor Hub sends them to Hatz.ai (Claude Haiku, via the `/v1/anthropic/messages` endpoint — `main/ipc/invoiceMonitor.js`, `callClaude`) for a plain-English summary. No model picker for this one — it's a single hardcoded model call.

**Prompt (built fresh per run, current invoice + detected changes):**
> You are a billing analyst reviewing an MSP's monthly Pax8 invoice.
>
> Invoice {id}, dated {date}, Total: ${current-period total}
>
> Changes detected vs. prior months:
>
> {company}:
>   - {change 1}
>   - {change 2}
> ...
>
> Write a concise plain-text executive summary (4-6 sentences) of the most significant billing changes. Do NOT use markdown, bold, asterisks, or headers — plain sentences only. Call out anything that looks like a potential error or needs immediate attention. Be specific about company names and dollar amounts. End with a recommended action if warranted.

The `Total` figure is the sum of current-period line items, not the invoice's Total Due (see the v2.4.0 release notes on the Net 45 fix). If no changes are detected, the AI step is skipped entirely. If the Hatz.ai call fails, the AI summary is simply omitted from the results — no error is shown.

---

## Troubleshooting

**Comparison comes back empty**
The selected invoices may not have loaded yet or the Pax8 API credentials may need to be refreshed. Check Settings and try again.

**A client is missing from the comparison**
If the client didn't have any active subscriptions in one of the selected months, they may be filtered out. Check the filter options to include all clients.
