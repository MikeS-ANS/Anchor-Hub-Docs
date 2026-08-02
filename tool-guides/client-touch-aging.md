# Client Touch Aging

> **Beta.** Marked Beta in the sidebar and on its own page since v3.6.0 while a couple of open questions get confirmed with the requester — see [Known open items](#known-open-items) below.

Client Touch Aging flags MSC (Managed Service Contract) MRR+ORR clients whose Autotask touch history has gone stale — no logged **Note** or **Quick Note** in the last 3 months, or no **Client Meeting** in the last 12 months. Both thresholds are admin-editable. It also surfaces a **due-soon** tier for clients approaching (not yet past) either threshold, and a computed **Priority** score to help you triage the flagged list. It was originally spec'd as a pure reporting tool with no write-back of any kind — a single, deliberate, explicitly-confirmed exception now exists (**Log a touch**, below); everything else remains read-only against Autotask.

Sidebar tool key: `client-touch-aging`.

---

## How it stays fast: never a live Autotask call

Unlike a tool that queries Autotask at the moment you open it, Client Touch Aging always reads from an Azure SQL cache (`company_touches`, `touch_sync_state`). That cache is kept current by:

- A **daily incremental sync**, run by the same Electron client-side scheduler pattern as Meraki License & EoL Expiration and User Audit Report — meaning it only actually fires while someone has Anchor Hub open. The scheduled time is admin-configurable (Settings → Daily Sync).
- A manual **"Check for new data"** button on the Report tab, for anyone who doesn't want to wait for the schedule.

Both paths run the identical sync logic, so there's exactly one code path to trust. The Report tab always shows a **"Data as of [timestamp]"** stamp next to the sync button so it's obvious this is a snapshot, not a live view — with idle / running / failed states, and a **Retry** option if the last sync attempt failed (the previous successful timestamp is preserved until a sync actually succeeds again).

---

## Report tab

- **Layouts.** A segmented switcher toggles between **Table**, **Cards**, and **By Account Manager** — whichever you pick is remembered per-user (stored in your browser's local storage), so it stays your default next time.
- **Scope.** **My clients / All clients**, defaulting to **My clients**. "Mine" resolves your signed-in identity to an Autotask resource and shows only clients whose Account Manager is you. If Anchor Hub can't resolve an Autotask resource for your email, you'll see an explicit warning rather than a silently empty list — switch to "All clients" or ask an admin.
- **Search & filter.** Filter by client name, or by Account Manager via a dropdown populated from whoever actually owns a currently-flagged client.
- **Metric strip.** Now five tiles: Flagged / Never touched / Note overdue / Meeting overdue / **Due soon** — counts reflect whatever the current scope + search + filter combination is showing. See [Due-soon early-warning tier](#due-soon-early-warning-tier) below.
- **Priority column (Table view).** Every row shows a computed **Priority** score and the column is sortable. See [Priority score](#priority-score) below.
- **Client names link out** directly to that client's Autotask company record.
- **Log a touch.** Any flagged row has a **"+ Log touch"** button. See [Log a touch (write-back)](#log-a-touch-write-back) below.
- **Breaching and due-soon clients are listed; current clients are not.** A client that's current on both a Note/Quick Note and a Client Meeting, and not within the due-soon window on either, never appears in the report at all. If everyone in the current scope is current, you get a quiet green **"Every client is current"** message instead of an empty table.

---

## Due-soon early-warning tier

Previously, the report only ever showed clients who had *already* breached a Note/Quick Note or Client Meeting threshold. A new admin-editable setting, **`dueSoonDays`** (default **14**), now flags clients *approaching* — not yet past — either threshold, so Account Managers can get ahead of a stale touch instead of only finding out after they've already missed it.

This is a real behavior change worth calling out clearly: **the report's client list is measurably broader than before** — it now includes both breached and due-soon clients, not breached-only.

Due-soon clients get a distinct, muted treatment so they're never confused with an actual breach:

- **Cards view** — a 5th lane alongside the existing Never Touched / Note Overdue / Meeting Overdue lanes.
- **Table view and By Account Manager view** — a muted "due soon" badge on the row.
- **Metric strip** — a 5th tile counting due-soon clients in the current scope/filter.

`dueSoonDays` is edited from **Settings → Thresholds** (`hub.admin` only), alongside the existing Note/Quick Note and Client Meeting threshold controls.

---

## Priority score

Every flagged or due-soon client now gets a computed **Priority** score, shown in all three report views and sortable as a **Priority** column in Table view. It weights:

- **Contract renewal proximity** — how soon the client's active contract is up for renewal.
- **Active contract value** — the size of the client's current contract(s).
- **Recent Autotask ticket volume** — how many tickets the client has opened recently.

...on top of the existing severity tier (never-touched / both-overdue / meeting-overdue / note-overdue / due-soon). **The report's default sort changed from "longest since any touch" to "highest priority first"** — another explicit, intentional behavior change worth knowing about if you're used to the old ordering.

This required a new Azure SQL table, `company_priority_signals`, and two new daily-sync sub-jobs — batched Autotask Contracts and Tickets queries, following the same no-fan-out pattern as the existing CompanyNotes sync (one batched query per sync run, not one call per client). Schema migration: `functions/sql/phase12-client-touch-aging-priority.sql`.

> **Known trade-off, flagged not fixed:** the ticket-volume weighting direction — treating *more* recent tickets as *higher* relationship risk, and therefore higher priority — is a judgment call that hasn't been validated against real usage yet. Tunable/revisit-later, same treatment as the two [known open items](#known-open-items) already carried over from the original spec.

---

## Log a touch (write-back)

Client Touch Aging was originally spec'd as read-only by explicit design. **This is a deliberate, explicitly-confirmed exception to that design**, not a scope-creep add.

A **"+ Log touch"** button appears on any flagged row. Clicking it opens a modal with:

- A **Note vs. Client Meeting** toggle (which touch type you're logging).
- A free-text note field.

Submitting posts a real Autotask **CompanyNotes** entry directly from the Hub — the client's row in the report updates immediately via an **optimistic cache update**, rather than waiting for the next daily (or manual) sync to pick it up.

**Requires your personal Autotask API key** in Settings → Autotask — the same gate as User Audit Report's existing write-back feature. If your key isn't configured, the button shows as **visibly disabled with an explanation**, rather than silently failing when clicked.

> **Known trade-off, flagged not fixed:** "Log a touch" — and the priority-signals sync alongside it — shares the same underlying trust model as this app's other cache-only sync endpoints: any signed-in Hub user's valid token can technically call the sync-persistence endpoint directly and mark a client as "touched" without the corresponding Autotask write having actually happened. (The Electron UI itself always does this correctly and safely — this is about someone bypassing the UI entirely, not a flaw in the UI flow.) This is a pre-existing architecture trade-off across this whole class of Sprint-2 endpoint, not unique to this feature — but it's worth flagging here specifically, since "Log a touch" is the first feature in this family where a single **fabricated** record (not just a mirror of real Autotask data) could be pushed through it.

---

## Settings tab

Settings mixes admin-only and open sections. Admin sections carry an **Admin** pill and are enforced read-only server-side for non-admins — not just hidden in the UI, so there's no client-side trick to bypass it.

| Section | Who can edit | What it controls |
|---|---|---|
| **Thresholds** | `hub.admin` | Months before a Note/Quick Note counts as overdue (default 3) and before a Client Meeting counts as overdue (default 12), plus how many days out the [due-soon early-warning tier](#due-soon-early-warning-tier) starts flagging an approaching threshold (`dueSoonDays`, default 14). |
| **Touch Types** | `hub.admin` | Which Autotask CompanyNotes action types count as a "Note/Quick Note" vs. a "Client Meeting" — picked from a live picklist of actual Autotask action types (via `autotask_get_field_info`), not raw ID entry. |
| **Excluded Clients** | `hub.admin` | Search-and-exclude list of MSC clients. An excluded client is fully hidden from the report — not just deprioritized. |
| **Daily Sync** | `hub.admin` | What time (24-hour, local) the daily sync job runs. |
| **Scheduled Digest** | **Any signed-in user** | Recipients and cadence (Off / Weekly / Monthly) for an emailed summary of every currently-flagged client. This is the one deliberate exception to Anchor Hub's usual "every Settings section is `hub.admin`-only" pattern — digest recipients/cadence needed to stay editable by anyone, since it's personal notification config, not shared business logic. |

**Preview / Send now.** The Scheduled Digest section has its own **Preview / Send now** button that opens a modal showing exactly what the next digest email will look like (recipients + rendered HTML table), with a **Send now** button to fire it immediately rather than waiting for the schedule. This mirrors User Audit Report's own billing-digest preview modal, though it's a separate, un-shared implementation.

---

## Default Touch Types

Confirmed live against this Autotask instance's actual `CompanyNotes.actionType` picklist. Only these are on by default — Settings can add or remove any action type from either bucket.

| Bucket | Default action types |
|---|---|
| Note / Quick Note | Note, Quick Note, **and** a plural "Notes" action type — included defensively in case staff have been logging under it (see [Known open items](#known-open-items) below). |
| Client Meeting | Client Meeting |

---

## AI Prompt

Not applicable — Client Touch Aging has no AI/LLM component of any kind. Every part of the report (thresholds, breach/due-soon detection, the Priority score, sorting) is deterministic math against the cached sync data; nothing here calls Hatz.ai or any other model.

---

## Known open items

Flagged here deliberately rather than left silent — open questions/trade-offs to revisit after real usage, not blocking issues:

- **The defensive "Notes" (plural) action type.** It's unconfirmed whether staff have actually been logging touches under this action type in practice, or whether it's just precautionary inclusion. Worth checking real usage once live data is flowing. *(Open question for Corey, the tool's requester.)*
- **"Data as of yesterday" framing.** Because the report is always served from the daily-synced cache rather than a live pull, there's an open question of whether that lag fits how TSMs and Account Managers actually want to use this day to day, or whether it needs a tighter refresh cadence once real usage patterns show up. *(Open question for Corey.)*
- **Priority score's ticket-volume weighting direction** *(added in the v2 pass)*. Treating more recent Autotask tickets as a *positive* priority signal (more tickets = more relationship risk) is a judgment call that hasn't been validated against real usage yet — tunable/revisit-later.
- **Shared cache-endpoint trust model** *(added in the v2 pass)*. "Log a touch" and the priority-signals sync share the same pre-existing trust model as this app's other Sprint-2 cache-only endpoints: any signed-in Hub user's valid token can technically call the sync-persistence endpoint directly and mark a client "touched" without the real Autotask write having happened (the Electron UI itself always does this correctly — this is about bypassing the UI entirely). Not a new gap, but "Log a touch" is the first feature in this family where a single fabricated record, not just a mirror of real Autotask data, could be pushed through it.

---

## Data & architecture notes (for future maintainers)

- Backend: `main/ipc/clientTouchAging.js` (client-side sync + IPC handlers) and `functions/src/functions/touchAging.js` (Azure Functions routes: `GET /touch-aging/report`, `PUT /touch-aging/sync`, `GET /touch-aging/settings`, `PUT /touch-aging/settings/admin` — `hub.admin`-gated, `PUT /touch-aging/settings/digest` — open to any signed-in user, plus the v2 write-back route backing "Log a touch").
- SQL schema migrations: `functions/sql/phase10-client-touch-aging-schema.sql` (base tables — `company_touches`, `touch_sync_state`), `functions/sql/phase11-client-touch-aging-snippet.sql`, and `functions/sql/phase12-client-touch-aging-priority.sql` (v2 — adds `company_priority_signals`, backing the Priority score's two new daily-sync sub-jobs: batched Autotask Contracts and Tickets queries, same no-fan-out pattern as the existing CompanyNotes sync).
- Two shared helpers were extracted during this build so User Audit Report and Client Touch Aging share code instead of duplicating it: `main/shared/accountManager.js` (Account Manager resolution) and generalized Autotask deep-link helpers in `app.js` (`atAccountUrl` / `atClientLink` / `wireAtLinks`, with the old `ua*`-prefixed names kept as thin wrappers for backward compatibility).
- v2's "Log a touch" write-back gate follows the same personal-Autotask-key pattern as User Audit Report's write-back (`main/ipc/userAudit.js`) rather than introducing a new credential-check pattern.
