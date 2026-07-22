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

---

## 🔄 Next Up

These tools have detailed specs written and are ready to build. See the `Idea MD Files/` folder for the full spec on each.

- [ ] **Newsletter Builder** *(spec: `NEWSLETTER_BUILDER_SPEC.md`)*
  Compose and send formatted internal company newsletters from within the Hub. Section-based form (Announcements, Team Highlights, Project Wins, Tech Tips, etc.), live HTML preview, and delivery via Microsoft Graph `Mail.Send` to a distribution group. Stores sent history with duplicate-and-edit support.
  *`Mail.Send` Graph scope is already active ✅*

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

- [x] **Role-based tool access & permissions architecture** *(complete)*
  Tool visibility, write operations, and tab-level access all gated by Entra ID role. Managed from the Azure portal. Per-user overrides via Hub User Overrides SharePoint list.

- [x] **Job-title-based hub role assignment** *(complete)*
  Roles assigned automatically from Entra job title at sign-in. Individual exceptions handled by additive override roles.

  | Job Title | Hub Role |
  |---|---|
  | Technical Account Manager | `hub.it` |
  | Account Manager | `hub.standard` |
  | Client Experience Manager | `hub.standard` |
  | Service Desk Engineer | `hub.standard` |
  | Workstation Deployment Engineer | `hub.standard` |

- [x] **Role × Tool access matrix** *(live — update as new tools are built)*

  | Tool | hub.admin | hub.it | hub.standard | hub.readonly |
  |---|---|---|---|---|
  | Subscription Audit | Full | Full | Read Only | Read Only |
  | Invoice Monitor | Full | Full | Read Only | Read Only |
  | Duo Management (admin tabs) | Full | Full | No Access | No Access |
  | Duo Management (client tabs) | Full | Full | Full | No Access |
  | Datto RMM Quick Jobs | Full | Full | Full | No Access |
  | Timesheet Review | Full | Full | No Access | No Access |
  | *(expand as tools are built)* | | | | |

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

- [ ] **Tool Inventory & License Count Tracker**
  Replace the manual quarterly tool inventory spreadsheet with an automated pull from every connected platform (Kaseya RMM, Pax8, BitDefender, CyberQP, Splashtop, etc.). Ad-hoc client lookup and full quarterly snapshot modes. Writes a dated tab into the shared SharePoint Excel file.

---

## 🤝 Contributors

| Person | Area |
|--------|------|
| Mike | Core platform, Autotask/Pax8 tools |
| Brian | Flask tool ports (Project Analysis, User Audit Report) |

**Branch rules:** All changes via PR into `main`. One approval required. Never push directly to main. Never bump `package.json` version without coordinating a release.
