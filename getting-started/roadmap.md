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

---

## 🔄 Next Up

These tools have detailed specs written and are ready to build. See the `Idea MD Files/` folder for the full spec on each.

- [ ] **Timesheet Review Tool** *(spec: `TIMESHEET_REVIEW_TOOL_SPEC.md`)*
  Manager-facing tool that pulls Autotask time entries for a resource or team and computes a client-facing utilization % (target: 70%). Runs deterministic data-quality checks (under-8h days, gaps, overlaps, late entries, thin notes) and an opt-in AI note-quality pass via Hatz.ai. Scoped by Graph direct reports. Links to Autotask for actual approval — no in-Hub approval (Autotask API limitation).

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
  Package as Win32 app and push via Intune. Solves Windows SmartScreen for all managed employee machines immediately — no cert required for internal distribution.

- [ ] **Datto RMM Quick Job — Install / Update Anchor Hub**
  PowerShell script that downloads the latest installer from the GitHub release and runs it silently. Can be targeted to specific sites or devices from the RMM console for on-demand installs without having to walk someone through it manually.
  - Download URL pulled from `latest.yml` on the GitHub release so it always gets the current version
  - Silent install: `Anchor-Hub-Setup-x.x.x.exe /S`
  - Works alongside auto-update — use this for first installs or forced re-installs; the app handles its own updates from there
  - Once repo is private, script will need a PAT with `Contents: read` to download the asset

- [ ] **Make GitHub repo private + extract sensitive config**
  The public repo currently contains ANS's Azure AD Tenant ID, Client ID, SharePoint hostname, and internal email addresses in plain text. Plan:
  1. Extract `MSAL_CLIENT_ID`, `MSAL_TENANT_ID`, `SP_HOST`, and `SUPPORT_TO` into a `config.js` file
  2. Add `config.js` to `.gitignore`
  3. Flip repo to private in GitHub settings
  4. Configure electron-updater with a read-only fine-grained PAT for update checks

---

### Sprint 2 — Azure Backend

Replace remaining local-only storage with a proper centralized backend.

| Current (prototype) | Replace with |
|---|---|
| Per-machine keytar for write keys | **Azure SQL** (encrypted per Entra identity) — Sprint 2 |
| Local JSON settings files | **Azure App Configuration** — push config to all users instantly |
| Local run history | **Azure SQL or Cosmos DB** — queryable, reportable |
| No monitoring | **Application Insights** — native to Azure tenant |
| No notifications | **Azure Function + DB** — admin posts, all users see it |
| Email idea submissions | **Microsoft Graph → Teams channel or Planner** |

Specific items:
- [x] **Azure Key Vault** *(complete)* — All shared API credentials in Key Vault: Pax8, Autotask (shared read-only), CIPP, Datto RMM, Meraki. HR Portal content also persisted via KV. Central, audited, revocable.

- [ ] **Azure App Configuration** — centralize tool settings. Admin changes a value, all machines pick it up.
- [ ] **Azure SQL / Cosmos DB** — run history, notifications, announcements, idea submissions, audit trail.
- [ ] **Application Insights** — replaces any current logging. Already in the Azure ecosystem.
- [ ] **Azure Functions** — serverless API layer between the Electron app and the Azure data services.

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
