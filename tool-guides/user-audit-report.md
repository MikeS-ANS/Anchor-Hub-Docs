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

The tool opens here for account managers (role-gated). It's the lifecycle view, with collapsible sections filtered to your own clients by default (toggle to All). Every client row also shows **Included** (contracted seat count from the MSC workbook), **Full Support** (the client's current full-support user count from the latest audit or write-back), and **Over/Under** (the difference between the two, color-coded) — so drift from the contract is visible without opening Contract Review.

- **Needs an audit run** — never run, or last run over 60 days ago.
- **Sent, not returned — pending** — sent and still within the return window.
- **Sent, not returned — overdue** — past the return-by date, with a **Start Write-Back** button that pre-loads the client and requires a "did not return" note.
- **Good standing** — recently run/returned, with last-ran and last-returned dates.

Every row also has **+ Time** — a quick time entry (default 15 min, work type **Client Success**, editable notes) logged against the client's linked *User Audits* task.

---

## User Count Contract Review

Role-gated to finance/admin. A roll-up of **contracted vs. supported users** per client:

- **Contracted** comes from the MSC sheet's *Included Users*; **Supported** is the count classified as supported (configurable) from the latest audit.
- **Basis** shows **PROVISIONAL** (from the audit as generated) or **CONFIRMED** (client returned the review).
- **Over/Under** flags clients **over** their contract (the "up to N" model — at or under is compliant); **Est. $/mo** multiplies the overage by the MSC **Seat Price**.
- **Show:** filters the table to **All clients**, **Confirmed positives** (over-contracted and confirmed — the actionable set), or **Provisional positives** (over-contracted but not yet confirmed — a look-ahead at what's likely coming).
- Defaults to **out-of-compliance first**. Expand a row for the classification breakdown.

**Increase** — for a CONFIRMED, over-contracted client, the row's primary button walks you through updating the price/cost/description on the Autotask contract and the MSC workbook in three tracked steps:

1. **Update Calculator** — opens the client's Service Plan Calculator in SharePoint; set the new user count there, then type the resulting price and cost back into the Hub.
2. **Update Autotask** — pushes the new invoice description, price, and cost to the matched contract service, and posts a Contract Note on the contract itself (the same Contract Notes tab your own manual notes live on).
3. **Update MSC Sheet** — runs automatically once Step 2 succeeds; updates Included Users and adds a dated note to the workbook's Contract Notes column.

The row's button label tracks real progress ("Continue (Step 2 of 3)," etc.) with a small progress indicator, and the estimated $/mo impact shows before you start. **📜 Increase History** (next to Digest/Reload) lists every client that's gone through the flow, across every cycle — useful for "what did we actually increase this month," and includes a way to remove stale/test entries.

**+ Note** (🗒) posts a free-text account note. **Disposition** is for clients you're **not** increasing this cycle — it records why (No change / Waive / Needs follow-up, or a manual-increase note if you handled it outside the Hub) and posts a note; it never touches the Autotask contract.

**Billing digest.** The **Digest…** button previews and sends a summary of **confirmed** over-contracted clients (with $ estimate) to the recipients set in Settings — provisional clients are excluded, since they're not yet actionable. It can also send automatically on a **weekly or monthly** schedule (the Hub must be running for a scheduled send).

---

## Notes & limits

- **Matching is by email** (M365 UPN/mail ↔ any of the Autotask contact's three email fields — primary plus the two secondary slots) for the main report. Write-back adds a name-based safety net on top (see **Write-back** above) to catch a drifted-email duplicate, but it's a review-flag heuristic, not a source of truth — always confirm the flagged match is actually the same person before choosing "Update existing."
- Reports and lifecycle data are stored in SharePoint and shared across the team; history is retained long-term.
- Time entries, notes, and dispositions post as **To-Do items assigned to whoever records them** (a property of the "User Count Audit" Action Type). The Action Type ID is set in Settings.
