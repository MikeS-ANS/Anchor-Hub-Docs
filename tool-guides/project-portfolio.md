# Project Portfolio

Project Portfolio is one screen that shows every open client project, read straight out of Autotask. It replaces the Planner board the Professional Services team kept by hand, where every field had to be re-typed from Autotask and quietly went stale — the board showed a project at 0% that Autotask knew was 62% done.

## Who sees it

Project Portfolio is intended for Admin, Manager, Projects and Delivery roles, and access is granted in Access Management.

The tool only appears in your sidebar after **two** separate steps, not one:

1. Somebody with admin access grants your role the tool in **Access Management**.
2. You tick **Project Portfolio** in **Sidebar Layout** (bottom-left navigation bar) and save.

The second step is easy to miss. A newly added tool lands in an existing person's saved sidebar layout switched off, so even after the grant is in place, Project Portfolio stays hidden until you turn it on.

## Where the data comes from, and how fresh it is

Nothing on this screen is typed in by a person. Every night at about 4 AM Mountain, the Hub's server reads every open client project from Autotask (plus projects completed in the last year or so), works out who the Technology Strategist, Account Manager and TAM are from the client's Autotask account team, and stores the result. The header shows **Last synced** with the time and whether it was the nightly run or a manual one.

**Sync Now** runs the same read immediately. It usually takes well under a minute. If it fails, the header says so in red and the grid keeps showing the last good data — nothing is ever half-updated.

Four things can be changed from this screen. Three write to Autotask under **your own** Autotask API key (Settings → Autotask): the project's status, a new dated status update, and the scope of work. The fourth — Labels and the StepUP IT flag — lives only in the Hub and needs no Autotask key. Everything else is read-only.

## The grid

One row per project, sorted by finish date. Click any column header to sort by it; click again to reverse.

* **Name** — client and project. Pinned on the left so it stays visible as you scroll sideways.
* **Lead** — the Autotask project lead.
* **Status** — the newest dated entry in the project's status log (the `statusDetail` field in Autotask), with a note of how many earlier entries exist. "No status yet" means nobody has written one.
* **Phase** — derived from the Autotask status: Discovery → Planning → Executing → Monitoring & Controlling → Closure, plus On Hold. **Needs a status** in amber means the project is sitting on plain "New" or "In Progress", which tells the board nothing about where it really is — pick a specific status in Autotask (IKO Scheduled, Go-Live Targeted, Schedule CKO, and so on) and it moves to the right phase on the next sync. An **Unmapped** phase means Autotask has a status this tool has not been told about; ask an admin.
* **Start / Finish** — from Autotask. A finish date in the past on an open project shows in red.
* **% Complete** — computed by Autotask from task completion. It cannot be edited anywhere in the Hub.
* **Project #**, **Technology Strategist**, **Account Manager**, **TAM** — a dash means no active ANS person holds that role on the account. People who have left ANS are never shown, even if Autotask still lists them on the team.
* **Labels** — Hub-only tags and the StepUP IT flag, edited from the detail panel.

## Filters

**Department** defaults to Professional Services. **Phase** and **Lead** narrow the list. **Scope** is Open by default; **Include completed** adds projects finished within the chosen window (last 30 days, last month, last quarter, last year, or a date range). **Search** matches client, project name or project number.

## The Board view

The **Grid | Board** switch in the header shows the same open projects as cards in six phase columns — Discovery, Planning, Executing, Monitoring & Controlling, Closure, On Hold — plus an **Unmapped** column when a project carries a status the map does not know. The Department, Lead and Search filters apply to both views; the Board never shows completed projects. Each card shows the client and project, the finish date (red when past), the newest status entry, the lead and % complete, and the same **Autotask status** dropdown as the panel: pick a new status, confirm, and the card moves to the column that status maps to. Cards cannot be dragged — several statuses share a phase, so a drag could not tell which status you meant.

## The detail panel

Click a row or a card. The panel shows the overview (dates, hours against estimate, duration), the account team, labels, the **full status history**, the scope of work, and the most recent writes made through the Hub. **Open in Autotask** jumps to the project in the Autotask web app.

The history merges two sources: the entries still in Autotask's 2,000-character `statusDetail` field, and every **Project Status** note on the project — your Hub status updates, entries the Hub moved into a note when the field filled up, the one-time history archive, and notes typed directly in Autotask. Entries that survive only in notes carry a small **note** badge. The history loads a moment after the panel opens; until then you see the entries from the field.

## Changing a status

Pick a new value in the **Autotask status** dropdown. A confirmation shows what you are changing from and to, and which phase the card will move to. Click **Write to Autotask**. The Hub then:

1. re-reads the project from Autotask, so it is not working from a stale copy;
2. writes the new status;
3. reads it back and only reports success if Autotask actually holds the new value.

If someone changed the status in Autotask after the last sync, you see **Status changed in Autotask since the last sync** with the board's value, Autotask's current value and the one you picked. **Cancel** keeps Autotask's value and refreshes the row; **Write anyway** replaces it.

If Autotask accepts the write but the read-back shows the old value, you see **Write did not land** and nothing on the board changes. Try again, or make the change in Autotask and run Sync Now.

## Adding a status update

Click **Add status update**, type what happened, what is next and when, then **Save to Autotask**. The update is stored as a **Project Status** note on the Autotask project (up to 32,000 characters — the full history lives there permanently). Autotask's older `statusDetail` field, which is capped at 2,000 characters, is then rebuilt to hold the newest entries that fit, newest first, in the same `M/D - text` form the team has always typed, so anyone reading the project in Autotask still sees recent history. Entries that no longer fit are copied into a second note before they are removed — nothing is trimmed until it has been confirmed stored.

If the note saves but the `statusDetail` rebuild does not, the Hub says so and keeps the note — your update is safe; run Sync Now to refresh the row. When older entries are moved to a second note, the confirmation line under the panel says how many.

The line under the history — "statusDetail currently mirrors the newest N entries · X / 2,000 characters" — tells you how much of the history Autotask's own field still shows. The full history is always in the project's notes.

## Editing the scope of work

Click **Edit** beside the counter, change the text and **Write to Autotask**. Autotask holds 8,000 characters; the counter turns red past that and the save is refused rather than letting Autotask truncate the text.

## Labels and the StepUP IT flag

Click **Edit** beside **Labels · Hub-owned**, add labels (pick one already in use from the list or type a new one), remove with ×, tick **StepUP IT project** if it applies, then **Save**. These live in the Hub only — nothing is written to Autotask — and every change is recorded in the same audit table as the Autotask writes so it is clear who changed what. Up to 20 labels of 60 characters each.

## Recent writes from the Hub

The bottom of the panel lists the newest ten writes made through Anchor Hub on this project — status changes, status updates, scope edits and label changes — with who made them and when. Changes made directly in Autotask are not listed; the history above already shows those.

## Settings (admins)

Admins see a **Settings** button in the header. It lists every Autotask project status — including inactive ones and any the board has not seen yet — with the phase it lands in, whether it counts as **Generic** (plain New or In Progress, which raises **Needs a status**), and how many cached projects carry it. A status can also be **Excluded** (never on the board) or left **Not mapped**. Below the table are the job titles that identify a client's **Technology Strategist** and **TAM** on the Autotask account team; a person is shown when their title contains one of them. **Save and re-sync** stores the settings and runs Sync Now so every project's phase follows the new map. Client Touch Aging keeps its own copy of the title lists. If a sync is already running when you save, the header says so and you run Sync Now yourself once it finishes.

## Archive status history (admins, once)

An admin can run **Archive status history** once. It copies every project's existing status text into a Project Status note titled "Status history before Anchor Hub", so pre-Hub history is preserved in the notes before the tool ever rebuilds the field. Running it again creates nothing new.

It was run on 2026-09-17 for open projects only (21 notes). Completed projects were left alone — the Hub never rewrites their status field, so nothing there is at risk.

## Every write is recorded

Each status change, status update, scope-of-work edit and label or StepUP IT flag change is written to a Hub audit table with who made it and the before/after values. It is separate from the Hub's general activity log because client project narrative should not be readable by every signed-in employee.

## Known open items

* The Board's Department, Lead and Search filters are shared with the Grid; the Grid's Phase and Scope filters do not apply to the Board.
* Writing to Autotask needs your personal Autotask API key. Without one, the dropdowns and the status/scope buttons are disabled and the panel says why; Labels can still be edited.
* If Autotask rejects the Hub's record of a write (someone changed the project in the seconds between the write and the read-back), the Hub offers one retry; if that fails too, the value is correct in Autotask and Sync Now refreshes the row.
