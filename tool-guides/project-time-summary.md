# Project Time Summary

The Project Time Summary pulls all open Autotask projects assigned to you, along with the time entries logged against each one. Use it to see where time is going across active projects, add notes for context, and share a formatted report with stakeholders.

---

## Prerequisites

- Autotask credentials configured in Settings
- Projects must be in an open/active status in Autotask to appear

---

## Running a Summary

1. Navigate to **Project Time Summary** in the left sidebar
2. Click **Run**

Anchor Hub queries Autotask for all active projects and their associated time entries. Depending on the number of projects and tickets, this typically takes 10–30 seconds.

---

## Reading the Results

Results are displayed as a table with one row per project:

| Column | Meaning |
|---|---|
| **Project** | Autotask project name and number |
| **Total Hours** | Sum of all time entries logged against this project |
| **Last Activity** | Date of the most recent time entry |
| **Notes** | Your notes for this project (editable inline) |

Click any column header to sort the table by that column.

---

## Adding Project Notes

You can add a note to any project row — useful for capturing status context, blockers, or next steps that aren't tracked in Autotask.

1. Click the **Notes** cell for a project
2. Type your note
3. Click away or press Enter to save

Notes are saved per project number and persist across sessions.

---

## Filtering Projects

If there are internal or overhead project numbers you want to exclude from the summary (e.g. admin or non-billable projects), add them to the exclusion list in **Settings → Project Time Summary**.

---

## Exporting

Click **Export** to download the summary as a formatted spreadsheet, including your notes column.

---

## Emailing the Report

Click **Email Report** to open a pre-formatted draft in your default email client. The report body includes the project table. Edit the recipients and subject before sending.

---

## Troubleshooting

**No projects appear after running**
Confirm you have active projects assigned in Autotask. If projects exist but don't show, check that your Autotask credentials are configured in Settings.

**A project you expect to see is missing**
It may be on the exclusion list in Settings, or its status in Autotask may be set to a closed or on-hold value. Check the project status in Autotask directly.

**Hours look wrong**
The summary reflects all time entries recorded in Autotask at the time of the run. If time was logged after you ran the summary, click **Run** again to refresh.
