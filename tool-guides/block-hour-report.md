# Block Hour Report

> **Beta.** Shipped in v3.7.0. Marked Beta in the sidebar and on its own page while the team gathers feedback.

Block Hour Report is an Account Manager-facing dashboard and AI-narrative review tool for **Block Hour clients** — Autotask contracts (`contractType=4`, "Block Hours Support Agreement", `status=1` In Effect) where a client prepays for a fixed block of support hours instead of a flat monthly retainer. It shows hours purchased/used/remaining across every such client, flags clients whose coverage has actually lapsed, and drafts a monthly value-delivered summary email — via Hatz.ai — that an AM reviews and approves before it ever reaches a client.

Sidebar tool key: `block-hour-report`.

---

## How this differs from Client Touch Aging: live, not cached

Client Touch Aging reads from a daily Azure SQL sync because fanning `CompanyNotes` queries out across ~74+ companies tripped Autotask's rate limits. Block Hour Report does the opposite on purpose: it queries Autotask **live, on every dashboard load**. The Block Hour population is small (~20 contracts as of this build) — a `Contracts` query plus one `ContractBlocks` query per contract stays well within Autotask's limits at that size, confirmed live during the build.

The only thing that actually persists in Azure SQL is the tool's own generated content — the AI narratives and their review/approval state (`bh_reports` table). Everything about hours, dates, and coverage is recomputed fresh from Autotask every time you open the Dashboard tab.

---

## Dashboard tab

Every current Block Hour client (contract `In Effect`), one row each:

- **Client** — links out to the Autotask company record.
- **AM** — the client's Account Manager (same resolution helper User Audit Report and Client Touch Aging use).
- **Block usage** — a **segmented usage bar**: one segment per currently-Active `ContractBlocks` row, sized proportionally to that block's share of the client's total purchased hours. This matters because a contract can have more than one block Active at once (confirmed on a real client with two overlapping blocks in progress) — the segmented bar reads as one continuous picture instead of needing a second bar per extra block.
- **Purchased / Used / Left** — summed across all Active blocks, trusting Autotask's own `hoursApproved` per block as ground truth rather than reconstructing usage from tickets (block date ranges can overlap, so attributing a ticket's hours to one specific block is genuinely ambiguous — Autotask has already resolved it internally).
- **Spend (12mo)** — the dollar total of blocks purchased in the trailing 12 months, by purchase date. This is independent of current Active-block status — a block that's since expired still counts toward what the client actually paid in that window.
- **Expires** — the latest block's end date, read directly from Autotask, never computed. (Autotask's "purchasing a block pushes the end date out 12 months" description is a rough guideline, not a formula — real end dates vary purchase to purchase.)
- **Last sent** — most recent report send date for that client.

A **metric strip** across the top shows: total Block Hour clients, clients with no current coverage, clients running low, clients awaiting review, and total hours remaining across the whole book. **Search** (by client name) and an **Account Manager filter** narrow the table the same way User Audit Report and Client Touch Aging do.

### Expanding a client row: block breakdown, 12-month trend, and ad hoc reports

Clicking the ▸ arrow on any row expands it into a detail panel:

- **Block breakdown** — every block for that client, active or expired, each with its own purchase/expiry dates, hours used/purchased, **hourly rate**, and status.
- **12-month average + estimated runway + unposted time** — loaded lazily on first expand (a Load button triggers it) rather than eagerly for every client on every dashboard load, to avoid reintroducing the same concurrent Autotask fan-out that once tripped a rate limit elsewhere in this feature. Once loaded, it shows:
  - **Avg. billable hrs/mo** over the trailing 12 months.
  - **Estimated runway at current pace** — hours remaining ÷ that monthly average, shown in weeks or months. A rough estimate, not a guarantee: it reads `—` for dormant clients and "No recent usage" for a client with no billable hours in the last 12 months.
  - **Unposted time pending approval (90d)** — hours entered in the last 90 days but not yet approved/posted (Autotask's `hoursToBill > 0` signal, floored to the last 90 days so it reflects a live early-warning signal rather than an accumulating historical artifact of stale unapproved entries). This is internal-only context — it is never part of the official hours-used figure, since only posted time counts there. When it's non-zero, a callout shows what remaining hours would look like if all of that unposted time posts as-is.
- **On-demand, block-scoped ("ad hoc") report** — check one or more blocks in the breakdown above (any status — a historical, already-expired block is a valid thing to report on too) and click **✨ Generate block report**. This produces a report scoped to just the selected blocks' own purchase-to-expiry date window instead of a fixed calendar month, through the exact same Hatz.ai narrative generation, guardrails, and Review queue pipeline as the regular monthly report. Useful when a client asks "what did we get for that last block we bought?" If the selected blocks' date window overlaps another block's own coverage, the internal-only narrative gets an automatic disclaimer noting that Autotask has no native ticket-to-block attribution, so billed work from the other block may be included too.

### Dormant vs. "running low" — two different states, styled differently on purpose

- **Dormant** (calm sky-blue, dashed border) — the client has **zero current block coverage**: every block they've ever bought has ended and nothing new has replaced it yet. This is a coverage *gap*, not a usage warning, so it's deliberately styled calm rather than alarming (not red) — it's flagged for a manual AM follow-up, not treated as an error state. An admin-editable **grace period** (days, default 0) controls how long after a block lapses before a client is actually marked dormant.
- **Running low** (amber) — the client still has current coverage, but remaining hours have dropped below the admin-editable **low-block threshold** (default 4.0 hours).

These are mutually exclusive and shown with distinct badges so they're never confused with each other. Dormant clients show `—` for usage figures instead of a "0% used" or misleading percentage, since there's no current block to measure against.

---

## Review queue tab

Where an AM reviews an AI-drafted monthly report before it emails the client. Each pending report shows:

- **🔒 From Autotask · not AI** — a visually distinct, locked box: hours used this period, hours remaining, block expiration date, per-block breakdown, and the list of billed work items posted last month. Read-only. These values are written into the client email verbatim — the model never alters them.
- **▲ Client-facing — this is what sends** — a copper-accented, editable textarea containing the AI-drafted client narrative (positive "value delivered" tone, hours and work description only, **never** dollar figures). An AM can edit this text directly before approving.
- **✕ Internal only — never leaves the Hub** — a dashed-border, muted panel with a separate AI-drafted internal narrative. This one *may* include dollar figures (block price, hourly rate) since it's for the AM's own context and is never sent anywhere.

### Guardrails

Two checks must both pass before **Approve & send** is enabled:

1. **No dollar figures detected** in the client-facing narrative.
2. **Hours match Autotask** — any hour figures mentioned in the client narrative's prose must match the real purchased/used/remaining numbers (within a small rounding tolerance). A narrative that doesn't mention hour figures at all passes this check by default — nothing to contradict.

These re-check **live** as the AM edits the textarea, and are re-validated **again server-side** at send time — not just trusted from what the renderer showed a moment ago.

### Approve & send

Clicking **Approve & send** immediately emails the client via Microsoft Graph, from the **signed-in AM's own mailbox** — not a shared/service account — and advances to the next pending report. There is no bulk approve or "send all"; every report goes out one at a time, reviewed individually. If a client has no recipient configured, the button is disabled with an on-screen warning pointing to Settings. If the send itself fails after the narrative edit and approver are recorded, the report is left in an `approved` state (not silently reverted) so it can be retried from the queue without losing the AM's edit.

### Other actions

- **Regenerate** (v3.7.2+) — discards the current draft (including any edits) and re-pulls live hours/tickets from Autotask before writing a fresh pair of narratives, then persists the refreshed numbers onto the same row. Earlier versions only reworded the narrative over whatever numbers the report already had stored, which could go stale if real activity happened after the report was first generated — this is fixed as of v3.7.2.
- **Skip this month** — marks the report skipped; it shows up in Send History but nothing is emailed.
- **Generate now** — a picker lets an AM pull a monthly-shaped report for any eligible client on demand, independent of the fixed monthly schedule. This is a real, permanent feature (e.g. a client just bought a new block, or is being onboarded) — not a testing shortcut. Distinct from the **ad hoc, block-scoped report** available from the Dashboard tab's expanded client row (see above), which scopes to specific blocks' own dates rather than a calendar month. As of v3.7.2, a client whose existing report for the current period is already **sent** or **skipped** reappears in this picker, so an AM can generate a fresh one mid-month or after reconsidering a skip — only a still-open (pending/approved) report blocks a repeat generate.

---

## Send history tab

A log of every report that reached a terminal state — **sent** or **skipped** — showing client, period, date, recipients, and who approved it. There's no separate history table under the hood: a report row's own state machine (`pending → approved/skipped → sent`) doubles as its own history entry.

---

## Settings tab

| Section | Who can edit | What it controls |
|---|---|---|
| **Generation schedule** | `hub.admin` | Day of month + time (America/Denver) for the monthly generation run — one fixed cadence for every client, not per-client purchase anniversaries. |
| **Dormant grace period** | `hub.admin` | Days a coverage gap is allowed before a client is actually flagged dormant (default 0). |
| **Low-block warning threshold** | `hub.admin` | Hours remaining below which a covered client is flagged "running low" (default 4.0) — shown with a live count of how many clients are currently under it. |
| **Hatz.ai model** | `hub.admin` | Which model both narrative prompts are sent to (default `anthropic.claude-haiku-4-5`). |
| **Per-client recipients & pause** | **Any signed-in Hub user** | Who receives a given client's report (semicolon-separated addresses), and a per-client **auto-generate** toggle to pause a specific client's monthly generation without touching the schedule for everyone else. |

The per-client recipients/pause table is deliberately **not** admin-gated — per the original spec, any Hub user should be able to keep a client's recipient list current. It's stored differently from the rest of Settings too: it writes to that company's directory metadata (`bhRecipients` / `bhAutoGenerate`), not the admin-only settings blob, since the generic settings write route is `hub.admin`-gated server-side and this table intentionally isn't. Because this table is open to any Hub user rather than just admins, every recipient or pause change made here is recorded to the shared activity log (who changed it, what changed, when) — the same accountability trail every other Hub write action gets.

A client with no recipient configured is **skipped** by the monthly generation run and **blocked** from manual "Generate now" send, with a clear warning shown directly in the recipients table.

---

## AI Prompt

Both narratives are generated by **two separate calls** to Hatz.ai's OpenAI-style `/chat/completions` endpoint (`https://ai.hatz.ai/v1/chat/completions`, `main/ipc/blockHourReport.js`) — never the same text sent to both places. Model is admin-configurable in Settings (default `anthropic.claude-haiku-4-5`). Each call sends the full instruction block as the **system** message; the **user** turn is just the fixed trigger phrase `"Write it now."`, matching the pattern already used by Timesheet Review, HR & Resources Portal, and Project Analysis.

The hard numbers (hours purchased/used/remaining, block expiration date) are never generated by the model — they're computed straight from Autotask and handed to the prompt as already-computed, read-only facts the model is instructed to restate exactly, not recalculate. Hours purchased/remaining/block expiration come straight from Autotask's `ContractBlocks` data (always current, live). For the monthly report, "hours used this period" (v3.7.2+) is the sum of that specific month's billed work items, not the block's all-time total — for the ad hoc block-scoped report, "hours used" is the block's cumulative total, since that report's whole period is defined as the block's full life through today.

Both prompt builders (`buildInternalPrompt`/`buildClientPrompt` in `main/ipc/blockHourReport.js`) take a `periodDescription` argument that changes how the prompt frames the reporting window — this is what lets the same two prompts serve both the fixed monthly report and the v3.7.0 on-demand, block-scoped ad hoc report without maintaining a second pair of prompts:

| Report type | Internal `periodDescription` | Client-facing `periodDescription` |
|---|---|---|
| Monthly (default) | `last month's` | `last month` |
| Ad hoc (block-scoped) | `the full period of {block label} ({date range})` | `the period of {date range}` |

### Internal narrative (review-queue only — never sent to the client)

**System prompt:**
> You are writing an internal note for the Account Manager of "{client name}", a Block Hour support client at a Denver-area MSP, summarizing {periodDescription} usage of their prepaid support-hour block(s). This is INTERNAL ONLY -- it will never be sent to the client -- so you may include dollar figures (hourly rate, block price) if useful context.
>
> Hard numbers (read-only, already computed -- do not recompute or contradict these):
> `{hoursPurchased, hoursUsed, hoursRemaining, expiryDate, blocks}`
>
> Work items posted in this period (name and hours -- use for color, not as the source of the hours-used total above):
> `[{label, hours}, ...]`
>
> Write a short (3-5 sentence) internal note: how the block is trending (burn rate), whether a renewal/new block purchase looks imminent, and anything unusual in the work performed. Plain language, no preamble, no restating these instructions.

For an ad hoc report, this narrative also gets a fixed, non-AI-generated disclaimer appended afterward (not part of the prompt — always present regardless of what the model wrote) whenever the selected block(s)' date window overlaps another block's own coverage: *"[Note: this covers billed work in the {date range} date window, not per-block ticket attribution -- Autotask has no native link between a ticket and a specific block, so if another block's coverage overlapped this window, its activity may be included here too.]"*

### Client-facing narrative (the actual outbound email text)

**System prompt:**
> You are writing a short, positive summary email to a Block Hour support client ("{client name}") about the value delivered over {periodDescription}, at a Denver-area MSP. This copy goes DIRECTLY to the client.
>
> Hard numbers (read-only, ALREADY COMPUTED -- restate them exactly as given, never round, alter, or recompute them):
> - Hours purchased: {hoursPurchased}
> - Hours used this period: {hoursUsed}
> - Hours remaining: {hoursRemaining}
> - Block expires: {expiryDate}
>
> Work items posted in this period (use these to describe what was accomplished -- do not invent work not listed here):
> `[{label, hours}, ...]`
>
> Rules:
> - Positive, "here's the value we delivered" tone -- a relationship-building touchpoint, not a bare usage report.
> - NEVER mention a dollar amount, price, or rate of any kind -- hours are the only unit of measure in this email.
> - Mention the hours figures above naturally in the prose, using the exact numbers given.
> - Output ONLY the email body paragraph(s) -- 3-5 sentences, no preamble, no restating these instructions, and absolutely no "Subject:" line or any other header/label before the prose.

Both prompts are duplicated (the exact same rules) into the renderer (`app.js`) purely so the guardrail row can re-validate instantly as an AM types in the review-queue textarea, without an IPC round trip per keystroke — the authoritative check that actually gates a send happens server-side in `main/ipc/blockHourReport.js` again before the email goes out.

---

## Access

In addition to the sidebar visibility toggle and a Hub Role Matrix row (both required for anyone to see the tool at all — see [Roles & Permissions](../getting-started/roles-and-permissions.md)), the tool's own render function checks the signed-in user's role directly: `hub.admin`, `hub.manager`, `hub.delivery`, `hub.tam`, and `hub.strategic` all have access; anyone else sees a locked-page message even if the sidebar entry and matrix row are both present.

---

## Known open items

Flagged deliberately rather than left silent — trade-offs and judgment calls to revisit after real usage, not blocking issues:

- **Guardrails are best-effort text checks, not proof.** They catch an AI narrative that clearly leaks a dollar figure or contradicts the given hours in prose — they can't catch every conceivable phrasing. The hard numbers themselves are never AI-generated, which is the real safeguard; the guardrails exist to catch the model drifting in its retelling.
- **Work items are labeled by Autotask's `itemName`, not always a real ticket number.** Confirmed live that `ticketID` is frequently null for Block Hour clients — a lot of their billed work comes from project tasks rather than tickets — so the "Tickets this period" list is really a billed-work-item list, described by name rather than ticket ID.
- **One fixed monthly cadence for every client**, not per-client purchase-anniversary scheduling — a deliberate simplification decided against the alternative up front, not a gap to fill in later.
- **Overage billing is out of scope by design.** ANS doesn't let Block Hour clients run into overage billing in practice — a new block gets purchased before that happens — so there's no "on overage" state to display or model.
- **Dashboard is a live Autotask read on every load**, so at higher future contract volumes this may eventually need the same daily-sync caching pattern Client Touch Aging already uses. Not needed at the current population size (~20 contracts).
- **Unposted-time warning uses a 90-day floor on `dateWorked`, not just `hoursToBill > 0`.** Autotask's own "not yet billed" flag doesn't reliably clear once an entry is actually processed — confirmed live that some entries from 2022-2024 still read `hoursToBill > 0` with no billing-approval timestamp, which would otherwise read as a large stale "unposted" backlog rather than a real, current early-warning signal.
- **Estimated runway is a rough estimate, not a guarantee** — it's a simple hours-remaining ÷ trailing-12-month-average division done client-side, with no seasonality or trend adjustment.

---

## Data & architecture notes (for future maintainers)

- Backend: `main/ipc/blockHourReport.js` (client-side Autotask reads, both Hatz.ai calls, IPC handlers, and the monthly-generation `registerJob` scheduled job) and `functions/src/functions/blockHourReport.js` (Azure Function routes: `GET`/`POST /block-hour-report/reports`, `PATCH /block-hour-report/reports/{id}`).
- SQL schema migrations: `functions/sql/phase14-block-hour-report-schema.sql` — one table, `bh_reports`, mutated through a `pending → approved/skipped → sent` state machine rather than an append-only log, since the row itself doubles as the Send History entry. `phase15-block-hour-report-adhoc.sql` added the ad hoc report fields (`reportType`/`blockIds`/`periodLabel`) and moved the one-report-per-client-per-month rule to a filtered unique index scoped to `reportType = 'monthly'` (ad hoc rows are unconstrained). `phase16-block-hour-monthly-reissue.sql` (v3.7.2) narrowed that index further, to only block a duplicate while the existing monthly report for that period is still `pending`/`approved` — once it's `sent` or `skipped`, a client can get a fresh monthly report for the same period.
- New `getContractBlocksByContracts()` helper in `main/shared/atHelpers.js` batches `ContractBlocks` queries by `contractID` (chunked 200 IDs at a time, run **sequentially**, not concurrently). The original concurrent per-contract shape tripped Autotask's rate limit at only ~23 contracts during QA — roughly half came back `429`, and a failed contract silently returned "no blocks," which nondeterministically mislabeled real active clients as dormant depending on which contracts happened to fail on a given run.
- Per-client recipients/auto-generate pause live in the shared Company Directory's JSON `metadata` column (`bhRecipients`, `bhAutoGenerate` — see `functions/src/lib/directoryMeta.js`), not this tool's own settings blob, so that table can stay open to any Hub user without needing a separate non-admin settings route.
- `getClientDetailMetrics()` (`main/ipc/blockHourReport.js`) computes the trailing-12-month average, unposted-time warning, and full block history shown on row expand — fetched lazily per client (`bh-get-client-detail` IPC handler) rather than folded into `bh-list-clients`, deliberately, to avoid an eager per-client `BillingItems`/`TimeEntries` fan-out across the whole client list on every dashboard load.
- `computeAdHocRange()` and `hasOverlappingBlocks()` (same file) derive the ad hoc report's date window (earliest selected block's purchase date through the latest selected block's end date, capped at today) and detect whether that window overlaps a non-selected block, which drives the internal-narrative overlap disclaimer.
- `spend12mo` (trailing-12-month spend) and the per-block `hourlyRate` are computed in `computeClientMetrics()` alongside the existing hours math — `hours * hourlyRate` was confirmed live against the matching `BillingItems` "Block Purchase" `extendedPrice` for real clients.
- `main/shared/hubApi.js` gained `getBlockHourReports()` / `postBlockHourReport()` / `patchBlockHourReport()` for the `bh_reports` CRUD calls.
