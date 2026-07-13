# Project Analysis

The Project Analysis tool takes a single Autotask project number and an optional scope-of-work PDF, pulls all tasks and time entries from Autotask, and runs an AI analysis via Hatz AI comparing what was scoped against what actually happened — surfacing overruns, findings, and recommendations.

## What It Does

- Fetches the full project record, all tasks, all time entries, and resource names from Autotask
- Extracts text from the signed scope-of-work PDF (via Azure Document Intelligence)
- Sends both to Hatz AI for a structured analysis: overall verdict, findings, per-task notes, and recommendations
- Renders an interactive on-screen report with charts and a task breakdown table
- Exports a standalone HTML file you can share or archive

## Running an Analysis

1. Open **Project Analysis** from the left nav
2. Enter the **project number** (e.g. `P20250826-0001`) and click **Fetch**
3. Optionally attach a **scope of work PDF** — paste a SharePoint URL or use the file picker to browse locally
4. Select the **AI model** to use (models are pulled live from Hatz AI) — your last choice is remembered automatically
5. Optionally click **Custom instructions** to add extra context for this run (e.g. "also weigh public effort-estimate data for similar work before concluding the project was under-scoped") — appended to the default prompt, never replacing it, and shown in the report so anyone reading it later knows the analysis was steered
6. Click **Run Analysis**

The fetch step takes 5–15 seconds. The AI analysis takes another 10–20 seconds depending on the model.

## Reading the Report

### Status Banner

The banner at the top shows the AI's one-line verdict color-coded by severity: green (good), blue (caution), amber (warning), or red (critical).

### Metric Cards

| Card | What It Shows |
|---|---|
| Billing type | Fixed Price, T&M, Block Hours, etc. — read from the contract name's `-FF`/`-TM` suffix, since ANS sometimes bills T&M-style engagements through Fixed Price contracts to use milestones. If no suffix is present, the card falls back to the raw contract type and flags it ("inferred from contract type, verify") instead of guessing silently. |
| Contract value | Agreed amount (from contract or milestone total) |
| Invoiced | Amount billed from completed milestones |
| Cost of labor | Actual hours × blended labor rate |
| Gross margin | Invoiced minus cost of labor (% and $) |
| Estimated hours | Hours estimated at project creation |
| Hours logged | Total actual hours with variance |
| Hours billed | Hours marked as billed in Autotask |
| Tasks not started | Count of tasks still at Not Started status |
| Resources | Number of team members who logged time |

### Charts

The charts card has four tabs:

| Tab | What It Shows |
|---|---|
| **By Task** | Estimated vs. actual hours per task — two parallel bars with a colored +Xh over / −Xh under badge |
| **By Resource** | Total hours per team member with share percentage |
| **By Category** | Hours by billing code from AT time entries (falls back to AI-inferred grouping) |
| **By Week** | Hours logged per calendar week — zero-activity weeks are shown as dimmed rows |

Hover any row for a tooltip with full task name, exact hours, and variance detail. Hovering a week also breaks that week's hours down **by category and by resource**, so a week that looks fully worked but was actually 100% project management with no technical work is visible at a glance — not just the total. The same per-week breakdown is printed in the exported report under each week's bar.

### Task Breakdown Table

A full table of every task with estimated hours, actual hours, variance %, status, and the AI's one-to-two sentence note about what happened on that task.

### Findings & Recommendations

AI-generated findings are color-coded by severity (critical / warning / good / info). Recommendations are scoped to the next similar project — what to scope differently, where to add buffer, what to watch for.

## Exporting

Click **Export Report** to download a standalone HTML file. The file is fully self-contained — no internet connection required to open it. It includes all four charts, the task table, findings, and recommendations.

The exported report has a **light/dark mode toggle** in the top-right corner. Your preference is saved per file.

## Settings

Click **Settings** to configure:

- **Blended Labor Rate** — used to calculate cost of labor and gross margin (default: $83.50/hr)
- **Default AI Model** — pre-selects the model each time you open the tool

## Recent Runs

The tool keeps a history of your last 20 analyses. Click any entry in the **Recent Runs** panel to reload the full report without re-fetching from Autotask or re-running AI.

## Tips

- The scope-of-work PDF is optional but significantly improves analysis quality — without it, the AI can only analyze the hours data and has no context about what was promised
- SharePoint URLs work directly — paste the link to the PDF in the ANS SharePoint and the tool downloads and extracts it automatically
- If a project has no billing codes on its time entries, By Category falls back to AI-inferred grouping based on task names; the source is labeled in the chart
- Gross margin only appears if the project has invoiced milestones in Autotask — T&M projects without milestones will show cost of labor but no margin
