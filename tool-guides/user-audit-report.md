# User Audit Report

The User Audit Report replaces the every-other-month manual user audit. It joins your Autotask contacts with **live Microsoft 365 data (via CIPP)** to surface people who are **licensed in M365 but missing from Autotask** — the users you may not be billing for — then produces a client-facing review workbook, writes the client's confirmed answers back to Autotask, and tracks the whole lifecycle for account managers and finance.

---

## Before you start — one-time setup

Most of this tool reads from the **Company Directory** so it doesn't hit the API on every run. Set these up first (Company Mapping tool):

| Setting | Where | Why |
|---|---|---|
| **MSC name → Autotask** | Company Mapping → MSC Clients | Links each MSC agreement client to its Autotask company. Drives the client list and the contracted user count. |
| **CIPP domain** | Company Mapping → Companies | Optional override when the client's name doesn't match its M365 tenant. |
| **Google Workspace flag** | Company Mapping → Companies | Marks clients with no M365 — the report skips CIPP for them. |
| **SharePoint folder** | Company Mapping → Companies | The client's folder in the **ANS-Clients** library where reports are saved. |
| **Time project/task** | Company Mapping → Time Projects | Links each client's *Client Time Tracking* project + *User Audits* task so time entries can be logged. Use **Auto-match**, or enter IDs manually, and **Re-sync** after the new-year projects are created. |

> **Global setting — shared by everyone.** Everything under the tool's **Settings** tab (supported classifications, license lists, service-account patterns, email text, billing options, digest schedule) is centrally managed via Azure App Configuration. The Settings screen shows the current values but the Save button won't apply a change — contact Mike (or whoever holds the `hub.admin` role) to update anything here.

**Autotask keys:** reading uses the shared read-only key automatically. Anything that **writes** to Autotask (write-back, contact creation, notes, time entries, dispositions) requires **your personal Autotask API key** in Settings → Autotask.

---

## Generate

1. Open **User Audit Report → Generate**.
2. Pick a **Report type**:
   - **Client-facing (2 tabs)** — Instructions + User Review. This is what the client sees.
   - **Full report (6 tabs)** — adds Service Accounts, Unlicensed, External, and an internal Summary.
   - **Both** — produces both files in one run.
3. Use the **search box** to filter, then check the clients to run (nothing is selected by default). Unlinked clients show "Not linked" and can't be run until linked in the Company Directory.
4. *(Optional)* **Create Autotask CRM note** logs a "User Count Audit" To-Do on each company. Requires your personal key.
5. Click **Generate**.

Each report is saved to the client's **ANS-Clients / {folder} / User Audits / {year}** location (and your Downloads). If a client has no SharePoint folder, it saves to Downloads only and says so.

**The User Review tab** pre-fills each contact's current *Support Classification*, with a drop-down for the client to change. Rows licensed in M365 but not in Autotask appear under a **"Review Required"** divider — the headline gap. A hidden baseline column lets the tool highlight exactly what the client changed on return.

**Watch for the ⚠ no-M365-data warning.** If a client isn't Google Workspace but CIPP returned no users, the run is flagged (not silently "successful") — check the tenant match / CIPP domain.

**Email to client.** After a run, enter one or more recipients (separate with `;` or `,`) and Send. The email uses an action-oriented template with the return-by deadline, and attaches the **current SharePoint copy** — so edits you make in SharePoint after generating are included. The subject and intro are editable in Settings; your cloud email signature is applied automatically.

---

## Write-back

When the client returns their workbook:

1. Open **Write-back** and pick the client — the tool **auto-loads their latest report from SharePoint**. (Or upload a returned file manually; manual uploads are pushed back to SharePoint.)
2. The preview shows **only the changed rows** (current → new), plus a **"New Autotask contacts to create"** list for any M365-only rows the client re-classified (with a per-row classification drop-down and bulk-set).
3. Add an optional account note, choose whether to post it as a CRM To-Do, and **Confirm**.

**⚠ Possible existing contact.** M365-only rows are also checked by name against the client's own Autotask contacts (not just email) — a person whose Autotask email has drifted from their current M365 address still has the same name, and email-only matching would miss that and create a duplicate. A likely name match shows an orange warning badge with the matched contact's name and email, and the row requires an explicit per-row choice instead of a checkbox:
- **Update existing** — updates the matched Autotask contact's classification, and sets its email to the M365 address (moving the old address to the secondary email slot if that slot isn't already in use — if it is, the email is left alone and flagged as needing manual resolution).
- **Create new anyway** — creates a new contact as normal, for when the name match is a coincidence (e.g. two different people with the same surname).

Leaving neither option selected excludes that row from the write-back, the same as leaving an ordinary row's checkbox unchecked.

On confirm, the tool:
- Updates each changed contact's **Support Classification** UDF.
- **Deactivates** contacts set to "No Longer Active"; **reactivates** contacts set to a supported classification.
- **Creates** the checked M365-only contacts (and, if a contact with that email already exists — active or inactive — updates it instead of making a duplicate), or **updates** the linked contact for rows resolved as "Update existing" above.
- Runs one more **live check** against Autotask's current contacts immediately before each create — not just against the possibly-stale generated report — so a name match that only exists in Autotask today (created after the report was generated, say) is never silently duplicated. These are skipped with a **needs-review** status in the progress list and counted separately in the summary; resolve them manually and re-run write-back for just that person next cycle.
- Posts an **account note** summarizing confirmed vs. contracted users, the detected changes, and any matched-and-updated or needs-review counts.

---

## Account Manager Dashboard

The tool opens here for account managers (role-gated). It's the lifecycle view, with collapsible sections filtered to your own clients by default (toggle to All). Every client row also shows **Included** (contracted seat count from the MSC workbook), **Full Support** (the client's current full-support user count from the latest audit or write-back), and **Over/Under** (the difference between the two, color-coded) — so drift from the contract is visible without opening Contract Review. These three columns are **full-time seats only**. A client with any part-time users shows Included and Full Support as **"X + Y"** (full-time + part-time) instead of a plain number, and a **PT +N** badge appears next to the client's name whenever more part-time users are supported than contracted — a signal that a manual contract conversation is needed, not something the Hub acts on. Part-time counts never affect Over/Under and are never written back anywhere automatically.

> The MSC workbook's part-time column is new and mostly unfilled right now. Until it's filled in for a given client, that client's contracted part-time count reads as zero — so you may see a row like Full Support "7 + 2" next to a plain "7" for Included, with a PT badge showing. That's expected on an unfilled column, not a bug.

- **Needs an audit run** — never run, or last run over 60 days ago.
- **Sent, not returned — pending** — sent and still within the return window.
- **Sent, not returned — overdue** — past the return-by date, with a **Start Write-Back** button that pre-loads the client and requires a "did not return" note.
- **Good standing** — recently run/returned, with last-ran and last-returned dates.

Every row also has **+ Time** — a quick time entry (default 15 min, work type **Client Success**, editable notes) logged against the client's linked *User Audits* task.

---

## User Count Contract Review

Role-gated to finance/admin. Opens to the **Confirmed positives** view every time you enter the tab — the actionable set. Switching the **Show** dropdown to All clients or Provisional positives during your session still works as before; **Reload** keeps whatever you last selected, but leaving the tab and coming back resets to Confirmed positives again.

A roll-up of **contracted vs. supported users** per client:

- **Contracted** comes from the MSC sheet's *Included Users*; **Supported** is the count classified as supported (configurable) from the latest audit. Both are **full-time counts**, and both display as **"X + Y"** (full-time + part-time) for a client with any part-time users, or a plain number otherwise.
- **Basis** shows **PROVISIONAL** (from the audit as generated) or **CONFIRMED** (client returned the review).
- **Over/Under** flags clients **over** their contract on **full-time seats only** (the "up to N" model — at or under is compliant); **Est. $/mo** multiplies the overage by the MSC **Seat Price**. Part-time seats never factor into Over/Under, the Increase flow, or the billing digest — see **Part-time users** below.
- **Show:** filters the table to **All clients**, **Confirmed positives** (over-contracted and confirmed — the actionable set, and the default view), or **Provisional positives** (over-contracted but not yet confirmed — a look-ahead at what's likely coming).
- A client already **dispositioned this cycle** as *No change (within contract)*, *Waive / grandfather*, or *Increase contract (handled manually, outside the Hub)* drops out of Confirmed Positives (and the billing digest) even if it's still technically over contract — that reflects a decision already made, not a bug. *Needs follow-up* and clients with no disposition yet keep showing. This resets automatically the next time a new audit cycle starts, so a client still over contract reappears then.
- Defaults to **out-of-compliance first**. Expand a row for the classification breakdown.
- **Open MSC Sheet** / **Open MRR Sheet** buttons open each source workbook directly in your browser — handy for double-checking a number or making a manual entry. Both show an "Opening…" state while resolving; the SharePoint lookup can take up to a minute on this network.

**Increase** — for a CONFIRMED, over-contracted client, the row's primary button walks you through updating the price/cost/description on the Autotask contract and the MSC workbook in three tracked steps:

1. **Update Calculator** — opens the client's Service Plan Calculator in SharePoint; set the new user count there, then type the resulting **price, cost, and new user count** back into the Hub. All three are now required — Save is blocked with an inline message until every field has a number, so a blank user count can no longer silently fall back to the audited figure.
2. **Update Autotask** — pushes the new invoice description, price, and cost to the matched contract service, and posts a Contract Note on the contract itself (the same Contract Notes tab your own manual notes live on).
3. **Update MSC Sheet** — runs automatically once Step 2 succeeds; updates Included Users and adds a dated note to the workbook's Contract Notes column, then logs the increase into the **MRR Contract Changes** workbook (see below).

The row's button label tracks real progress ("Continue (Step 2 of 3)," etc.) with a small progress indicator, and the estimated $/mo impact shows before you start. **📜 Increase History** (next to Digest/Reload) lists every client that's gone through the flow, across every cycle — useful for "what did we actually increase this month," and includes a way to remove stale/test entries.

**+ Note** (🗒) posts a free-text account note. **Disposition** is for clients you're **not** increasing this cycle — it records why (No change / Waive / Needs follow-up, or a manual-increase note if you handled it outside the Hub) and posts a note; it never touches the Autotask contract. Recording any disposition other than *Needs follow-up* is also what pulls a client out of Confirmed Positives for the rest of this audit cycle (above).

**Billing digest.** The **Digest…** button previews and sends a summary of **confirmed** over-contracted clients (with $ estimate) to the recipients set in Settings — provisional clients are excluded, since they're not yet actionable, and so is any client already dispositioned this cycle (same rule as Confirmed Positives, above). It can also send automatically on a **weekly or monthly** schedule (the Hub must be running for a scheduled send).

Two things about the digest are worth knowing, because both are silent when they happen:

- **It will not send from an incomplete read.** The digest is built from the MSC workbook and the Company Directory. If either fails to load — most often a SharePoint permission problem — the Hub refuses to send rather than mail out a digest that understates what clients owe. The preview still opens so you can see what went wrong, with a red banner naming the source that failed, and **Send** stays disabled until it loads. The same red banner appears on the Contract Review list itself, so a short list is never mistaken for a quiet month. A scheduled send that hits this reports a failed scan rather than sending anything, and — importantly — does **not** consume that week's or month's send: another authorised person's Hub can still send it later in the same window. If nobody else's is open, that scheduled run is simply missed rather than sending something wrong — send it by hand from **Digest…** once the data loads.
- **Only people with Contract Review access run it.** The scheduled send fires from a running copy of the Hub, and it now only fires from one belonging to someone who actually holds Contract Review access (the Finance or Admin role). Previously any signed-in machine that happened to be open could be the one that ran it.

### Part-time users

The MSC workbook now has a dedicated **Included Part Time Users** column, separate from the full-time seat count everything above is based on. Part-time seats are tracked for visibility only:

- They're never part of Contracted/Supported's full-time number, Over/Under, the Increase flow, the Autotask push, or the MSC write-back — closing a part-time overage is always a manual contract conversation, not something the Hub automates.
- A client with any part-time users shows Contracted/Supported as **"X + Y"** (full-time + part-time) instead of a plain number, on both this tab and the Account Manager Dashboard.
- A **PT +N** badge appears next to the client's name when more part-time users are currently supported than contracted — a flag to have that conversation, nothing more.
- The part-time column is brand new in the MSC workbook and mostly blank right now. Until a client's contracted part-time count is filled in, it's treated as zero — so it's normal, not a bug, to see a client's Supported column read "7 + 2" next to a Contracted column that's still a plain "7", with a PT badge showing.

### MRR Contract Changes log

The moment Step 3 of an Increase (the MSC sheet update) succeeds, the Hub writes one row into the **MRR Contract Changes** workbook's "CHANGES MRR +/-" section for the quarter matching Step 2's effective date — company name, the seat delta, the new per-user price, the effective date from Step 2, and your increase note. New contracts and cancellations are still entered by hand; only existing-client seat/price increases are logged automatically.

- The quarter the row lands in — and the **year tab** it goes into — both come from Step 2's **effective date**, not from the day you make the push. So a change you key in March that takes effect 1 April is Q2 revenue and lands in Q2. This keeps the section the row sits in agreeing with the date printed on the row itself.
- A **quarter dropdown** next to the effective date shows that quarter pre-selected, labelled with its year (e.g. `Q2 2026`), and relabels itself if you change the date. Override it when you're logging a genuine late entry — a change that was effective last quarter, say. If you pick a quarter yourself, editing the date afterwards won't move your choice.
- An increase effective in **January** belongs to the *next* year's tab. If that tab hasn't been created in the workbook yet, the log write fails and tells you which tab to create — it will not quietly drop the row into the current year's Q1, which is a closed quarter. Worth creating next year's tab before the December/January run.
- The write only happens once per increase — repeating or retrying a push won't double-log it. If the log write itself fails, Step 3 still shows the MSC sheet update as successful (that part already happened and is never undone) alongside a separate warning telling you to open the MRR sheet and log that entry by hand.

---

## Notes & limits

- **Matching is by email** (M365 UPN/mail ↔ any of the Autotask contact's three email fields — primary plus the two secondary slots) for the main report. Write-back adds a name-based safety net on top (see **Write-back** above) to catch a drifted-email duplicate, but it's a review-flag heuristic, not a source of truth — always confirm the flagged match is actually the same person before choosing "Update existing."
- Reports and lifecycle data are stored in SharePoint and shared across the team; history is retained long-term.
- Time entries, notes, and dispositions post as **To-Do items assigned to whoever records them** (a property of the "User Count Audit" Action Type). The Action Type ID is set in Settings.
