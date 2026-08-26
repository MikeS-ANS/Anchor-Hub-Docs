# Payroll Processing

> **Not yet released.** Built and in testing — this page describes the tool exactly as it works today, and is updated as the remaining pieces land. Access is restricted — see [Access](#access) below.

Payroll Processing replaces the manually-maintained **"2026 Leadership Approved Payroll.xlsx"** spreadsheet — the one with a separate tab for every pay period, where each department manager's changes were collected by hand before payroll was sent off. Managers now enter their own people's per-period changes directly in the Hub, sign off once per period, and Heather and Mike review every department in a single screen instead of chasing tabs.

It sits **upstream of [Payroll Review](payroll-review.md)** in the payroll pipeline: Payroll Processing is where the changes for a period are collected and approved before payroll runs; Payroll Review is where the resulting pre-process reports get checked before Heather submits. Two different jobs, two different tools, same sensitivity level — this one holds per-employee pay changes, reimbursements, and bonuses for every ANS employee.

---

## Access

Payroll Processing does **not** use the Hub's general Role Matrix. Like Payroll Review, it's gated end to end — every read, not just writes — by dedicated Azure AD roles, and `hub.admin` does not grant access. There are two roles, and they see genuinely different tools:

| Role | Who | What they see |
|---|---|---|
| `hub.payroll` | Mike, Heather | Everything: My People, Departments, Autotask Match, and the cross-department Review & Send screen |
| `hub.payrollmanager` | A department manager | **My People only** — their own employees, for the open period, and nothing else |

A manager's scope isn't a UI filter. The server works out which employees belong to whoever is asking (from their department assignment, plus any per-employee overrides Heather and Mike have set) and simply never returns anybody else's row — so a manager cannot see, edit, or sign off on another department's people even by asking for them directly.

> **`hub.payrollmanager` does not exist in Entra yet.** The code is built and the server-side permission checks are live, but until that role is created and assigned to the department managers, no manager can open the tool at all — they'll get the standard "you don't have access to this tool" screen. That's the one outstanding step between the built tool and managers actually using it.

---

## My People — what a manager does each period

Open **Payroll Processing** and the **My People** tab shows the currently open pay period, its dates, its pay date, and every employee you're responsible for. Each employee is one row:

| Column | Who fills it in |
|---|---|
| On call | You |
| Perf bonus | You |
| **Mileage** | **Synced from Autotask — read-only** |
| **Expenses** | **Synced from Autotask — read-only** |
| Commission | You |
| Reg hrs | You |
| Rewards crew | You |
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

Heather and Mike get a **Review & Send** tab: every department's employees for a chosen period in one grid, grouped by department, with the counts across the top — employees, sign-offs collected, flagged entries, total changes, and expense reports read.

- **Sign-off cards** show each department, its manager, how many employees they own, and whether they've signed off yet (with the time they did).
- **Flags** mark anything unusual on an individual value — hover a flag for the detail.
- **Any editable cell can be corrected here**, and the edit is logged against the name of whoever made it. Mileage and Expenses stay read-only on this screen too, for everyone.
- A warning appears if any employee has **no manager assigned** — they're still listed, but nobody can sign them off.

**"N expense reports need attention."** When this warning appears, it means the Hub found approved expense reports with reimbursable items on them that were **not** counted into this period — most often a late submission from outside the six-month window. Read it as: *someone may still be owed money and hasn't been paid.*

These are never settled automatically, on purpose. The Hub won't quietly treat a report it couldn't fully read as though it were paid off. Follow it up in Autotask — find the report, confirm what the employee is actually owed, and handle it deliberately. The warning only appears when the count is above zero; a period with nothing outstanding shows nothing at all, so if the line is there, it's worth a look.

---

## Known open items

- **`hub.payrollmanager` hasn't been created in Entra yet**, so no department manager can open the tool. Mike and Heather can use it today; managers can't, until the role exists and is assigned.
- **A manager can't trigger their own sync from the UI yet** — the server-side permission is in place, the button isn't.
- **Approve & Send to Puzzle is visible but disabled.** Sending the consolidated period to Puzzle, and marking the matching expense reports settled in Autotask, is the next piece of work and isn't built.
- **The Departments tab's suggested department rows include an example that doesn't exist in real data**, and a department row added by mistake can't be removed from the UI — ask for it to be cleaned up rather than leaving a department that matches nobody.
