# Autotask Contract Changes

The Autotask Contract Changes tool shows a side-by-side comparison of contract service quantities and prices between two points in time. Use it to audit what changed on billing runs, investigate client billing discrepancies, or verify that a push from the Invoice Processor applied correctly.

---

## Running a Comparison

1. Navigate to **Autotask Contract Changes** in the left sidebar
2. Select the **From** date and **To** date for the period you want to compare
3. Click **Run**

The tool queries Autotask for all contract service adjustments in that date range, then groups results by company and service.

---

## Reading the Results

Results are organized by company. Each row shows a contract service and what changed:

| Column | Meaning |
|---|---|
| **Company** | Autotask company name |
| **Service** | Contract service line (e.g. Azure, Nerdio, Security+) |
| **From Units** | Seat count / units at the start of the period |
| **To Units** | Seat count / units at the end of the period |
| **Change** | Delta — positive means increase, negative means decrease |
| **From Price** | Unit price at the start of the period |
| **To Price** | Unit price at the end of the period |
| **Effective Date** | When the change took effect in Autotask |

Rows with no change are hidden by default. Use the filter to toggle them on if needed.

---

## Exporting

Click **Export to Excel** to download the full comparison as a formatted spreadsheet. Useful for sharing with account managers or keeping a billing audit trail.

---

## Troubleshooting

**Results come back empty**
No contract adjustments were recorded in Autotask for that date range. Confirm the date range is correct and that contract pushes from the Invoice Processor ran successfully during that period.

**A company is missing from results**
The company may not have had any billable services change during the selected period, or there may be no Autotask contract linked to it. Check Company Mapping to confirm the company is mapped.
