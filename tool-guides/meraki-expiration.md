# Meraki License & EoL Expiration Report

Scan all Meraki client organizations for expiring licenses and hardware approaching end-of-life. Cross-references Autotask Configuration Items for date accuracy and lets you create renewal tickets or sync AT records without leaving the Hub.

Requires the **hub.it** or **hub.admin** role.

---

## Overview

The scan checks every device across all mapped Meraki orgs and classifies each one by urgency:

| Badge | Meaning |
|-------|---------|
| **Expired** | License or EoL date has passed |
| **Critical** | Expiring within the first third of your configured threshold |
| **Warning** | Expiring within your threshold |
| **Notice** | Expiring within 1.5× your threshold |
| **EoS** | Hardware end-of-support date is within range |
| **CO-TERM** | Organization uses co-term licensing — one shared expiry for all devices |

**Shelf spares** — unclaimed devices sitting in inventory with no license assigned are tagged "Shelf spare" and excluded from the Expired/Critical/Warning/Notice counts in the top summary banner and the Severity filter chips, since an unassigned spare isn't an actual license problem. They still show up under the **Status** filter group (**Shelf spare**) if you want to see them.

---

## Running a Scan

Click **Re-scan All Orgs** in the toolbar. A progress bar tracks the scan org by org (typically 30–90 seconds for 70+ orgs). Results are written to a shared cache in SharePoint so all Hub users see the same data without re-scanning.

**Last scanned** timestamp is shown at the top. The scheduler (configured in Settings) can run this automatically on a daily or weekly basis.

---

## Refreshing a Single Client

Waiting through a full 70+-org scan just to check one thing (a license you just claimed, an org setting you just fixed) is slow. Two faster paths update just that one org, in place, without touching anyone else's data or blocking the page:

- **↻ Refresh button** — every org card header has one, next to **+ License** and **🎫 Ticket**. Click it to rescan just that org.
- **"Refresh one client:" picker** — next to **Expand all / Collapse all**. Use this for an org that isn't currently showing as a row (hidden by an active filter or search) — pick it from the dropdown and click **↻ Refresh**.

Either way, only that org's entry in the shared scan cache is updated — the rest of the report stays exactly as you left it (filters, search, other expanded cards). There's no full-page scanning screen; a small status message next to the control shows progress and confirms when it's done.

A busy-guard prevents starting a second single-client refresh while one is already running in the same window. It does **not** protect against two refreshes (or a refresh and a full Re-scan All) started from different Hub instances/machines at the same moment — same shared-cache-file limitation as assigning unused licenses or syncing an AT warranty date elsewhere in this tool.

Single-client refreshes are logged to the Audit Log (see below) as their own entry, separate from full scans.

---

## Reading the Report

Results are grouped by client org. Click any org header to expand it and see individual devices.

**Filter bar** — click a badge to show only that severity. Click again to exclude those devices (red highlight). Multiple filters stack. **Clear Filters** resets all.

**Batch mode** — toggle **Batch** in the toolbar to select multiple orgs at once, then click **Create Tickets** to create renewal tickets for all selected orgs in one operation.

**Search** — type to filter across device name, serial, model, and org name in real time.

**Export CSV** — downloads the current filtered view as a spreadsheet.

---

## Device Actions

Expand an org row to see its devices. Each device row has action buttons on the right:

### Sync AT Date

For devices with an AT Configuration Item, a **Sync** button appears when the AT warranty/expiry date differs by more than 1 day from the Meraki license date. Click it to update the AT CI record to match.

### Create Ticket

Click **🎫 Ticket** to open the ticket form for that device. The tool checks for an existing open renewal ticket first — if one is found, you can add a note to it instead of creating a duplicate.

The ticket form lets you:
- Select devices to include (defaults to the current device)
- Set the assigned resource (defaults to the logged-in user)
- Add time entry details

All tickets are created in the **CS - Subscription Procurement** queue with the correct work type, SLA, and source fields populated automatically.

---

## Batch Ticket Creation

1. Click **Batch** in the toolbar to enter batch mode
2. Check the orgs you want to ticket
3. Click **Create Tickets**
4. Review the list of qualifying orgs and devices
5. Fill in the ticket details and submit

Each org gets its own ticket. Orgs without an AT company match or with no expiring devices under the current filters are skipped automatically.

---

## Exclusions

You can exclude devices or entire orgs from the report without removing them from Meraki.

**Exclude a device:** click the **···** menu on any device row → Exclude Device. The device is hidden from all future scans.

**Exclude a model globally:** click **···** → Exclude Model. This hides that hardware model across all orgs (useful for MT sensors and other non-client hardware).

**Exclude an org:** click **···** → Exclude Org. The entire org is skipped during scanning.

Exclusions are stored per-company in the Hub Company Directory and shared across all Hub users. To remove an exclusion, open **Settings → Exclusions** within the tool.

---

## Audit Log

Click **⚙** → **Audit** to see a history of activity for this tool: scan started/completed, client refreshed, license renewed, AT date synced, ticket created, and exclusions added/removed. A single-client refresh (see above) records its own **↻ Client refreshed** entry, distinct from the **✅ Scan completed** entry a full Re-scan All produces.

---

## Settings

Click the **⚙** gear icon to open the Expiration Report settings.

| Setting | Description |
|---------|-------------|
| **Warning threshold** | Days until expiry that triggers a Warning badge (default: 90) |

> **Global setting — shared by everyone.** Centrally managed via Azure App Configuration. The Settings screen shows the current threshold but the Save button won't apply a change — contact Mike (or whoever holds the `hub.admin` role) to update it.

The scan itself runs automatically every **Monday at 9:00 AM Denver time** — this schedule is fixed in the app and isn't user-configurable (a per-user schedule toggle used to exist but caused the scan to silently stop firing if everyone happened to leave it unchecked; a single fixed schedule guarantees it always runs). A desktop notification appears when it starts and completes. If the Hub was closed during the scheduled window, the scan runs automatically on the next launch.

---

## Company Mapping Requirement

The tool matches Meraki org names to Autotask companies using the **Meraki Orgs** tab in the Company Mapping tool. Orgs that aren't mapped show a **No AT Match** badge and cannot have tickets created or AT dates synced.

If you see unmatched orgs, go to **Company Mapping → Meraki Orgs** and use **Auto-Match** to link them.
