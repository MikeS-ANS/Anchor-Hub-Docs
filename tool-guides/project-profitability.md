# Project Profitability

The Project Profitability tool pulls all completed (and optionally active) projects from Autotask and calculates gross margin, cost of delivery, and billing efficiency for each one — so you can see at a glance which projects were profitable and which ones need attention.

## What It Does

For each project in your selected date range, the tool calculates:

- **Cost of Delivery** — billed hours × your configured blended labor rate
- **Gross Margin** — invoiced amount minus cost of delivery
- **Gross Margin %** — color-coded green/amber/red based on your margin threshold
- **Hours Variance** — estimated vs. actual hours
- **Billing Type** — Fixed Price, Time & Materials, or Block Hours

It also flags issues automatically:

- Margin below your warning threshold
- Pending milestone amounts not yet invoiced
- T&M projects with unbilled hours
- Projects missing an hour estimate

## Running a Report

1. Open **Project Profitability** from the left nav
2. Set the **start** and **end** dates for the project range
3. Check **Include active projects** if you want in-progress work included
4. Click **Run Report**

The report takes 20–40 seconds depending on how many projects are in range — it's fetching live data from Autotask.

## Reading the Results

The results table has 16 columns. Click any column header to sort.

| Column | What It Shows |
|---|---|
| Project | Project name (links to Autotask) |
| Company | Client name |
| Lead | Project lead resource |
| Billing Type | Fixed Price, T&M, or Block Hours |
| Status | Complete, In Progress, etc. |
| Contract | Associated contract name |
| Est Hours | Hours estimated at project creation |
| Actual Hours | Total hours logged |
| Billed Hours | Hours actually billed to client |
| Hours Variance | Actual vs. estimated (%) |
| Invoiced | Total amount invoiced from milestones |
| Pending | Milestone amounts not yet invoiced |
| Cost of Delivery | Billed hours × blended labor rate |
| Gross Margin $ | Invoiced minus cost of delivery |
| Gross Margin % | Color-coded: green = healthy, amber = watch, red = below threshold |
| Flags | Auto-detected issues for this project |

The **stat cards** at the top show totals across all projects: total invoiced, total cost, blended margin, and counts of flagged projects.

## Configuring Settings

Click **Settings** at the top of the tool to adjust:

- **Blended Labor Rate** — your average fully-loaded cost per billable hour (default: $83.50/hr)
- **Standard Billable Rate** — your rack rate, used for comparison (default: $200/hr)
- **Margin Warning Threshold** — projects below this % show in amber/red (default: 20%)

Changes save immediately and apply to the current results without re-running.

## Exporting

Click **Export to Excel** to download a 3-sheet workbook:

- **Project Summary** — all projects with full detail and margin color-coding
- **Flags & Issues** — only flagged projects, with issue descriptions
- **Lead Summary** — margin and billing efficiency grouped by project lead

## What's Excluded

The tool automatically excludes:

- Internal/Anchor ANS projects
- Recurring Service contracts (type 7) — those belong in the Invoice Processor
- Projects with no date information

## Tips

- Run it monthly after invoicing to catch margin problems before they compound
- Sort by **Gross Margin %** ascending to find your worst performers first
- The **Flags & Issues** export sheet is the fastest way to action items
- If a project shows $0 invoiced but has billed hours, the contract may be missing milestones in Autotask
