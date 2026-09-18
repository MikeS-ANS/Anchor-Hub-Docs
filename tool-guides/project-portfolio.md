# Project Portfolio

Project Portfolio is one screen that shows every open client project, read straight out of Autotask. It replaces the Planner board the Professional Services team kept by hand, where every field had to be re-typed from Autotask and quietly went stale — the board showed a project at 0% that Autotask knew was 62% done.

## Who sees it

Project Portfolio is intended for Admin, Manager, Projects and Delivery roles, and access is granted in Access Management.

The tool only appears in your sidebar after **two** separate steps, not one:

1. Somebody with admin access grants your role the tool in **Access Management**.
2. You tick **Project Portfolio** in **Sidebar Layout** (bottom-left navigation bar) and save.

The second step is easy to miss. A newly added tool lands in an existing person's saved sidebar layout switched off, so even after the grant is in place, Project Portfolio stays hidden until you turn it on.

## Where the data comes from, and how fresh it is

Nothing on this screen is typed in by a person. Every night at about 4 AM Mountain, the Hub's server reads every open client project from Autotask, plus any project Autotask still shows as recently completed, works out who the Technology Strategist, Account Manager and TAM are from the client's Autotask account team, and stores the result. The header shows **Last synced** with the time and whether it was the nightly run or a manual one.

Once a completed project has been picked up this way, it stays in the Hub for good — a finished project is never removed just because time has gone by. That's what makes the **Completed** and **All** views (see Filters, below) genuinely useful for counting how many projects wrapped up over a longer stretch, not just the last month or two.

**Sync Now** runs the same read immediately. It usually takes well under a minute. If it fails, the header says so in red and the grid keeps showing the last good data — nothing is ever half-updated.

Occasionally the projects read succeeds but the **Project Status** notes behind the detail panel's history do not. The header then says **status notes could not be read** in amber: the grid is current, and the stored notes from the last good run are kept rather than cleared, so the history you see may be a run or two behind. Run Sync Now again to refresh them.

Four things can be changed from this screen. Three write to Autotask under **your own** Autotask API key (Settings → Autotask): the project's status, a new dated status update, and the scope of work. The fourth — Labels and the StepUP IT flag — lives only in the Hub and needs no Autotask key. Everything else is read-only.

## The grid

One row per project, sorted by finish date. Click any column header to sort by it; click again to reverse. The grid scrolls inside its own box with the column headers pinned at the top, so on a long list the horizontal scrollbar sits at the bottom of your screen where you're actually looking, instead of trailing off below the last row.

* **Name** — client and project. Pinned on the left so it stays visible as you scroll sideways.
* **Lead** — the Autotask project lead.
* **Status** — the newest dated entry in the project's status log (the `statusDetail` field in Autotask), with a note of how many earlier entries exist. Long status text is clipped to a few lines in the grid — hover it to read the whole entry without opening the project. "No status yet" means nobody has written one.
* **Phase** — derived from the Autotask status: Discovery → Planning → Executing → Monitoring & Controlling → Closure, plus On Hold. **Needs a status** in amber means the project is sitting on plain "New" or "In Progress", which tells the board nothing about where it really is — pick a specific status in Autotask (IKO Scheduled, Go-Live Targeted, Schedule CKO, and so on) and it moves to the right phase on the next sync. An **Unmapped** phase means Autotask has a status this tool has not been told about; ask an admin.
* **Start / Finish** — from Autotask. A finish date in the past on an open project shows in red.
* **% Complete** — computed by Autotask from task completion. It cannot be edited anywhere in the Hub.
* **Project #** — the Autotask project number.
* **Billing** — **Fixed fee**, **T&M**, **Not tagged**, or a dash. This is read from the contract's **name** in Autotask, not from Autotask's "contract type" field — every project contract at ANS is set up as Fixed Price in Autotask regardless of how the work is actually billed, so contract type would be actively misleading here. Whoever named the contract typed the real billing arrangement into the name instead, and that's what this column reads. **Not tagged** means a real project contract whose name carries no billing suffix — that is the list worth cleaning up, and the filter of the same name pulls up exactly those. A **dash** means the question doesn't apply or can't be answered — no contract at all, a client-level agreement that was never meant to carry a billing tag, or a contract the Hub hasn't been able to read. Hover the cell and it tells you which; Estimated revenue, below, lists every case.
* **Estimated revenue** — the dollar estimate on the **project's own contract**, and only that. Most open projects aren't attached to a contract of their own; they're attached to something at the client level — Recurring IT Services, a Block Hours agreement, T&M Support — and the estimate on one of those is what that client pays for the whole arrangement, typically for a year. Showing it here would read as the project's value, and it isn't: of the open projects that carry a contract at all, only about 1 in 6 is on a genuine project contract, and pulling the client figures in made the portfolio total read roughly twenty-five times larger than the real project money. The Hub now tells the two apart using Autotask's contract **category**, not the contract's name, so nothing depends on how a contract was typed. Everything else shows a dash, and hovering the cell (or opening the project) says which of these it is: no contract at all; a client agreement, named, with a note that its revenue is the client's rather than the project's; a contract whose category the Hub hasn't read yet; a project contract with no estimate entered; a project contract whose estimate is genuinely $0; or a contract the Hub couldn't read at all. The cell itself never shows $0, so "no estimate entered" and "a real $0" look identical in the grid — but the hover now says which of the two it is, rather than leaving you to go and check the contract in Autotask.

  One consequence worth knowing: if **every** row in the grid shows a dash, the Hub hasn't read any contract's category yet. That's the expected state between this change landing and the first sync after it, and the hover says exactly that. Run **Sync Now** rather than waiting for the overnight run.
* **Technology Strategist**, **Account Manager**, **TAM** — a dash means no active ANS person holds that role on the account. People who have left ANS are never shown, even if Autotask still lists them on the team.
* **Labels** — Hub-only tags and the StepUP IT flag, edited from the detail panel.

The summary line above the grid also totals **Estimated revenue** for whatever is currently in view — something like "$125,090 across 15 of 17." The second number is how many of the visible projects actually had a figure to add up, so you can tell a real total from one that's quietly built on top of a lot of dashes. One contract can cover more than one project, and the total counts it **once**; when that happens the line says so, because otherwise "across 15" would imply fifteen separate amounts were added together.

## Filters

**Department** defaults to Professional Services. **Phase**, **Lead** and **Billing** narrow the list further. **Scope** has three settings:

* **Open** (the default) — every open project, no time window involved.
* **Completed** — only projects that have finished, inside a time window you choose.
* **All** — open projects, plus completed ones inside that same window.

Choosing **Completed** or **All** shows a **Completed within** window — last 30 days, last month, last quarter, last year, or a date range you type in yourself — right there in the filter bar. Because completed projects are kept in the Hub for good (see "Where the data comes from," above), a wide window genuinely shows everything that finished in it, not just whatever happened to still be around.

The window is measured on the date a project **actually completed**, not the finish date that was planned for it. At ANS those two are rarely the same — on about half of completed projects they're more than a month apart, and often much more — so the distinction decides the answer rather than trimming it. Counting last quarter by the planned date reported 24 completions where 106 projects had actually been completed. The **Finish** column still shows the planned date, because that's the question it's answering; the Completed window is the one that asks when the work really finished.

If you pick a window that reaches further back than the Hub's data actually goes, the summary line above the grid says so plainly — "completed projects are only kept back to [date]" — rather than quietly showing zero results and letting you think nothing finished that far back. An empty result and an out-of-range result are different findings, and this is meant to keep you from mistaking one for the other, especially when counting completions year over year.

**Billing** filters to Fixed fee, T&M, or **Not tagged** — see the Billing column above for what determines each. **Not tagged** deliberately lists only projects on a real project contract that has no billing suffix in its name; a project sitting on a client-level agreement isn't missing a tag and doesn't belong on a cleanup list, so it isn't included. **Search** matches client, project name or project number.

Every other filter — Department, Phase, Lead, Scope, the completed window and its dates, Billing, and whichever column you last sorted by — is remembered for you individually the next time you open Project Portfolio on this machine. If something you had selected no longer exists in the data (a department or a lead that's gone away, say), that one filter quietly resets to its default rather than leaving you staring at an empty grid with no idea why. **Search is the one filter that is deliberately not remembered** — starting with a clean search box every time means an old search term can never silently hide rows you didn't mean to filter out.

## The Board view

The **Grid | Board** switch in the header shows the same open projects as cards in six phase columns — Discovery, Planning, Executing, Monitoring & Controlling, Closure, On Hold — plus an **Unmapped** column when a project carries a status the map does not know. The Department, Lead and Search filters apply to both views; the Board never shows completed projects. Each card shows the client and project, the finish date (red when past), the newest status entry, the lead and % complete, and the same **Autotask status** dropdown as the panel: pick a new status, confirm, and the card moves to the column that status maps to. Cards cannot be dragged — several statuses share a phase, so a drag could not tell which status you meant.

## The detail panel

Click a row or a card. The panel shows the overview (dates, hours against estimate, duration), the account team, labels, the **full status history**, the scope of work, and the most recent writes made through the Hub. **Open in Autotask** jumps to the project in the Autotask web app.

Scrolling down to read something, then expanding a status entry or clicking "Show full scope," no longer bounces you back up to the top of the panel — your place holds. Opening a *different* project does start you back at the top, since that's a new project to read from the beginning.

Beside **Open in Autotask** is **Open project folder**, which tries to open the matching folder in SharePoint. Be aware this is the exception rather than the rule right now: a check of the real SharePoint library found a folder matching the project number for only about 1 in 10 open projects. For the rest, the button opens that client's whole **Projects** folder instead, and a line under the panel says so along with noting that no subfolder matches this project's number yet. If it can't tell which client's folder to look in at all, it says that plainly and opens nothing — it will never guess and open a different client's folder by mistake. If more than one folder looks like a match, it also opens nothing rather than picking one for you.

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

A status or a note shown on a row was read back from Autotask before the row was recorded. Labels and the StepUP IT flag are Hub-owned, so there is no Autotask value to read back — those rows are simply what the Hub stored. A status row also shows what the app believed the previous value to be, labelled **was reported as** — Autotask keeps no history of previous values, so that half was never verified and is shown as a claim rather than as fact.

## Settings (admins)

Admins see a **Settings** button in the header. It lists every Autotask project status — including inactive ones and any the board has not seen yet — with the phase it lands in, whether it counts as **Generic** (plain New or In Progress, which raises **Needs a status**), and how many cached projects carry it. A status can also be **Excluded** (never on the board) or left **Not mapped**. Below the table are the job titles that identify a client's **Technology Strategist** and **TAM** on the Autotask account team; a person is shown when their title contains one of them. **Save and re-sync** stores the settings and runs Sync Now so every project's phase follows the new map. Client Touch Aging keeps its own copy of the title lists. If a sync is already running when you save, the header says so and you run Sync Now yourself once it finishes.

Every save is recorded. The bottom of the Settings screen lists the newest five changes — when, who, and a one-line summary of what actually changed (a status that moved phase, a status that became Excluded or Not mapped, a job title added or removed). If a save cannot be recorded, the Hub says so rather than staying quiet; the settings themselves are still saved.

## Archive status history (admins, once)

An admin can run **Archive status history** once. It copies every **open** project's existing status text into a Project Status note titled "Status history before Anchor Hub", so pre-Hub history is preserved in the notes before the tool ever rebuilds the field. Running it again creates nothing new: before recording anything, the Hub re-reads the note from Autotask and checks it really is that project's own archive note.

It was run on 2026-09-17 for open projects only (21 notes). Completed projects were left alone — the Hub never rewrites their status field, so nothing there is at risk.

## Every write is recorded

Each status change, status update, scope-of-work edit and label or StepUP IT flag change is written to a Hub audit table with who made it and what it changed to. A previous value is stored alongside where the Hub had one, but only as reported — see **Recent writes from the Hub** above. It is separate from the Hub's general activity log because client project narrative should not be readable by every signed-in employee.

## Known open items

* The Board's Department, Lead and Search filters are shared with the Grid; the Grid's Phase and Scope filters do not apply to the Board.
* Writing to Autotask needs your personal Autotask API key. Without one, the dropdowns and the status/scope buttons are disabled and the panel says why; Labels can still be edited.
* If Autotask rejects the Hub's record of a write (someone changed the project in the seconds between the write and the read-back), the Hub offers one retry; if that fails too, the value is correct in Autotask and Sync Now refreshes the row. The same offer now appears when a status update reached Autotask as a note but the rest of the write did not — the error dialog gains a **Record this write** button.
* **Open project folder** finds a project-specific subfolder for only about 1 in 10 open projects today — the rest of the time it opens the client's Projects folder instead, which is still a useful shortcut but not the exact folder. About 40 clients also don't yet have their Autotask name matched to their SharePoint folder name; until that list is filled in, those clients get a "could not identify the client folder" message instead of even the Projects folder. Both should improve over time as that folder-name matching is filled in — they're a known gap, not a bug to report.

---

*Imagined by Andi Gingerich. Project Portfolio replaced the Planner board she kept by hand for the Professional Services team, and this round of improvements came directly from her own feedback after using the tool day to day.*
