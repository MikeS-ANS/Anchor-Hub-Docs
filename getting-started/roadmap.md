# Anchor Hub — Roadmap

> Living document. Add tool ideas here so nothing gets lost. Update status as things move.
>
> **Vision:** Enterprise-grade internal platform. No "simplest option" shortcuts — build it the way a SaaS company would. Azure is available and should be used properly.

---

## ✅ Shipped

| Tool / Feature | Version |
|------|---------|
| Subscription Audit | v1.0 |
| Invoice Monitor | v1.0 |
| Margin Analyzer | v1.1 |
| Company Mapping | v1.1 |
| Pax8 Invoice Comparison | v1.2 |
| Kaseya Invoice Processor | v1.2 |
| Pax8 Invoice Processor | v1.2 |
| Contract Changes | v1.2 |
| Contract Renewals | v1.2 |
| BlackPoint Invoice Processor | v1.2 |
| Project Time Summary | v1.3 |
| MSC Agreements | v1.3 |
| Sidebar Customization | v1.3 |
| Microsoft SSO / Entra ID login | v1.4 |
| Contract Name column (Project Time Summary) | v1.4 |
| Microsoft profile photo in user chip | v1.4 |
| Duo Management | v1.4.1 |
| Fuzzy company auto-match + push history (Invoice Processor) | v1.4.2 |
| Project Profitability | v1.4.3 |
| Role-based access & permissions (hub.admin / hub.it / hub.standard / hub.readonly) | v1.5.3–1.5.6 |
| Modular IPC restructure (main/ipc/) | v1.5 |
| Pax8 & Autotask credentials → Azure Key Vault | v1.4.2 |
| BlackPoint billing automation redesign — push confirmation, audit log, export | v1.5.2 |
| Home Screen daily start page | v1.5.7 |
| Help page overhaul + in-app support | v1.5.7 |
| Kaseya Invoice Processor full rebuild — SharePoint-native, QBO journal entries, bundled billing | v1.5.8 |
| Centralized Hub Company Directory (SharePoint JSON, shared across all tools) | v1.5.9 |
| Company Directory v2 — inline chip editing, AT classifications, multi-service mappings | v1.6.0 |
| Meraki Admin Management — audit, gap analysis, bulk fix, org matching tab | v1.7.0 |
| Meraki License & EoL Expiration Report — scan, tickets, AT sync, scheduled auto-scan | v1.8.0 |
| Company Directory Extension — MSC/CIPP matching tabs, GWS flag, sharepointFolder, CIPP API | v1.9.0 |
| HR & Resources Portal — flip-card benefits, HR contacts, Hatz.ai agent chat, admin edit mode (Key Vault-backed) | v2.0.0 |
| Project Analysis — scope-vs-actuals AI analysis, charts, exportable report (initial release) | v2.0.0 |
| Project Analysis — chart overhaul (By Task/Resource/Category/Week, hover detail) | v2.1.0 |
| Invoice Processor overhaul + Project Time Summary improvements | v2.2.0 |
| User Audit Report — AT × M365 (CIPP) join, client-facing Excel, write-back, Account Manager dashboard | v2.3.0 |
| Hub Directory migration (retired local mapping cache), Meraki scheduler fix, Pax8/billing accuracy fixes | v2.4.0 |
| Values & Norms — daily home screen widget + library, AI-enriched blurb, SharePoint-backed content | v2.5.0 |
| Project Analysis — multi-PDF change order support, task phase display, Open Project button, billing-type fix | v2.6.0 |
| Repo privacy prep — tenant config extraction, private-repo-aware auto-updater | v2.7.0 |
| SharePoint installer mirror, Open Project button fixes, Client Time Tracking project creation | v2.8.0 |
| **Sprint 2 — Azure backend migration**: shared storage (Company Directory, per-tool run history/snapshots, User Audit history, cross-tool audit log) moved from SharePoint-JSON files to Azure SQL via a dedicated Azure Functions API, authenticated per-user via Entra ID; centrally-managed settings moved to Azure App Configuration with an admin-gated write path (`hub.admin`); Application Insights telemetry confirmed live | v3.0.0 |
| **Ideas & Bug Tracker** — first Hub feature built purely against the Sprint 2 Azure SQL + Functions pattern (not a migration). Pinned in the sidebar between Help and Settings. Any signed-in user can submit, vote on, comment on, and view ideas/bugs; `hub.admin` can change status, post Official comments, merge duplicates, and delete items (admin cleanup utility, not in the original spec). In-app notifications bell for status changes/comments on things you follow. Attachments upload directly to SharePoint via Graph. Built from Claude Design's mockup handoff — the first feature to go through the Claude Design → Claude Code pipeline | v3.1.0 |
| **Timesheet Review Tool** *(marked Beta in-app — gathering manager feedback before general release)* — pulls Autotask time entries for a resource or team, computes client-facing utilization %, and runs deterministic data-quality checks (Gap/Overlap/Late/Thin Note/After Hours/Note-Duration Mismatch, all admin-configurable thresholds). AI Addendum pattern-tiering layer classifies each check's recurrence as CONCERN/WATCH/NORMAL via fixed thresholds computed once per scan — Manager Review, Team Review, and Ask AI all narrate those same tiers instead of independently judging severity, so results are consistent across features. Scoped by Graph direct reports (`hub.admin` sees everyone); results cached in Azure SQL so reopening a previously-scanned period is instant. Links out to Autotask for actual timesheet approval — no in-Hub approval (Autotask API limitation) | v3.2.0 |
| Timesheet Review — fixed team scoping for non-admin managers: `/me/directReports` needs `User.ReadBasic.All`, not `User.Read` (the "/me" prefix doesn't exempt reading other users' relationship data). Requires signing out and back in once to pick up the new consented scope | v3.2.1 |
| **Bug fix batch** — User Audit Report header overlap; duplicate taskbar icon when pinned (AppUserModelId mismatch); announcement dismissal now persists (was session-only) plus a non-dismissible announcement type; sidebar section collapse now persists (was in-memory only); AI Prompt tab removed from global Settings, moved into Contract Renewals' own settings; HR benefit card and Home calendar long-URL overflow fixed; Contract Changes filter dropdowns no longer render behind/clickable-through the summary strip (same `.content > *` z-index root cause as the Ideas & Bug Tracker fix, audited every other tool for the same pattern — none found). Settings now opens to the Customize tab by default. **New:** Markdown export for Ideas & Bugs (select 1-to-all, includes attachments/comments); export/import for sidebar customizations + personal quick links, so moving to a new computer or reinstalling doesn't lose them | v3.2.2 |
| Help page's Tools Overview was silently missing 8 shipped tools (Project Analysis, Meraki Admin Management, Meraki License & EoL Expiration, User Audit Report, Timesheet Review, HR & Resources Portal, Values & Norms, Ideas & Bug Tracker) — added, and a release-checklist step added so new tools don't drift out of this list again | v3.3.0 |
| User Audit Report — data-load failures (e.g. missing SharePoint permission to the MSC workbook) now surface as a clear warning banner on the AM Dashboard and Generate tab instead of silently rendering an empty list indistinguishable from "you just own no clients" | v3.3.0 |
| **Ideas & Bug Tracker** — sort dropdown (Priority/Newest/Status/Alphabetical/Created By); terminal-status items (Shipped/Fixed/Declined/Won't Fix/Duplicate) collapse into a "Closed" section by default instead of cluttering the main list; per-user "hide from my view" (local-only, not team-visible) with a collapsible "Hidden" section to unhide. Also: spell-check right-click menu was missing entirely (Chromium's underline-detection worked, but Electron needs its own context-menu wiring for suggestions) — added | v3.3.0 |
| **"Who's Using the App"** — new Installs tab in Settings (`hub.admin` only): name, machine, version, last seen, first seen, sorted most-recent-first. Surfaces the install-heartbeat data every launch already wrote to Azure SQL since Sprint 2, but nothing ever read. Also closed a real gap found while wiring it up: the backend's `GET /installs` route had no role gate at all — added `hub.admin` enforcement server-side, matching every other admin-only route | v3.3.1 |
| **Kaseya Invoice Processor** — billing-accuracy and push-reliability pass: workplace bundle-override client-name matching now normalizes legal suffixes/punctuation instead of requiring an exact string; SaaS bundled-flag saves batched into one round trip (was racing on rapid checkbox clicks and silently dropping changes); SaaS Protection push/preview now targets only the AT service matching each client's actual platform (GSuite vs M365) instead of creating a duplicate line on the platform they don't use; multi-domain clients (Kaseya bills separate domains under one AT company) now aggregate correctly instead of double-counting bundle allowances and overwriting each other's push; AT contract selection now picks whichever contract's date range actually contains the effective date instead of whichever starts latest; first-time service-line attachment now goes through `ContractServiceAdjustments` (the previous endpoint has no working create path in this Autotask API version). Also added: a live current→new preview before every push, "Last pushed" timestamps under the push buttons, a load-warning banner for silent MSC/Revenue sheet failures, and parallelized AT lookups for faster large-invoice pushes. **New:** drag-and-drop (with click-to-browse fallback) invoice upload straight into SharePoint, added to both the Kaseya and BlackPoint invoice processors | v3.4.1 |
| **Meraki License Management** — single-client refresh: a per-org "↻ Refresh" button plus a client-picker dropdown rescans just one org in place (read-modify-write against the shared scan cache), instead of always running the full "Re-scan All" across every client org. Own `ORG_RESCAN` Audit Log entry. Also fixed the top summary banner and Severity filter chips counting unclaimed "shelf spare" devices as license problems (they have no license assigned by design) — now excluded there, matching how the HTML export already treated them | v3.4.2 |
| **User Audit Report** — write-back duplicate-contact safeguard: email-only matching missed the case where an Autotask contact's email had drifted from a person's current M365 address even though their name clearly hadn't, risking a duplicate contact on create. M365-only rows with a likely name match (last name + first-name prefix/Levenshtein-2 fuzzy match against the client's own Autotask contacts) now show a warning badge in the write-back preview and require an explicit per-row "Update existing" vs. "Create new anyway" choice — no auto-resolution for a likely duplicate. A live re-check against Autotask's current contacts runs immediately before every create (not just against the possibly-stale generated report), so a match that only exists in Autotask today is blocked with a "needs review" status instead of silently creating a duplicate | v3.4.3 |
| **Strety Rocks & To-Dos** — personal, read-only home-screen widget pulling the signed-in user's open Strety Rocks and To-Dos, via a per-user OAuth connection (loopback flow, Key Vault-backed refresh-token storage). Shows on-track/at-risk/off-track rock counts, an open to-do count, and the next 5 to-dos sorted by due date, with an inline "Connect Strety" prompt when not yet linked. Home's widget row goes from one column to two to fit it alongside the existing Calendar widget. A homepage widget like Calendar/Quick Links/Values & Norms, not a standalone navigable tool — no Help-page tool card | v3.5.0 |
| Strety Rocks & To-Dos — fixed a live bug where concurrent refresh-token races (Strety rotates refresh tokens on use) could burn the shared token twice, and a failed Key Vault write-back of the rotated token was silently swallowed; once the token went dead, the widget had no way back to Connect since `strety-check-connected` only checked whether a token existed, not whether it still worked. Concurrent refresh calls now dedupe onto one in-flight grant call, a failed write-back is logged instead of swallowed, and a confirmed-dead token (400 on refresh) is cleared from Key Vault so the widget self-heals back to the Connect strip within the same page load | v3.5.1 |
| Strety Rocks & To-Dos — the v3.5.1 self-heal still depended on clearing the dead token in Key Vault, which silently fails for any user without Key Vault write permission (most non-admins), leaving them stuck on "Strety unavailable" with no Connect button, same as before v3.5.1. The widget now trusts the `needsReconnect` signal directly and shows the Connect button regardless of whether the Key Vault clear succeeded, via a shared `_renderStretyConnectStrip()` helper used by both the "never connected" and "connection went dead" paths. *(Open follow-up, not resolved here: whether non-admin users should get Key Vault write access to the Strety refresh-token secret, or refresh should be centralized to an admin-only process.)* | v3.5.2 |
| Strety Rocks & To-Dos — fixed a related but distinct bug: the cached access token could be invalidated by Strety mid-session (e.g. another user's app rotating the shared refresh token before our own expiry bookkeeping expected it), producing a raw "401 INVALID_TOKEN" error with no recovery, even right after a successful reconnect. `stretyGetUrl()` now treats a 401 on the actual data call as a signal that the cached token is stale: it drops the cache, forces one real refresh, and retries once; if still invalid after that, it's tagged with the same `needsReconnect` signal so the widget offers Connect instead of a raw error. *(Open follow-up, still not resolved here: whether non-admin users should get Key Vault write access to the Strety refresh-token secret, or refresh should be centralized to an admin-only process — now clearly the root cause behind all three of these patches, multiple independent app instances racing on one shared, unsynchronized credential.)* | v3.5.3 |
| **Duo Management** — access check still referenced the retired `hub.it`/`hub.standard` Entra roles from before the Hub Role Matrix migration (see Sprint 4 below); the sidebar already used the new 9-role matrix, so Manager/Delivery/Tam/Strategic users could see the tool but were blocked the moment they opened it. Now checks the current matrix roles (Admin/Manager/Delivery/Tam/Strategic) directly, and grants all of them full tab access instead of the old admin-tab/client-tab split | v3.6.1 |
| **Client Touch Aging Report** — Flags MSC MRR+ORR clients whose Autotask CompanyNotes touches have gone stale: no Note/Quick Note in 3 months, or no Client Meeting in 12 months (both admin-editable thresholds), plus a **due-soon early-warning tier** (admin-editable `dueSoonDays`, default 14) that flags clients approaching — not yet past — a threshold, so the client list is broader than just outright breaches. Sidebar tool key `client-touch-aging`, marked Beta. Report tab: Table/Cards/By Account Manager layout switcher (persisted per-user in localStorage; Cards has a 5th "Due Soon" lane, Table/By AM show a muted due-soon badge), a "My clients / All clients" scope toggle, search-by-name + AM filter, a 5-tile metric strip (Flagged/Never touched/Note overdue/Meeting overdue/Due soon), and a "Data as of [timestamp]" + manual "Check for new data" sync button (idle/running/failed states) — only flagged/due-soon clients are ever shown, with a quiet "Every client is current" all-clear state otherwise. Every client also gets a computed **Priority score** (weighting contract renewal proximity, active contract value, and recent Autotask ticket volume on top of severity), sortable as its own Table column and shown as a "★ N" badge with inline 📅/💰/🎫 signal icons in all three views — the report's default sort is now "highest priority first" rather than "longest since any touch." A **"Log a touch"** button on any row opens a modal (Note vs. Client Meeting + free-text note) that posts a real Autotask CompanyNotes entry directly from the Hub, reflected immediately via an optimistic cache update — a deliberate, explicitly-confirmed exception to this tool's original read-only design, gated behind the user's personal Autotask API key (same gate as User Audit Report's write-back), showing visibly disabled with an explanation rather than failing silently if that key isn't configured. Client names link out to their Autotask company record. Settings tab (admin-only sections carry an Admin pill, enforced server-side): Thresholds (including the due-soon window), Touch Types (picklist-driven, not raw ID entry), Excluded Clients, Daily Sync time, and a Scheduled Digest (recipients + Off/Weekly/Monthly, editable by any signed-in user) with a Preview/Send-now modal. Never queries Autotask live at view time — served from an Azure SQL cache (`company_touches`/`touch_sync_state`/`company_priority_signals`/`priority_sync_state` tables, migrations `functions/sql/phase10`–`phase12`) populated by a daily incremental sync that batches Autotask CompanyNotes/Contracts/Tickets queries by companyID to avoid the rate-limit fan-out those entities are prone to, plus the manual sync button running the same logic (`main/ipc/clientTouchAging.js`). No AI/LLM component anywhere — purely deterministic threshold logic and weighting. Extracted two shared helpers along the way so User Audit Report and this tool share code instead of duplicating it: `main/shared/accountManager.js` (Account Manager resolution) and generalized Autotask deep-link helpers in `app.js` (`atAccountUrl`/`atClientLink`/`wireAtLinks`). **Open follow-ups to confirm with Corey (the requester):** whether staff have actually been logging touches under a stray "Notes" (plural) Autotask action type defensively included in the default Note/Quick Note bucket, whether the non-live "data as of yesterday" framing works for day-to-day use, and whether the priority score's ticket-volume weighting direction (more tickets = higher priority, treated as a relationship-risk signal) holds up against real usage. **Known trade-off, not fixed here:** "Log a touch" (and the priority-signals sync) shares this whole class of Sprint-2 cache-only endpoint's pre-existing trust model — any signed-in Hub user's valid token can technically call the sync-persistence endpoint directly and mark a client "touched" without the real Autotask write happening (the Electron UI itself always does this correctly; this is about bypassing the UI entirely) — not a new architecture gap, but the first feature in this family where a single fabricated record, not just a mirror of real Autotask data, could be pushed through it | v3.6.0 |
| **Company Directory Rebuild** — company-to-platform mapping (Kaseya, Blackpoint, Meraki, MSC, CIPP, Pax8) consolidated onto one shared "search Autotask, click to attach" component (`AttachPicker`), replacing four independent bespoke search-and-attach implementations that had silently diverged — including a live class-collision bug where confirming a Kaseya Org Mapping match also mis-fired the Pax8 tab's click handler and could write a corrupt entry into that company's Pax8 mapping data, fixed as part of this work. **Companies tab** is now the canonical mapping surface: sortable by Autotask Company/Status (defaults alphabetical), a sticky Autotask Company column, inline Autotask classification badge, clickable company names linking to Autotask, and the shared attach/reassign/remove flow now covers every platform plus MSC Name/CIPP Domain/Google Workspace/SharePoint Folder. **Auto-provisioning**: a new identity sync job (admin-configurable Account Type/Status/Classification filter settings, nightly + manual "Sync Now") keeps the directory current with Autotask automatically — closing the originally-reported bug where an Autotask company findable via search silently failed to map because it had no directory row yet — plus a manual "+ Add Company" action for anything outside the filter. **Meraki Orgs/MSC Clients/CIPP Tenants/Blackpoint Confirm Match** all fold onto the same shared component (MSC/CIPP get a fuzzy name-match hint when there's no exact Autotask match; Meraki/Blackpoint keep their existing confidence-scored auto-match systems — only the manual fallback changed). **Time Projects** tab gained Autotask classification, linked project start/end dates with a past-end-date flag, and a per-client exclude toggle. **Kaseya Invoice Processor** gained a prominent "Org Mapping" shortcut so its company-mapping screen is easy to find. Backend: company data moved off individual scalar SQL columns onto one JSON `metadata` column (new fields need no migration going forward); every write handler moved from a full-directory read-modify-write to scoped per-company endpoints, closing a race window where two near-simultaneous saves could silently overwrite each other's changes; the legacy SharePoint-JSON fallback path (`hub-company-mappings.json`) is fully retired in favor of Azure SQL. **Also fixed, found during testing (pre-existing, unrelated to the rebuild):** the bulk directory read was silently missing an internal format-version marker, so every `isV2()` check in `main/ipc/companyMapping.js` (9 call sites) took the wrong branch — Pax8 Sync's review rows showed "undefined" instead of company names, and clicking Accept silently failed to persist the match. Any Pax8 match "Accepted" recently should be spot-checked, since it may not have actually saved | v3.6.2 |

---

## 🔄 Next Up

These tools have detailed specs written and are ready to build. See the `Idea MD Files/` folder for the full spec on each.

- [ ] **Newsletter Builder** *(spec: `NEWSLETTER_BUILDER_SPEC.md`)*
  Compose and send formatted internal company newsletters from within the Hub. Section-based form (Announcements, Team Highlights, Project Wins, Tech Tips, etc.), live HTML preview, and delivery via Microsoft Graph `Mail.Send` to a distribution group. Stores sent history with duplicate-and-edit support.
  *`Mail.Send` Graph scope is already active ✅*

- [ ] **Tool Inventory & License Count Tracker** *(spec: `TOOL_INVENTORY_LICENSE_TRACKER_SPEC.md`)*
  Replaces the manual quarterly tool inventory spreadsheet (48 tabs back to 2014) with a monthly per-client license/seat reconciliation report — actual vendor usage (RMM, Duo, SaaS, Pax8-sourced licensing, and phased-in CyberQP/Splashtop/Liongard/LifeCycle Insights) vs. each client's MSC-contracted baseline across all four MSC sections (MRR/TCC/ORR/CoM), flagging both quantity mismatches and clients using tools with no MSC record at all. Read-only in Phase 1 (no Autotask push); Excel workbook kept as a monthly-written archive, not retired. Admin/Finance/Strategic access only.

- [ ] **Employee Scorecard** *(spec being written — see below, do not resume the old branch until it lands)*
  Replaces the manual monthly Excel scorecard (7 employee tabs). Intended to pull metrics from Autotask and Strety per employee, prompt for any metrics that can't be auto-pulled (CE credits, client touches), and send each person their scorecard on a schedule (email/Teams).

  A partial implementation exists on `feature/employee-scorecard` (Strety OAuth + a generic editable metrics grid, no Autotask pull, no per-role metric definitions, no delivery scheduling) but it's unmerged and ~15 releases stale. **A proper spec is in progress with Mike before any further build work** — see `!Idea MD Files/EMPLOYEE_SCORECARD_STATUS_REPORT.md` for the full reconciliation of what exists vs. what was ever specified.

  | Employee | Key Metrics |
  |---|---|
  | Director (Brian French) | TSMs (≥5), hours, revenue, cabling/hardware profit, cont. ed |
  | Client Experience (Susan Castle) | Unique touch points (75/mo), proactive visits (40/yr), trainings (4/yr) |
  | TAM (Cody Mead) | Project survey avg (≥4.5), site reviews (8-10/mo), tickets, onboard projects |
  | Pro Services Mgr (Andi Gingrich) | Team survey avg, project on-time %, revenue billed |
  | WSD (Michael Nolan) | CSAT (≥98%), tickets, hours |
  | Service Desk Mgr (Patrick Kiah) | CSAT, tickets, hours, certifications |
  | Cybersecurity (Andy Harper) | Project survey avg, site reviews, tickets, onboard projects |

---

## ✅ Invoice Processor — Near-Term Queue

- [x] **Pax8 API key → Key Vault** — `pax8-client-id` / `pax8-client-secret` in `anchor-hub-vault`. *(shipped v1.4.2)*
- [x] **Autotask two-tier key** — shared read-only key in Key Vault. Personal write key in keytar. `atFetch` checks keytar first, falls back to KV automatically. *(shipped v1.4.2)*
- [ ] **Push error logging** — persist a structured log entry for every AT push run: timestamp, service type, effective date, per-company result (success / skipped / error). Show a "Push History" panel. Long-term: admin notification via Teams webhook when any company is skipped due to missing mapping.
- [ ] **QBO auto-push** — connect QuickBooks Online API to directly update the QBO account breakdown instead of copy-paste. OAuth flow for QBO credentials stored in keytar. Same pattern as Autotask push: per-account buttons, effective date picker, progress modal with per-line results.

---

## 🏗 Platform Priorities

---

### Sprint 1 — Foundation

- [x] **Microsoft SSO / Entra ID** *(shipped v1.4.0)*
  MSAL PublicClientApplication with file-based token cache. Roles defined in Entra ID (`hub.admin`, `hub.standard`, `hub.readonly`). Profile photo pulled from Microsoft Graph.

- [x] **Modular file restructure** *(shipped v1.5.0)*
  `main/ipc/` is fully split into per-tool files. `main.js` is under 200 lines.

- [ ] **Intune / MDM deployment**
  Package as Win32 app and push via Intune so it auto-installs on every new employee computer. Solves Windows SmartScreen for all managed employee machines immediately — no cert required for internal distribution. (Datto RMM Quick Job deployment was considered and dropped — tenant sharing policy blocks anonymous SharePoint links needed for unattended downloads; manual SharePoint download covers the personal-device case, Intune covers new-computer provisioning.)

- [ ] **Make GitHub repo private + extract sensitive config**
  The public repo currently contains ANS's Azure AD Tenant ID, Client ID, SharePoint hostname, and internal email addresses in plain text.
  - [x] Extract `MSAL_CLIENT_ID`, `MSAL_TENANT_ID`, `SP_HOST`, `SUPPORT_TO`, and a scoped GitHub PAT into a gitignored `config.js` *(v2.7.0)*
  - [x] Configure electron-updater to use the PAT once the repo goes private *(v2.7.0 — shipped ahead of the flip so every installed app has the PAT-aware code before visibility changes)*
  - [x] SharePoint installer mirror for first-time installs on machines with no GitHub access (personal devices, no RMM/Intune) *(v2.7.0)*
  - [ ] Flip repo to private in GitHub settings — waiting for v2.7.0 to propagate to all installs first

---

### Sprint 2 — Azure Backend

Replace remaining local-only storage with a proper centralized backend.

**Planned in detail** — see [`!Idea MD Files/SPRINT_2_BACKEND_SPEC.md`](!Idea%20MD%20Files/SPRINT_2_BACKEND_SPEC.md) (full spec: verified storage placement per file, SQL schema, Functions endpoints, App Config keys, auth model, and a 7-phase migration order), backed by the read-only [`SPRINT_2_STORAGE_INVENTORY.md`](!Idea%20MD%20Files/SPRINT_2_STORAGE_INVENTORY.md). Decided: **Azure SQL** (not Cosmos) for shared records; Functions is the **SQL/data layer only** (vendor API calls + scheduled jobs stay client-side); Autotask personal operator creds stay in keytar. Mike's provisioning prerequisites are in [`SPRINT_2_PHASE_0_SETUP.md`](!Idea%20MD%20Files/SPRINT_2_PHASE_0_SETUP.md).

| Current (prototype) | Replace with |
|---|---|
| Local JSON settings files + `margin_*` keytar | **Azure App Configuration** — push config to all users instantly |
| SharePoint-JSON bridges + local run history | **Azure SQL** — queryable, reportable, concurrency-safe |
| No monitoring | **Application Insights** — native to Azure tenant |
| No notifications | **Azure Function + SQL** — admin posts, all users see it (Sprint 3) |

Specific items:
- [x] **Azure Key Vault** *(complete)* — All shared API credentials in Key Vault: Pax8, Autotask (shared read-only), CIPP, Datto RMM, Meraki. HR Portal content also persisted via KV. Central, audited, revocable.

- [x] **Phase 0 — Azure provisioning** *(complete)* — SQL DB, Functions app (Entra token validation), App Configuration store. See `SPRINT_2_PHASE_0_SETUP.md`.
- [x] **Phase 0b — Functions scaffold + health check** *(complete, v3.0.0)* — token validation, managed-identity SQL pool, `hubApi.js` client.
- [x] **Phase 1 — Azure App Configuration (read path)** *(complete, v3.0.0)* — 8 settings sources re-pointed with App Config → stale local → hardcoded default fallback. **Read-only for now** — Settings "Save" buttons for fully-migrated tools correctly show a "centrally managed" error instead of a silent no-op.
- [x] **Phase 2 — Audit backbone** *(complete + verified, v3.0.0)* — `activity_log` + `installs` tables; Meraki audit, User Audit log, Blackpoint push-log, and Invoice push-log all write through the central audit stream (Invoice Processor's Push History was per-machine before — now shared). Install/version heartbeat on launch. Verified live in SQL.
- [x] **Phase 3 — User Audit history → SQL** *(complete + verified, v3.0.0)* — `ua_runs`/`ua_writebacks`/`ua_sends`/`ua_reviews` tables replace the single `hub-user-audit-history.json` SharePoint blob. No fallback — SQL is the only source of truth; a Hub API failure surfaces a clear "Failed to load" error instead of showing stale/incomplete history. Existing history backfilled.
- [x] **Phase 4 — Per-tool snapshots & run caches → SQL** *(complete + verified, v3.0.0)* — Project Analysis runs (now unbounded, was capped at 20/machine), Project Time Summary daily worked-hours snapshots, Kaseya invoice-month snapshots, Blackpoint agent-count snapshots — all four now shared across the team instead of per-machine. Same no-fallback principle as Phase 3: reads degrade to empty/null on a Hub API outage rather than a second stale copy; writes that matter (saving a Project Analysis run) surface a clear error in the UI if they fail.
- [x] **Phase 5 — Central directory → SQL** *(complete + verified, v3.0.0)* — `hub-company-mappings.json` → `companies`/`company_platforms`/`service_mappings`. The flagship migration (7 consumer files) shipped with **zero changes to any of those 7 files** — `hubDirectory.js`'s `loadHubDirectory()`/`saveHubDirectory()` keep their exact original shape/contract; the Functions layer reassembles/accepts the same JSON shape underneath. `company_platforms` stores each platform entry as JSON (not fixed columns) since shape varies by platform (Pax8 has `id`/`source`, others don't) — `name` is extracted only for lookups. Company Directory's badge corrected to "Azure SQL ✓". Backfilled: 146 companies, 391 platform mappings, 16 service mappings (7 legacy no-name/no-atId entries correctly skipped). Also fixed two pre-existing dead tabs (MSC Clients, CIPP Tenants) found while verifying — never-defined render functions, unrelated to the migration itself.
- [x] **App Config write path** *(complete)* — `PUT /config/{key}` (`{value}` to set, `{delete:true}` to clear back to the stale-local/hardcoded default), gated by `hub.admin` checked server-side against the `roles` claim on the caller's validated token (same app registration used for both interactive sign-in and this API's `access_as_user` scope, so no extra Graph/SharePoint round trip needed). Every change writes an `activity_log` row (`tool: 'config'`, old→new in `detailJson`). Wired up all 7 Settings Save buttons that were showing the "centrally managed, contact Mike" rejection (Profitability, Renewals, Kaseya cost-split, Prompt Templates, Margin Analyzer, Meraki expiration threshold, User Audit settings — two more than the original spec's "5" since Meraki/User Audit were migrated to App Config later in the sprint). Required a one-time Portal grant (Function App's managed identity → App Configuration Data Owner) — done.
- [x] **Application Insights** *(complete)* — Function App was already linked to an Application Insights resource (`anchorhub-api`); no code changes needed. Confirmed live via Live Metrics: request rate, dependency calls, and traces (e.g. `Functions.directory`, `Functions.installsHeartbeat`) all flowing.
- [x] **Cleanup** *(complete, v3.0.0)* — Removed the dead `pax8_client_id`/`pax8_client_secret` keytar reads from `creds-check` (real client uses Key Vault; nothing in the UI ever wrote these keys). Turned up an actual live bug while checking usage: the credentials-warning banner (`checkCredsWarning()`) hid itself only when `pax8 && autotask` were both true — since `pax8` was always `false` (Key Vault-backed, not per-user keytar), this banner has likely been showing a false "API credentials not configured" warning to **every** user regardless of real configuration. Now checks `autotask` only, the one credential that's genuinely per-user. The remaining `loadHubFile`/`saveHubFile` call sites (Meraki/User Audit settings fallback tier, User Audit history's one-time backfill, the confirmed-stays-as-cache paths) turned out to all be intentional, not leftovers — no further retirement needed there.
- [x] **Regression pass fixes** *(complete, pending SQL migration)* — An 8-angle diff review of the whole branch turned up one real correctness bug plus several cleanup items, all fixed:
  - **Blackpoint same-day rerun bug** — `bp_snapshots`' `(snapshotDate, tenantId)` primary key meant re-running the endpoint-usage scan twice in one day silently overwrote that day's row before a delta could be computed against it. Replaced with an append-only `bp_snapshot_runs` table (one row per tenant per run, sharing a `capturedAt` timestamp) — GET now finds the true previous run via `MAX(capturedAt)`, matching the original "diff against whatever ran last" behavior regardless of same-day reruns. **Requires running `functions/sql/phase6-fixes.sql` in the Portal Query Editor** (creates the new table, migrates existing history, drops the old one) before redeploying.
  - **`projectAnalysisRuns` POST** now `MERGE`s instead of a plain `INSERT` — a retried/duplicate save no longer throws a `UNIQUE` constraint violation on `runId`.
  - **`syncDirectory` now deletes companies removed from the payload** (not just those flagged `excluded`) — the old SharePoint save was a full-file overwrite, which implicitly dropped removed entries; the SQL upsert-only sync left them behind forever. Platform rows are deleted first (FK dependency).
  - **`pts_snapshots` now trims to 90 days** on every write — the old per-machine JSON file had no unbounded-growth risk; SQL does.
  - **Shared `withAuth()` wrapper** (`functions/src/lib/auth.js`) and **`unwrapSqlError()` helper** (`functions/src/lib/db.js`) replace the `requireCaller` try/catch and mssql transaction-error-unmasking boilerplate that had been copy-pasted across all 12 Functions route files.
  - **`ptsSnapshots`/`blackpointSnapshot` writes** now go through a staging-temp-table + set-based `MERGE`/`INSERT` (the pattern `directory.js`'s `syncDirectory` already proved out) instead of one round trip per project/tenant.
  - Removed the dead `fs` import from `main/ipc/blackpoint.js` (local snapshot file/SharePoint push-log bridge were retired in favor of the Hub API earlier in this sprint).
  - Deliberately **not** touched: `blackpoint.js`/`kaseyaProcessor.js`/`projectAnalysis.js`/`projectTimeSummary.js` still catch-and-return-empty on a Hub API read failure rather than throwing like `userAudit.js` does — flagged as an inconsistency with the "SQL only, no fallback" principle, but changing it touches renderer error-handling across 4 tools with no way to test live tonight, so left alone pending a follow-up pass.

---

### Sprint 2.5 — Microsoft Graph: Email & Teams

These build directly on the SSO foundation already in place. No Azure backend required — just additional Graph scopes.

- [x] **Direct email sending (`Mail.Send` scope)** *(complete)* — Graph `sendMail` active. Email sends directly from the signed-in user's Exchange mailbox. Enables Newsletter Builder and User Audit Report email delivery without Outlook.

- [ ] **Teams channel posting (`ChannelMessage.Send` scope)** — Post messages into any Teams channel the signed-in user has access to. Use cases: tool completion summaries, count mismatch alerts, admin announcements.

- [ ] **Teams incoming webhooks (no scope — just a URL)** — Simpler one-way option for automated alerts. Configure a webhook URL in Teams, paste it into Hub settings.

---

### Sprint 3 — Home Screen & Notifications

- [ ] **Home screen card revamp** — Two card types: report cards (run schedule + status badge) and management/utility cards (contextual status, e.g. "X drift issues").
- [ ] **Tool run schedule & status badges** — Configurable run frequency per report card. Badge turns green/yellow/red based on last-run recency. Last-run timestamp stored in Azure DB.
- [ ] **Notifications center** — Bell icon with badge count. Pulls overdue tool alerts, admin announcements, and new release notes.
- [ ] **In-app idea submission** — Button that submits to the Azure backend → posts directly to a Teams channel via Graph.

---

### Sprint 4 — Access Control & Polish

- [x] **Role-based tool access & permissions architecture** *(complete — migrated off the original Azure-portal-managed 4-tier system to the 9-role Hub Role Matrix)*
  Sidebar visibility, in-tool tab/feature gating, and per-user exceptions are all driven by two SharePoint lists on the Intranet site: **Hub Role Matrix** (tool key × role → allowed) and **Hub User Overrides** (individual grants regardless of role), read live via `main/shared/roleMatrix.js`. The 9 roles are `hub.admin`/`hub.manager`/`hub.delivery`/`hub.tam`/`hub.strategic`/`hub.projects`/`hub.finance`/`hub.sales`/`hub.wsd` — only `hub.admin` carried its name over unchanged from the old 4-tier system (`hub.admin`/`hub.it`/`hub.standard`/`hub.readonly`, fully retired).

- [x] **Job-title-based hub role assignment** *(complete)*
  Roles assign automatically from Entra job title at sign-in, via the **Hub Title Roles** SharePoint list — seeded by `scripts/setup-role-matrix.js` (`TITLE_ROLES`) and edited directly in SharePoint since, so treat the list itself as current, not the script. Individual exceptions handled by additive override roles or a Hub User Overrides row.

  | Job Title | Hub Role |
  |---|---|
  | Managing Director, Director of Service Delivery, Director of Strategic Services, Director of Client Success | `hub.admin` |
  | Manager of Service Delivery, Manager of Professional Services | `hub.manager` |
  | Technical Account Manager | `hub.tam` |
  | Account Manager, Client Experience Manager, Technology Strategist, Talent Acquisition and Learning Manager | `hub.strategic` |
  | Technical Project Engineer, Project Engineer | `hub.projects` |
  | Service Desk Engineer/Team Lead/Technical Coordinator, Co-Managed Technical Lead, Centralized Technical Services Engineer, Cybersecurity Administrator, Office Administrator | `hub.delivery` |
  | Accountant, Accounting Assistant | `hub.finance` |
  | Business Development, Business Development Manager | `hub.sales` |
  | Workstation Deployment Specialist (+ title variants) | `hub.wsd` |

- [x] **Role × Tool access matrix** *(live — managed directly in the "Hub Role Matrix" SharePoint list, deliberately not mirrored here)*
  A stale markdown copy of the old 4-tier version of this table (last synced who-knows-when) was the direct cause of the v3.6.1 Duo Management lockout bug, so this roadmap no longer tries to keep a parallel copy current. To check or change what a role can access: the SharePoint list is authoritative; `scripts/setup-role-matrix.js`'s `TOOL_MATRIX` is only the one-time seed (13 tools, run once — it does not update existing list rows, so tools added straight in SharePoint since won't appear there); `main/shared/roleMatrix.js` is the runtime read path.

  Two related gotchas surfaced by the v3.6.1 fix, worth checking on any future role-gated tool:
  - **Three separate gates, not one:** the sidebar's master tool-key toggle, a Hub Role Matrix row for the tool (no row = invisible to anyone with an assigned role — Hub User Overrides is the one-off exception), and the tool's *own* internal render-function check can all drift independently.
  - **Known still-open instance of the third gate:** `renderMerakiAdmin`, `mkRenderAudit`, `mkOrgDetailHtml`, and its org-row renderer in `app.js` still gate Meraki Admin Management's admin-tier controls (Exclude, Add All Missing, etc.) with `['hub.admin','hub.it']` — `hub.it` no longer exists, so only `hub.admin`/`_currentUser.isAdmin` can ever match. Unlike Duo Management this doesn't hard-lock the page, it just silently hides those buttons for anyone else. Flagged to Mike (2026-07-29), not yet fixed — needs a decision on which of the 9 roles should get that tier before applying the same fix pattern.

- [ ] **Audit trail** — Log every significant action to Azure DB. (Depends on Sprint 2 DB)
- [ ] **Central API key revocation** — Admin UI to revoke or rotate any API key from one place. (Depends on Key Vault, which is complete — this is the management UI layer)

---

## 🔁 Ongoing / Continuous Improvement

- **Hub Home (daily start page)** — Live and actively improving. Quick-launch tiles, greeting bar, calendar, AT tickets, and growing widget set. Future additions: Values & Norms widget (see Next Up), tool run status cards (Sprint 3), and richer admin-managed content once the Azure backend lands.

---

## 💡 Ideas & Backlog

Drop ideas here. Nothing too small or too big.

- [ ] **Project Analysis — Historical comparison baseline**
  Once enough projects have been run through the analyzer, surface ANS-wide averages ("similar projects averaged X hours for this phase / category") alongside current project numbers. Gives the on-screen and exported report actual context instead of raw hours in isolation. Requires persisting run history to Azure DB (Sprint 2).

- [ ] New employee onboarding checklist tool
- [ ] In-app bug / feedback reporter

- [ ] **New Client Onboarding Wizard**
  A guided, multi-step form that walks an employee through provisioning a brand-new client across Autotask end-to-end — duplicate check, Company record, T&M and Managed Services contracts, primary contact, and optional Discovery Call ticket. One-click "Open in Autotask →" deep links for each created record.

- [ ] **Device Coverage Tool**
  Cross-platform coverage matrix: for every managed device, what's installed and what's missing (Datto RMM, Duo, EDR, CyberQP, Splashtop, etc.). Per-client view and fleet sweep mode. One-click "Push Quick Job" for gaps fixable via Datto RMM.

- [ ] **Duo Billing Tool**
  Read-only pull of Duo sub-account edition/telephony data mapped to Pax8 or billing records for monthly reconciliation. Requires `duo-accounts-ikey/skey` (Accounts API read) but zero write permissions.

- [ ] **SharePoint Contract Lookup**
  Given a client name, search their SharePoint folder for a contract document, pull key fields (term dates, rates, services), and surface them inside Anchor Hub. Requires `Sites.Read.All` or `Files.Read.All` Graph scope.

---

## 🤝 Contributors

| Person | Area |
|--------|------|
| Mike | Core platform, Autotask/Pax8 tools |
| Brian | Flask tool ports (Project Analysis, User Audit Report) |

**Branch rules:** All changes via PR into `main`. One approval required. Never push directly to main. Never bump `package.json` version without coordinating a release.
