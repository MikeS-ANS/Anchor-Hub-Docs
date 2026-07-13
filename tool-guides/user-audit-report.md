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

> **Settings are shared.** Everything under the tool's **Settings** tab (supported classifications, license lists, service-account patterns, email text, billing options, digest schedule) is stored centrally — a change applies to **everyone**.

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

On confirm, the tool:
- Updates each changed contact's **Support Classification** UDF.
- **Deactivates** contacts set to "No Longer Active"; **reactivates** contacts set to a supported classification.
- **Creates** the checked M365-only contacts (and, if a contact with that email already exists — active or inactive — updates it instead of making a duplicate).
- Posts an **account note** summarizing confirmed vs. contracted users and the detected changes.

---

## Account Manager Dashboard

The tool opens here for account managers (role-gated). It's the lifecycle view, with collapsible sections filtered to your own clients by default (toggle to All):

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
- **Delta** flags clients **over** their contract (the "up to N" model — at or under is compliant); **Est. $/mo** multiplies the overage by the MSC **Seat Price**.
- Defaults to **out-of-compliance first**. Expand a row for the classification breakdown.
- **+ Note** posts an account note; **Disposition** records a decision (Increase / No change / Waive / Follow-up) and posts a note.

> This tab **never changes the Autotask contract quantity** — it records the decision and posts a note. Contract changes happen through your normal process and the MSC sheet; the number here updates on the next load.

**Billing digest.** The **Digest…** button previews and sends a summary of over-contracted clients (with $ estimate) to the recipients set in Settings. It can also send automatically on a **weekly or monthly** schedule (the Hub must be running for a scheduled send).

---

## Notes & limits

- **Matching is by email** (M365 UPN/mail ↔ Autotask contact email). A contact whose Autotask email differs from both won't auto-match.
- Reports and lifecycle data are stored in SharePoint and shared across the team; history is retained long-term.
- Time entries, notes, and dispositions post as **To-Do items assigned to whoever records them** (a property of the "User Count Audit" Action Type). The Action Type ID is set in Settings.
