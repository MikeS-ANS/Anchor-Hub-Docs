# Autotask Contract Renewals

The Autotask Contract Renewals tool scans all active Autotask contracts and surfaces the ones expiring within your configured look-ahead window. Use it to proactively reach out to clients before contracts lapse and to identify which managed service agreements need renewal attention this month or next.

---

## Prerequisites

- Autotask credentials configured in Settings
- Contracts in Autotask must have an end date set to appear in results

---

## Configuration

Before running, confirm your renewal window in **Settings → Contract Renewals**:

- **Look-ahead days** — How far ahead to scan (e.g. 60 days shows contracts expiring within the next 60 days)
- **Contract types** — Filter to specific contract types if needed (e.g. Managed Services only)

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
