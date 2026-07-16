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
3. Optionally attach a **scope of work PDF** — paste a SharePoint URL or use the file picker to browse locally. The file picker supports selecting multiple PDFs at once (or clicking it again to add more) — the first is treated as the Scope of Work and every additional file as Change Order 1, 2, etc. Drag the ⠿ handle on any attached file to reorder the list if the labels come out wrong (e.g. after a multi-select where the OS didn't return files in the order you expect)
4. Select the **AI model** to use (models are pulled live from Hatz AI) — your last choice is remembered automatically
5. Optionally click **Custom instructions** to add extra context for this run (e.g. "also weigh public effort-estimate data for similar work before concluding the project was under-scoped") — appended to the default prompt, never replacing it, and shown in the report so anyone reading it later knows the analysis was steered
6. Click **Run Analysis**

The fetch step takes 5–15 seconds. The AI analysis takes another 10–20 seconds depending on the model.

## Reading the Report

### Report Header

Shows the project number/name, client, and an **Open Project ↗** button that opens the project directly in Autotask's web UI — no extra lookup, it's derived from the project ID and Autotask zone already resolved during the fetch.

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

A full table of every task with estimated hours, actual hours, variance %, status, and the AI's one-to-two sentence note about what happened on that task. Each task's **phase** is shown as a sub-line under its title (in both this table and the By Task chart) since the same task name can appear under multiple phases on a project.

### Findings & Recommendations

AI-generated findings are color-coded by severity (critical / warning / good / info). Recommendations are scoped to the next similar project — what to scope differently, where to add buffer, what to watch for.

## Exporting

Click **Export Report** to download a standalone HTML file. The file is fully self-contained — no internet connection required to open it. It includes all four charts, the task table, findings, and recommendations.

The exported report has a **light/dark mode toggle** in the top-right corner. Your preference is saved per file.

## Settings

Click **Settings** to view:

- **Blended Labor Rate** — used to calculate cost of labor and gross margin (default: $83.50/hr) — shared with the same setting in Project Profitability
- **Default AI Model** — pre-selects the model each time you open the tool

> **Blended Labor Rate is a global setting — shared by everyone**, centrally managed via Azure App Configuration. The Save button won't apply a change to it — contact Mike (or whoever holds the `hub.admin` role) to update it. **Default AI Model is per-user** — it's remembered on your machine only, and saves normally.

## Recent Runs

The tool keeps a history of your last 20 analyses. Click any entry in the **Recent Runs** panel to reload the full report without re-fetching from Autotask or re-running AI.

## AI Prompt

Sent to Hatz.ai's `/chat/completions` endpoint (`main/ipc/projectAnalysis.js`, `pa-run-analysis` handler). Model is user-selectable (see Settings below).

**System prompt:**
> You are analyzing a completed IT project for an MSP. You are given (a) the signed scope of work — which may be followed by one or more Change Order sections that modify or extend the original scope, each clearly labeled — and (b) structured actuals from the PSA (tasks, estimated vs. actual hours, time-entry notes, billing type, and contract value if available). If change orders are present, treat the combined (original + change orders) scope as the baseline to compare actuals against — do not judge the project as under-scoped for work that a change order already accounted for. Compare what was scoped against what actually happened. Identify where the budget/hours were exceeded and why, where work went well, whether the project was under-scoped, over-billed, or lost time to unplanned issues. If a contract value is present, factor in the financial impact of any overrun. Base every statement on the provided data — do not invent facts. Assign each task to a small set of work categories.
>
> Return ONLY a valid JSON object with exactly these fields — no preamble, no markdown fences, no extra text:
> ```json
> {
>   "overallStatus":    "short verdict line",
>   "statusSeverity":   "good | caution | warning | danger",
>   "summaryNarrative": "2–4 sentence plain-English summary",
>   "categoryGrouping": [ { "category": "string", "taskIds": [ <taskId numbers> ] } ],
>   "perTaskNotes":     { "<taskId>": "1–2 sentence explanation" },
>   "findings": [
>     { "severity": "critical | warning | good | info", "title": "short title", "body": "1–2 sentences" }
>   ],
>   "recommendations": [
>     { "title": "short action title", "body": "1–2 sentences on what to do differently on the next similar project" }
>   ]
> }
> ```

If you filled in **Custom Instructions** for the run, it's appended after this (never replacing it): `ADDITIONAL INSTRUCTIONS FOR THIS ANALYSIS (apply on top of the above, do not replace it):` followed by your text.

**User message:** `SCOPE OF WORK:` followed by the extracted scope text (or `(No scope document provided)`), then `PROJECT DATA:` followed by the full project data object (tasks, time entries, hours, billing info, etc.) as pretty-printed JSON.

The response is expected as raw JSON (markdown fences are stripped defensively if present) and parsed directly into the report.

## Tips

- The scope-of-work PDF is optional but significantly improves analysis quality — without it, the AI can only analyze the hours data and has no context about what was promised
- SharePoint URLs work directly — paste the link to the PDF in the ANS SharePoint and the tool downloads and extracts it automatically
- If a project has no billing codes on its time entries, By Category falls back to AI-inferred grouping based on task names; the source is labeled in the chart
- Gross margin only appears if the project has invoiced milestones in Autotask — T&M projects without milestones will show cost of labor but no margin
