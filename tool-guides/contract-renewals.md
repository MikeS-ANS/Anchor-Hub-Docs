# Autotask Contract Renewals

The Autotask Contract Renewals tool scans all active Autotask contracts and surfaces the ones expiring within your configured look-ahead window. Use it to proactively reach out to clients before contracts lapse and to identify which managed service agreements need renewal attention this month or next.

---

## Prerequisites

- Autotask credentials configured in Settings
- Contracts in Autotask must have an end date set to appear in results

---

## Look-Ahead Window

Before running, pick your look-ahead window using the **30 / 60 / 90 days** tabs at the top of the tool — this is a per-run choice, not a saved setting, so it's remembered only for your current session.

## Settings

The tool's **Settings** tab holds two shared items:

- **Eligible Services for % Price Increase** — a list of service-name phrases (partial match, one per line); any matching service shows the % increase column at renewal
- **Renewal Info Prompt** — an AI prompt override used elsewhere in the renewal workflow

> **Global setting — shared by everyone.** Both are centrally managed via Azure App Configuration. The Settings screen shows the current values but the Save button won't apply a change — contact Mike (or whoever holds the `hub.admin` role) to update either one.

---

## Running a Renewals Check

1. Navigate to **Autotask Contract Renewals** in the left sidebar
2. Click **Run**

The tool queries all active Autotask contracts with end dates falling within your look-ahead window.

---

## Reading the Results

Results are grouped by expiration urgency:

| Section | Meaning |
|---|---|
| **Expiring this month** | Contracts with an end date in the current calendar month — act now |
| **Expiring next 30 days** | Contracts expiring within 30 days from today |
| **Expiring 31–60 days** | Contracts in the outer window — start renewal conversations |

Each row shows:

| Column | Meaning |
|---|---|
| **Company** | Autotask company name |
| **Contract** | Contract name and type |
| **End Date** | When the contract expires |
| **Value** | Annual contract value (if set in Autotask) |

---

## Exporting

Click **Export to Excel** to download the full renewal list as a spreadsheet, sorted by end date. Useful for sharing with account managers or scheduling renewal outreach.

---

## Troubleshooting

**No contracts appear**
No contracts have end dates within your look-ahead window, or Autotask credentials aren't configured. Check Settings and confirm the look-ahead window is set correctly.

**A contract you expect is missing**
The contract may be set to auto-renew in Autotask, or its status may be inactive. Contracts with no end date set will not appear in results.

**End dates look wrong**
Contract end dates are pulled live from Autotask. If a date was recently updated in Autotask, click **Run** again to refresh.
