# Payroll Review

> **Beta.** Shipped in v3.9.0. Access restricted — see [Access](#access) below.

Payroll Review replaces the manual, ad hoc pre-processing review Mike used to run in a separate Claude chat session before every payroll submission. Heather still exports the same Paylocity reports and uploads them to the same SharePoint folder — the Hub now reads them directly, applies months of accumulated payroll knowledge automatically, and gives Mike and Heather a shared, in-app workspace to review, discuss, and sign off before Heather submits payroll.

This is the highest-sensitivity tool in the Hub. It touches individual compensation data for every ANS employee.

---

## File checklist

Opening a pay period shows a checklist of the files Heather uploads for that period, each with a clear status:

| File | Required? | If missing |
|---|---|---|
| Pre Process Payroll Comparison (XLSX or PDF) | **Yes** | Parse is blocked until it's present |
| Pre Process Payroll Register (PDF) | **Yes** | Parse is blocked until it's present |
| Pre Process Payroll Summary (PDF) | No | Noted, review continues |
| New Hire Date (PDF) | No | Noted, review continues — assumed no new hires |
| Termination Listing (PDF) | No | Noted, review continues — assumed no terminations |

The Comparison report can arrive as either an Excel file or a PDF — Paylocity has switched this export's format before, so both are read the same way. If Heather uploads a corrected `_V2` file after a mistake is found, it's picked up automatically and used instead of the original, with the Register PDF cross-checked for any employee whose numbers look stale in the V2 file (a known Paylocity export quirk).

A manual upload option is available if SharePoint access is briefly unavailable or a file needs to be added ad hoc — anything uploaded this way is still written back into the correct SharePoint folder, so SharePoint stays the single source of truth.

---

## Standing rules — auto-resolving known-safe patterns

Before anything is shown to a reviewer, Payroll Review runs every active **standing rule** — a library of known, safe, recurring patterns built from months of real payroll history — to automatically clear anything that doesn't need a human look. Examples of the kind of thing a standing rule handles: a pay code that's expected to be absent on certain check dates, an employer-paid benefit contribution that occasionally shows as "new" purely because of a benefits-system sync in progress, or a recurring deduction (like a loan repayment) that's expected at a fixed amount every single period until it ends.

Standing rules are managed on their own **Standing Rules** tab — view every active and retired rule, see who approved it and when, and add, edit, or retire one directly. New rules can also be proposed by the AI chat (see below) when it notices something repeating, but a proposal only ever becomes a real rule after a person reviews and approves it from this tab.

---

## Flags

Anything a standing rule didn't clear gets classified into one of four tiers:

- **Action Required** — needs attention before payroll goes out (e.g. an active employee showing $0 net pay, or a pattern that's been declining for several consecutive periods).
- **Verify** — needs a quick confirmation (e.g. a bonus or reimbursement was added — was it authorized?).
- **Watch** — worth noting but not yet a pattern (e.g. one employee's hourly pay dipped once — could easily be normal).
- **Confirmed / Info** — already explained (e.g. a termination showing the expected $0, or a new hire's first prorated check).

Each flag shows the finding, Heather's own comment from the source file when she left one (only available when the Comparison report is Excel — the PDF format doesn't carry a comment column), and a note if the same underlying issue was already flagged last period, so it's referenced rather than raised again from scratch. A flag has its own note field and a sign-off control that records who resolved it and when, straight from your signed-in Hub account — no typing your name.

If a correction comes in later (a V2 file, a late supplemental PDF) and a period gets re-parsed, sign-offs and notes on unaffected flags are preserved. A flag whose underlying numbers changed after it was already signed off is flagged again with a clear "data changed since sign-off" marker rather than silently disappearing or silently staying resolved.

---

## Employees tab

A filterable, sortable, searchable table of every employee in the period — prior net pay → current net pay → change → any flag/note for that employee — so you can scan the whole company at a glance, not just the flagged exceptions.

---

## Pre-processing checklist & gate

A short checklist mirrors what used to live in the old standalone report, ending in one final item: **"All items resolved — OK to process payroll."** This is the human gate. Nothing about the Notify email is available until it's checked.

---

## Notify Puzzle

Once the gate is checked, **Notify Puzzle — Approved to Submit** opens an editable email preview (subject and intro are customizable per user in Settings) and sends it via Microsoft Graph once you click Send — Heather then proceeds with the actual submission in Paylocity, exactly as before.

---

## Audit Log

Every parse, flag resolution/reopen, note edit, standing-rule change, AI-proposed-rule decision, and approval-email send is logged with who did it and when. This log is completely separate from the Hub's shared activity log used by every other tool — kept isolated so that even the *existence* of a payroll action never becomes visible to anyone without payroll access.

---

## AI Prompt — the chat assistant

Payroll Review includes a read-only AI chat, scoped to the review currently open and the full payroll history behind it. It's the **first tool-calling AI feature in the Hub** — instead of stuffing everything into one prompt, the model is given a small set of on-demand tools it can call as needed:

- **Find an employee** by name (a first name is enough) and resolve it to their record
- **Get an employee's pay history** across every period on record — no date limit
- **Get flag history** for an employee, a pay code, or a date range, including how it was resolved
- **Get standing rules**, filtered to an employee or a code
- **Get the current review's summary** — flags, totals, and sign-off status

**System prompt (`main/shared/hatzToolChat.js` runs the loop; `main/ipc/payrollReview.js` owns the prompt and tools):**
> You are the Payroll Review assistant inside Anchor Hub, used by Mike (CISO/Managing Director) and Heather (payroll admin) at Anchor Network Solutions to review payroll before it's submitted.
>
> You have read-only tools that query real payroll review data... Always use a tool rather than guessing or recalling a number from earlier in the conversation -- data can change between questions.
>
> Every data tool... takes an Emp ID, never a name. When the user names an employee instead of giving an Emp ID, call find_employee FIRST to resolve it...
>
> Hard rules, no exceptions:
> - You are strictly read-only and advisory. You cannot resolve a flag, sign off on a period, send the approval notification, or create/edit/retire a standing rule -- there is no tool for any of that, on purpose.
> - If asked to create or change a standing rule, do not attempt it yourself. Instead, explain your reasoning in plain text, then emit a fenced proposal block for a human to review from the Standing Rules tab -- you never create the rule yourself, even if asked directly.
> - You can accurately answer direct data questions... For "why did X change" questions, you can only surface what data and comments actually exist -- you do NOT know external causes... Never invent a causal explanation you don't have data for.

If asked to create or change a standing rule, the chat drafts a proposal (name, proposed rule text, and its reasoning) instead of acting — that proposal shows up on the Standing Rules tab for a person to approve or reject, the same as a manually-typed one.

Model is Claude Sonnet by default (a deliberately stronger model than most other Hatz.ai integrations in the Hub, given the multi-step tool-use judgment this scope requires).

---

## Access

Payroll Review does **not** use the Hub's general Role Matrix. It's gated end-to-end — including every read, not just writes — by a dedicated Azure AD role, `hub.payroll`, held only by Mike and Heather. This deliberately excludes `hub.admin`: unlike every other tool in the Hub, an admin role does not grant visibility here. If you believe you should have access, that's a conversation with Mike directly, not a Hub Role Matrix change.

---

## Known open items

- The PDF format of the Comparison report doesn't carry Heather's per-employee comment column at all — a period parsed from a PDF won't show her explanatory notes the way an Excel-sourced period does.
- A small number of standing-rule condition types (timing-exception fields beyond the basic code match, and some expected-value fields) are editable from the Standing Rules UI but not yet read by the actual rule-checking logic — those fields are clearly marked "not currently enforced automatically" in the edit form so editing them doesn't create a false sense that they're already active.
