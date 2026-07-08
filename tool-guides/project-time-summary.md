# Project Time Summary

The Project Time Summary pulls all active Autotask projects and their time entries, adds AI-generated insight via Hatz AI, and delivers a formatted report directly to a recipient's inbox — with a copy saved to SharePoint automatically.

---

## First-Run Setup

Before running a report, expand and fill in the four settings panels at the bottom of the page. **Click any panel header to expand it.** All settings are saved locally per machine.

### 1. Email Settings *(required to send reports)*

| Field | What to enter |
|---|---|
| **To** | Recipient email address (e.g. the PS manager) |
| **Subject** | Email subject line |
| **Intro Line** | Opening sentence of the email body — the AI summary and SharePoint folder link are appended automatically |
| **AI Model** | Select a Hatz AI model from the dropdown — defaults to the first available model |

Click **Save Settings** when done.

### 2. Always Exclude Projects *(optional)*

List any Autotask project numbers (e.g. `P20260101.0001`) to permanently hide from the table and all reports — one per line or comma-separated. Useful for internal overhead or non-billable projects that should never appear.

Click **Save & Re-fetch** — this saves the list and immediately re-runs the fetch so excluded projects disappear right away.

### 3. Project Filters *(required if projects aren't appearing)*

Controls which projects Autotask returns. These must match the **exact label** used in Autotask.

| Field | What to enter |
|---|---|
| **Exclude Statuses** | One status label per line (e.g. `Complete`, `Canceled`, `Inactive`) — projects with these statuses are skipped |
| **Department Filter** | Limit results to one department (e.g. `Professional Services`) — leave blank for all |
| **Project Type Filter** | Limit by project type (e.g. `Client`) — leave blank for all |

**Read-only API account?** If the department/type filters aren't working, click **Resolve IDs** — this looks up the numeric IDs once and saves them as overrides that bypass the admin-only Departments endpoint.

Click **Save Filters** when done.

### 4. Auto-Send Schedule *(optional)*

Set a recurring day and time to automatically fetch projects, generate an AI summary, and email the report — with no manual action required.

| Field | What to enter |
|---|---|
| **Enable auto-send** | Check to activate the schedule |
| **Day of Week** | Day to send (e.g. Monday) |
| **Time** | Hour and minute in 24-hour format, system timezone |

Click **Save Schedule** when done.

> **Note:** The schedule runs inside the Hub application. The app must be open and running at the scheduled time for the send to trigger. The email goes out from the account logged into the Hub.

---

## Running a Summary

1. Navigate to **Project Time Summary** in the left sidebar
2. Click **Run**

The Hub queries Autotask for all active projects matching your filters and loads their time entries. This typically takes 10–30 seconds depending on project count.

---

## Reading the Results

Results display as a sortable table — click any column header to sort. Click again to reverse.

| Column | Meaning |
|---|---|
| **Account** | Client account name |
| **Project** | Autotask project name and number |
| **Est. Hours** | Total estimated hours on the project |
| **Worked** | Hours logged to date |
| **Last 7d** | Hours logged in the past 7 days |
| **Δ Week** | Change in worked hours vs. the prior day's snapshot — shows whether the project was active this week |
| **Burn Rate** | Estimated weeks of work remaining based on the last 7 days' pace — shows **Stalled** if no recent activity, **Over** if already over budget |
| **Last Active** | Date of the most recent time entry — highlighted yellow if over 14 days, red if over 30 days |
| **Notes** | Your notes for this project (editable inline) |

### Row color coding

| Color | Meaning |
|---|---|
| Yellow tint | Worked hours are ≥ 50% of estimated |
| Red tint | Worked hours exceed estimated (over budget) |
| Green tint | Active in the last 7 days |

### Metric cards

Above the table, summary cards show: total active projects, total hours worked, stalled projects (no activity in 14+ days and no recent pace), and a Needs Attention count.

---

## Needs Attention Filter

Click **Needs Attention (N)** in the action bar to toggle the table to show only projects that are stalled or over budget. Click again to show all projects. The button label updates with the current count.

---

## Adding Project Notes

Notes are useful for capturing status context, blockers, or next steps that aren't tracked in Autotask. Notes are included in the AI summary and in the emailed report.

1. Click the **Notes** cell for any project row
2. Type your note
3. Click away or press Enter to save

Notes persist across sessions and are keyed to the project number.

---

## AI Summary

After the table loads, Anchor Hub automatically generates an executive summary using Hatz AI. The summary appears in the **AI Summary** card above the table.

To regenerate after adding or editing notes, click **Re-run with Notes** in the AI card. This sends the current notes alongside the project data for a more contextual summary.

---

## Sending the Report

Click **Email Report** in the action bar. The Hub will:

1. Generate a formatted HTML report table
2. Upload it to the **ANS-Professional Services → Project Time Summary Reports** folder in SharePoint
3. Send an email to the configured recipient with the AI summary, the full project table embedded in the email body, and a link to open the SharePoint folder

> The SharePoint link opens the **folder** (not a direct file link) because SharePoint Online forces HTML files to download rather than preview in the browser.

A green **Open in SharePoint ↗** link appears in the action bar once the upload is complete.

---

## Troubleshooting

**No projects appear after running**
Check that your Autotask credentials are configured in Settings. Confirm you have active projects in Autotask. If projects exist but don't show, verify your **Project Filters** — the status, department, and type labels must match Autotask exactly.

**A project you expect is missing**
It may be on the Always Exclude list, or its Autotask status may match one of your excluded statuses. Also check that the department/type filter isn't too narrow.

**Burn Rate shows "Stalled"**
The project has no time logged in the last 7 days. This doesn't mean the project is on hold — it may simply have had no recent entries.

**Δ Week shows nothing**
Week-over-week delta compares against a saved daily snapshot. Snapshots are created each time you run the summary. If you've only run it once, there's no prior snapshot to compare against — the delta will appear after the second run.

**Email fails to send**
Confirm Email Settings are saved and the To address is valid. The Hub sends via Microsoft Graph using your logged-in account — if your Graph token has expired, log out and back in to refresh it.

**Schedule isn't sending automatically**
The Hub must be open at the scheduled time. If the app is closed or the machine is asleep, the send won't trigger.
