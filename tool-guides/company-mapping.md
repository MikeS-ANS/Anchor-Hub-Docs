# Company Mapping (Company Directory)

Company Mapping — shown in the app as the **Company Directory** — is the foundation that connects every platform Anchor Hub touches back to its Autotask company: Pax8, Kaseya/Datto, Blackpoint, Cisco Meraki, Cytracom, Duo Security, BitDefender, Liongard, Lifecycle Manager X, the MSC revenue workbook, and CIPP (Microsoft 365 tenants). Every tool that needs to know "which Autotask company is this" — the Kaseya Invoice Processor, Blackpoint, User Audit Report, Client Touch Aging, Tool Inventory, and others — reads from this same directory instead of keeping its own copy.

The platform list is no longer fixed in code: each platform is declared once in a central registry, and this screen renders whatever that registry says. Adding a new vendor portal in future is a one-line entry rather than a code change here.

Mappings live in **Azure SQL**, shared instantly across every signed-in user. There is no SharePoint file to sync and no per-tool re-mapping needed — confirm a match once and every tool sees it immediately.

---

## Companies Tab — the canonical mapping surface

The **Companies** tab lists every company Anchor Hub knows about, one row per Autotask company, sorted alphabetically by default (click a column header to sort by Status instead). Each row shows:

- The Autotask company name (click it to open the company in Autotask) and its Autotask classification
- A chip for each platform mapping — click **+** to attach one, click an existing chip to reassign or remove it. There are nine platform columns: Kaseya / Datto, Cisco Meraki, Blackpoint Cyber, Cytracom, Pax8, Duo Security, BitDefender, Liongard, and Lifecycle Manager X. The last four are new, and are the ones Tool Inventory's unmapped accounts get mapped to.
  - **Pax8 is shown but not editable here** — it has no **+**. Those mappings are written by the Pax8 Sync on the Pax8 Sync tab, not by hand.
  - **Datto RMM has no column at all**, deliberately: it matches on the Autotask company id set inside Datto rather than on a name, so a name mapping for it would silently never take effect.
  - The table scrolls sideways at narrower window widths rather than squashing its columns.
- MSC Name / CIPP Domain — single-value fields with the same attach flow
- Google Workspace vs. M365 toggle, and a SharePoint Folder field

### The shared attach flow

Every "attach a platform value to a company" interaction in the Hub — whether you're starting from the Companies tab, the Meraki Orgs tab, MSC Clients, CIPP Tenants, or Blackpoint's Confirm Company Match screen — opens the same small search popover: type part of a name, see live Autotask search results, click the right one to attach it. A successful attach shows a confirmation with an **undo** link; a failure shows the actual error with a **Retry**, never a silent no-op.

If the value you're attaching is already on a different company, the popover offers **Move here** instead of failing outright.

### Auto-provisioning

A nightly identity sync automatically adds every Autotask company matching your current filter settings (Account Type, Status, and Classification — configurable via **⚙ Sync Settings**, admin only) as a bare, unmapped row — so a brand-new Autotask client always has a directory row waiting for it, rather than only appearing once some platform's own match flow happens to create one. Use **⟳ Sync Now** to run it immediately instead of waiting for the nightly run (useful right after onboarding a new client).

**Where companies come from.** Every company in the Hub comes from Autotask. There is no way to add one by hand: if it doesn't exist in Autotask as an active Customer, it doesn't exist in the Hub. For an urgent new client, use **Sync Now** and the nightly sync's rules apply immediately.

**Account Type matters now.** Each night (and on Sync Now), a company whose Autotask Account Type is no longer Customer — for example switched to Cancellation or Prospect during an offboarding — is hidden across every Hub tool, the same as if it had been deactivated. Switching it back to Customer brings it back the next night. Its mappings are never deleted. The sync log says why each company changed ("no longer Customer" or "inactive in Autotask"). The same pass now refreshes each company's Classification daily, so the Classification shown in the Companies tab no longer waits for the Update Classifications button.

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
