# Payroll Processing

> **Not yet released.** Built and in testing — this page describes the tool exactly as it works today, and is updated as the remaining pieces land. Access is restricted — see [Access](#access) below.

Payroll Processing replaces the manually-maintained **"2026 Leadership Approved Payroll.xlsx"** spreadsheet — the one with a separate tab for every pay period, where each department manager's changes were collected by hand before payroll was sent off. Managers now enter their own people's per-period changes directly in the Hub, sign off once per period, and Heather and Mike review every department in a single screen before sending the period on to Puzzle.

It sits **upstream of [Payroll Review](payroll-review.md)** in the payroll pipeline: Payroll Processing is where the changes for a period are collected, approved, and sent to Puzzle to kick off the actual payroll run; Payroll Review is where the resulting pre-process reports get checked before Heather submits. Two different jobs, two different tools, same sensitivity level — this one holds per-employee pay changes, reimbursements, and bonuses for every ANS employee.

---

## Access

Payroll Processing does **not** use the Hub's general Role Matrix. Like Payroll Review, it's gated end to end — every read, not just writes — by dedicated Azure AD roles, and `hub.admin` does not grant access. There are two roles, and they see genuinely different tools:

| Role | Who | What they see |
|---|---|---|
| `hub.payroll` | Mike, Heather | Everything: My People, Departments, Autotask Match, and the cross-department Review & Send screen |
| `hub.payrollmanager` | A department manager | **My People only** — their own employees, for the open period, and nothing else |

A manager's scope isn't a UI filter. The server works out which employees belong to whoever is asking (from their department assignment, plus any per-employee overrides Heather and Mike have set) and simply never returns anybody else's row — so a manager cannot see, edit, or sign off on another department's people even by asking for them directly.

> **`hub.payrollmanager` now exists in Entra and is assigned.** Department managers can open the tool and work their own people. One piece is still missing on their side: there's no **Sync expenses & mileage** button on My People yet, so a manager can't trigger a fresh Autotask pull themselves. It doesn't hold anything up — the sync runs on its own every morning, and Heather or Mike can run it on demand — but a manager who's just been told "your mileage is in Autotask now" can't pull it in on the spot.

---

## My People — what a manager does each period

Open **Payroll Processing** and the **My People** tab shows the currently open pay period, its dates, its pay date, and every employee you're responsible for. Each employee is one row:

| Column | Who fills it in |
|---|---|
| **Mileage** | **Synced from Autotask — read-only** |
| **Expenses** | **Synced from Autotask — read-only** |
| On Call | You |
| Performance Bonus | You |
| CrewHu Rewards | You |
| Commission | You |
| Regular Hours | You |
| Notes | You |

Edits save as you go — there's no Save button to forget. When you're finished, tick **"I've completed my updates for this period"** at the bottom. That's one checkbox for you, not one per employee: it records that your whole department is done, with your name and the time, and it's what Heather and Mike watch to know whether they're still waiting on you. You can untick it and keep editing if something changes.

If the period's deadline passes and your department hasn't signed off, a past-due banner appears at the top of your own screen.

The Hub creates each upcoming pay period on its own, so there's nothing to set up at the start of a period — when a new period opens, it's simply there.

---

## Why Mileage and Expenses are read-only

This is the question most likely to come up, so it's worth being explicit: **nobody can type a mileage or expense number into the Hub.** Not a manager, not Heather, not Mike, not on the review screen either. Those two columns are always read-only, everywhere in the tool, by design.

They come from expense reports **already approved in Autotask**. Each morning the Hub reads the approved expense reports for everyone on the payroll roster and works out two figures per employee for the open period:

- **Mileage** — from expense items with mileage on them.
- **Expenses** — from the remaining items that are actually reimbursable to the employee.

The amount used is what the employee is genuinely owed for that item, which is why a charge put on a company credit card correctly contributes nothing — it was never money out of their pocket.

**Autotask is the single source of truth, and there is deliberately no override inside the Hub.** If a number looks wrong, the fix is always the same:

1. Correct it in Autotask — the expense item, its mileage, its reimbursable flag, or the approval itself.
2. Re-sync (or wait for the next morning's run).
3. The Hub picks up the corrected figure.

The reason for the hard line is simple. If a mileage figure could be typed over in the Hub, the Hub and Autotask would disagree, and the number that reached payroll would be the one nobody could trace back to an approved expense report. Every dollar in those two columns can be pointed at a specific approved report in Autotask, and that's worth more than the convenience of an in-app edit.

An expense item is also only ever counted into **one** pay period. Re-running the sync recalculates the open period from scratch rather than adding to what's already there, and an item that was already counted in an earlier period is skipped — so re-syncing, or a later correction on a report that's been part-paid already, can't quietly pay somebody twice.

### "not synced" is not the same as $0.00

A blank, italic **not synced** in the Mileage or Expenses column means the Hub hasn't been able to match that person to their Autotask account yet — not that they had nothing to claim. A real **$0.00** means the sync ran for them and found nothing owed.

Matching is by name, and names don't always agree between systems: a roster "Judith" who is "Judy" in Autotask, a middle name on one side only, an accent that doesn't survive the trip. Rather than guess — a wrong guess here pays the wrong person's mileage — the Hub leaves those unmatched and Heather or Mike finish them by hand once, on the **Autotask Match** tab, after which the match sticks permanently. If one of your people shows "not synced" for a whole period, that's the thing to mention to Heather.

---

## The sync — when it runs, and what it can see

The sync runs **on its own, in Azure, early each morning** (around 6:15 a.m. Denver time in summer, an hour earlier by the clock in winter). Nobody needs to have the Hub open for it to happen, and it doesn't matter who's signed in.

Heather and Mike can also run it on demand — **Sync expenses & mileage**, on both the Review & Send screen and the Autotask Match tab — which is the right move after correcting something in Autotask rather than waiting for the morning.

> The permission for a **manager** to re-sync their own people is built and live on the server, but the button for it isn't on the My People screen yet. For now, ask Heather or Mike to run a sync, or wait for the next morning's run.

**The 180-day window.** Each run looks back about six months in Autotask. In practice that covers everything anyone submits on a normal timeline, with a lot of room to spare. What it doesn't cover is a genuinely old submission — an expense report approved well over six months after the fact. Those aren't silently dropped: they surface on the review screen as needing attention (below) rather than being paid automatically off a partial reading.

---

## Review & Send — the cross-department screen

Heather and Mike get a **Review & Send** tab: every department's employees for a chosen period in one grid, grouped by the manager who actually owns them, with the counts across the top — employees, sign-offs collected, flagged entries, total changes, and expense reports read.

- **Sign-off cards** show each manager, the department (or departments) they actually own, how many employees that comes to, and whether they've signed off yet — with the time they did it. The cards are per *manager*, not per department: if a few people in a department have been reassigned to somebody else, that second manager gets their own card and their own sign-off, and the grid below groups the same way. A department with two managers in it therefore appears twice, once per manager, which is the honest picture — they sign off separately because they're responsible for different people.
- **Flags** mark anything unusual on an individual value — hover a flag for the detail.
- **Any editable cell can be corrected here**, and the edit is logged against the name of whoever made it. Mileage and Expenses stay read-only on this screen too, for everyone.
- A warning appears if any employee has **no manager assigned** — they're still listed, but nobody can sign them off.

### Signing off for a manager who's away

A manager on PTO used to block the entire period. Nobody else could tick their box, so the period could never be sent, and the only way round it was a database fix.

Heather and Mike can now do it for them. Each sign-off card carries a **Sign off for them** button — or **Reopen**, on one that's already signed. Either way you're asked for a reason. It's optional, deliberately, because the moment you need this is usually the moment you're unblocking a deadline and don't want the friction — but it's worth typing one anyway (*"Chris on PTO, cover confirmed by Jacee"*), because the reason is the first thing anyone asks months later.

What gets recorded is deliberately not a forgery. The sign-off is stored as **that manager's**, performed **by you**, and the card says so underneath — *"by Mike Stewart, on their behalf"* — along with whatever reason you gave. The audit log records it as a delegated action, distinct from an ordinary sign-off, so it stays obvious after the fact who actually did what.

**Reopen** works the same way, and exists for the mirror-image problem: a sign-off made too early, or made by mistake, or overtaken by a late correction. Reopening one puts the period back to waiting on that manager, and the reason you give is kept.

> The manager isn't notified yet that somebody signed off for them — that arrives with the Teams reminders. For now, tell them.

### Approve & Send to Puzzle

When a period is ready, **Approve & Send** on the Review & Send screen opens a preview of exactly what's about to happen — nothing is sent, and nothing changes, until you press **Send to Puzzle** inside it.

The preview shows:

- The email itself — **To**, **Cc**, **Subject**, and body, all editable. **The first time anyone sends, To and Cc come up empty** — the Hub doesn't ship with Puzzle's address built in, on purpose: sending real payroll to an address nobody's actually confirmed is worse than the one minute it takes to type it in. Type it in and press Send, and from then on those recipients are remembered and pre-fill automatically every period after — a one-time step, not a recurring one. (Subject and the body always start fresh with wording written for that specific period, which you can still edit same as everything else.)
- The attached file — a generated Excel workbook carrying the same ten columns you see on the review screen above, in the same order, grouped the same way. It lists **only the people who have something changed this period**, not the whole roster, so a quiet period produces a short file. The columns ANS stopped using (New Hire/Term, Insurance Eligible, 401K, Overtime Hours, Pay Increase, Taxable Reimburse) are gone from it entirely — they had been sitting blank for a long time and Puzzle has no use for them.
  - One thing that occasionally surprises people: somebody who has **only a note** and no amounts still appears in the file. A note is usually there for a reason, and leaving a person out of a payroll submission is a far more expensive mistake than including a sparse line. A row where every figure is genuinely empty or zero is left out.
- **The expense reports that need a manual flip to Paid in Autotask afterward** — every employee, amount, and week ending, with a plain explanation of why (below).
- A warning if any manager still hasn't signed off. You can send anyway — the copy says so — and the send is recorded against your own name either way.

Pressing **Send to Puzzle** does four things, in order, and the first one is the one that can't be undone:

1. **The email goes to Puzzle.** This is the irreversible step — once it's sent, it's sent to a real vendor, the same as sending any other email.
2. **The period is marked sent and closes to edits.** Nobody — not a manager, not Heather, not Mike — can change an entry in it any more. The grid still shows every value, just read-only, with a banner naming who sent it and when.
3. **A second, separate email goes out internally** — never to Puzzle — listing exactly which expense reports need to be flipped to Paid in Autotask, and why the Hub can't do it for you (below). If this one fails to send for some reason, you're told plainly, and the standing panel described next still has the same list.
4. If more than 200 expense reports are ever waiting on a flip at once, Send is blocked instead, with a message asking you to work through the backlog first — that's meant as a safety net against a data problem quietly turning into an unworkable to-do list, not something that should come up in normal use.

Clicking Send a second time on an already-sent period is refused outright — nothing sends twice.

#### Why the Hub can't mark those expense reports Paid itself

This is worth explaining plainly, because it's easy to assume it's a bug: **it's tested, confirmed, and permanent.** Autotask's connection lets the Hub read an expense report's status, but not change it — a request to update it comes back looking successful and silently does nothing. That's Autotask's own design, not something ANS or the Hub can turn on.

So instead, every time a period is sent, Approve & Send tells you exactly which expense reports were paid through this period and now need to be flipped to **Paid** in Autotask by hand — the normal place you'd do that anyway. Two things make sure that list doesn't get lost:

- **The internal email**, sent the moment you press Send (item 3 above).
- **A standing panel** on the Review & Send screen, always visible, listing every report still waiting — not just from the period you're looking at, but any from earlier periods too, so nothing quietly ages out. Each row shows how long it's been waiting. There's a **Copy list** button for pasting the ids into Autotask.

That panel doesn't have a "dismiss" button on purpose. The only thing that clears a row is the report actually reaching **Paid** in Autotask — the Hub checks every morning, and the moment it sees a report you (or anyone) flipped, it disappears from the list on its own. If the panel is empty, everything really is caught up.

#### Reopening a sent period

A **Reopen period** button appears once a period has been sent, for Mike and Heather only. It's for the case a sent period needs to change — a mistake found afterward, a late correction. Reopening it puts the period back to open and editable.

**Reopening does not undo the parts that already happened for real:** it does not un-send the email to Puzzle, and it does not un-flip any expense report someone has already marked Paid in Autotask, or reset how long the remaining ones have been waiting. It only reopens the Hub's own record so the period can be edited and sent again.

---

## "N expense reports need attention"

When this warning appears, it means the Hub found approved expense reports with reimbursable items on them that were **not** counted into any period — most often a late submission from outside the six-month window. Read it as: *someone may still be owed money and hasn't been paid.*

These are never settled automatically, on purpose. The Hub won't quietly treat a report it couldn't fully read as though it were paid off — this is a genuinely different situation from the "waiting to be flipped" list above, which only ever holds reports the Hub is confident were actually paid through a period. A "needs attention" report might never have been paid through payroll at all.

The warning expands into the actual list — report, employee, amount, and how long it's been sitting — so there's something to act on, not just a count. For each one:

- **Look it up in Autotask** and confirm what's actually owed, the same as always.
- If the report's status changes in Autotask on its own — approved into a payable state some other way, rejected, whatever the real resolution turns out to be — the Hub notices the next morning and clears the warning by itself. Nothing to do.
- If it's genuinely handled some other way (reimbursed directly, confirmed as a duplicate, whatever it turns out to be) and Autotask's own status won't reflect that, click **Mark as handled** and type a short reason. This retires the warning — it does **not** pay anyone and it writes nothing to Autotask; it's purely the Hub's own record that a human looked at it and knows why it's being left alone. The reason is required, and it's worth being specific (*"confirmed reimbursed directly, not through payroll"*), since this is the kind of thing that gets asked about months later with no other record of why.

The warning only appears when the count is above zero; a period with nothing outstanding shows nothing at all, so if the line is there, it's worth a look.

---

## Known open items

- **No settings screen yet for the internal reminder recipient.** Puzzle's own To/Cc are remembered automatically after the first successful send (see above) — but if nobody's configured a specific person or distribution list to receive the internal "reports to flip" email, it defaults to whoever pressed Send. Never lost, just not yet routed anywhere fixed.
- **A manager can't trigger their own sync from the UI yet** — the server-side permission is in place, the button isn't. The daily automatic sync covers it; this is a convenience gap, not a blocker.
- **Nobody is notified when a sign-off is made on their behalf, or when a period is waiting on them at all, or when an expense report has been sitting on the "waiting to be flipped" list for a while.** Teams reminders are the last piece of the tool, covering all three.
- **A department row added by mistake can't be removed from the UI.** The suggested rows on the Departments tab now come from the real roster, so they no longer offer a department that doesn't exist — but if a wrong one does get saved, ask for it to be cleaned up rather than leaving a department that matches nobody.
