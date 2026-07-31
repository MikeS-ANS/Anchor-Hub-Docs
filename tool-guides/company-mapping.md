# Company Mapping (Company Directory)

Company Mapping — shown in the app as the **Company Directory** — is the foundation that connects every platform Anchor Hub touches back to its Autotask company: Pax8, Kaseya/Datto, Blackpoint, Cisco Meraki, the MSC revenue workbook, and CIPP (Microsoft 365 tenants). Every tool that needs to know "which Autotask company is this" — the Kaseya Invoice Processor, Blackpoint, User Audit Report, Client Touch Aging, and others — reads from this same directory instead of keeping its own copy.

Mappings live in **Azure SQL**, shared instantly across every signed-in user. There is no SharePoint file to sync and no per-tool re-mapping needed — confirm a match once and every tool sees it immediately.

---

## Companies Tab — the canonical mapping surface

The **Companies** tab lists every company Anchor Hub knows about, one row per Autotask company, sorted alphabetically by default (click a column header to sort by Status instead). Each row shows:

- The Autotask company name (click it to open the company in Autotask) and its Autotask classification
- A chip for each platform mapping (Kaseya, Blackpoint, Meraki, Pax8) — click **+** to attach one, click an existing chip to reassign or remove it
- MSC Name / CIPP Domain — single-value fields with the same attach flow
- Google Workspace vs. M365 toggle, and a SharePoint Folder field

### The shared attach flow

Every "attach a platform value to a company" interaction in the Hub — whether you're starting from the Companies tab, the Meraki Orgs tab, MSC Clients, CIPP Tenants, or Blackpoint's Confirm Company Match screen — opens the same small search popover: type part of a name, see live Autotask search results, click the right one to attach it. A successful attach shows a confirmation with an **undo** link; a failure shows the actual error with a **Retry**, never a silent no-op.

If the value you're attaching is already on a different company, the popover offers **Move here** instead of failing outright.

### Auto-provisioning

A nightly identity sync automatically adds every Autotask company matching your current filter settings (Account Type, Status, and Classification — configurable via **⚙ Sync Settings**, admin only) as a bare, unmapped row — so a brand-new Autotask client always has a directory row waiting for it, rather than only appearing once some platform's own match flow happens to create one. Use **⟳ Sync Now** to run it immediately instead of waiting for the nightly run (useful right after onboarding a new client).

A company outside your current filter settings (e.g. a vendor or a different account type) can still be added manually with **+ Add Company** — it won't be pruned if the filter settings change later, and any manual exclude/include you set on a company always wins over what the filter would otherwise decide.

---

## Meraki Orgs / MSC Clients / CIPP Tenants

These tabs each list one platform's own set of names (Meraki organizations, MSC revenue-workbook clients, CIPP-managed tenants) and let you map each one to an Autotask company using the same shared attach popover described above.

- **Meraki Orgs** keeps its confidence-scored Auto-Match suggestions (Accept ✓ / Skip ✕ per row) for the common case; the attach popover is the fallback when there's no good auto-suggestion or you need to search manually.
- **MSC Clients** and **CIPP Tenants** show a fuzzy name-match hint next to any unmapped row when there's no exact Autotask name match, as a starting point for the manual search.

---

## Pax8 Sync

Pax8's own review queue works differently from the tabs above — it's a confidence-scored **auto-match review queue**, not a manual point-and-click list:

1. Click **Sync Pax8** to pull current Pax8 companies and attempt automatic name matching against Autotask
2. Each result shows the Pax8 name, its suggested Autotask match (if any), and a confidence badge
3. **Accept** confirms a suggested match; **Find Match** opens manual search for anything unmatched or wrong; **Exclude** hides a Pax8 entry that isn't a real billing client (internal test accounts, reseller pass-throughs, etc.)

Accepted matches appear in the **Confirmed Company Mappings** table below, alongside **Confirmed Service Mappings** (Pax8 product ↔ Autotask service).

---

## Kaseya Org Mapping and Blackpoint Confirm Match

Both of these live inside their own tools — **Kaseya Invoice Processor** (via its prominent "→ Org Mapping" button) and **Blackpoint**'s Confirm Company Match screen — rather than being replaced by a link out to Company Directory. Both embed the same shared attach popover for manual matching, alongside their own tool-specific auto-suggestion system, so confirming a match there updates the same shared directory the Companies tab reads from.

---

## Time Projects

A separate tab links each company to the Autotask project/task used for tracking billable time against it (surfaced elsewhere in the Hub, e.g. User Audit Report's "+ Time" action). Each row shows the linked project's start/end dates — flagged if the project is past its end date — plus the company's Autotask classification. A per-client **Exclude** toggle marks a company as not needing time tracking without affecting its directory-wide excluded status.

---

## Excluding Companies

Toggle **Active/Excluded** on any Companies-tab row to hide it from audit results and push targets across every tool. This is separate from a platform-specific exclusion (e.g. excluding one Blackpoint customer from billing without affecting anything else about that company).

---

## Storage

Company data is stored in **Azure SQL**, accessed through the same Azure Functions API the rest of the Hub's shared data uses — not SharePoint. Core identity fields (Autotask ID/name, excluded status) are real columns; every other field (MSC Name, CIPP Domain, Google Workspace, SharePoint Folder, Autotask classification, linked time-tracking project, etc.) lives in a flexible metadata field, so new fields don't require a data migration to add. Every write touches only the one company being changed, not the whole directory, so two people editing different companies at the same time can't overwrite each other's changes.
