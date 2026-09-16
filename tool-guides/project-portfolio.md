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

Two things are deliberately **not** editable here yet: the Autotask status (a later release adds changing it from this screen — until then, change it in Autotask and click Sync Now) and the scope of work.

## The grid

One row per project, sorted by finish date. Click any column header to sort by it; click again to reverse.

* **Name** — client and project. Pinned on the left so it stays visible as you scroll sideways.
* **Lead** — the Autotask project lead.
* **Status** — the newest dated entry in the project's status log (the `statusDetail` field in Autotask), with a note of how many earlier entries exist. "No status yet" means nobody has written one.
* **Phase** — derived from the Autotask status: Discovery → Planning → Executing → Monitoring & Controlling → Closure, plus On Hold. **Needs a status** in amber means the project is sitting on plain "New" or "In Progress", which tells the board nothing about where it really is — pick a specific status in Autotask (IKO Scheduled, Go-Live Targeted, Schedule CKO, and so on) and it moves to the right phase on the next sync. An **Unmapped** phase means Autotask has a status this tool has not been told about; ask an admin.
* **Start / Finish** — from Autotask. A finish date in the past on an open project shows in red.
* **% Complete** — computed by Autotask from task completion. It cannot be edited anywhere in the Hub.
* **Project #**, **Technology Strategist**, **Account Manager**, **TAM** — a dash means no active ANS person holds that role on the account. People who have left ANS are never shown, even if Autotask still lists them on the team.
* **Labels** — Hub-only tags and the StepUP IT flag. Editing them arrives with the Board view.

## Filters

**Department** defaults to Professional Services. **Phase** and **Lead** narrow the list. **Scope** is Open by default; **Include completed** adds projects finished within the chosen window (last 30 days, last month, last quarter, last year, or a date range). **Search** matches client, project name or project number.

## The detail panel

Click a row. The panel shows the overview (dates, hours against estimate, duration), the account team, labels, the **full status history** with the newest entry first, and the scope of work. **Open in Autotask** jumps to the project in the Autotask web app.

The line under the history — "Autotask's statusDetail field holds the newest N entries · X / 2,000 characters" — is there because Autotask caps that field at 2,000 characters, and several live projects are within a couple of updates of the cap. A later release moves the running log into Autotask project notes, which have no such limit.

## Known open items

* Status changes and new status entries are read-only in this release (Phase 2).
* The Board view and label editing are not built yet (Phase 3).
* The phase map and the job titles that count as "Technology Strategist" / "TAM" are admin-editable in configuration but have no settings screen yet.
