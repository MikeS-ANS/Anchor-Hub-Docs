# Daily Progress

> **This tool exists only in an unreleased development branch (Phase 1 of 3).** It is not in
> the released app you're using today — nothing on this page describes anything you can open
> right now unless someone has specifically told you they're testing the new build with you.
> Once it ships, it will also need a grant from **Access Management** before it shows up in
> anyone's sidebar (see "Getting access" below) — being on a role that normally gets everything
> is not enough on its own. This page describes the tool as Phase 1 actually built it.

Daily Progress is a personal report of your own day at ANS: what you actually did, not what you
were assigned. It reads your own sent email, your own calendar, and your own Autotask activity
for one day at a time, and turns them into four headline numbers plus a timeline of everything
that moved forward — sorted earliest to latest. Nothing about it is shared. There is no manager
view, no team rollup, and no admin view of anyone else's day, including yours.

Sidebar tool key: `daily-progress`.

---

## What it shows

Across the top, four tiles:

- **Actions taken** — sent-mail threads plus Autotask tickets you touched that day, added
  together, with the split shown underneath (e.g. "3 email threads · 5 tickets").
- **Clients touched** — the number of distinct client companies your email or ticket work
  touched that day, with up to five names listed underneath (and a "+N more" if there were more
  than that). Vendor companies are counted and shown separately in that same line, never folded
  into the client count.
- **Meetings** — how many of the day's counted meetings are already past, out of the day's
  total, with how many are past vs. still upcoming and the combined scheduled hours across all
  of that day's counted meetings (past and upcoming together) underneath.
- **Hours logged** — your total Autotask hours for the day, across however many tickets, with
  how many of those were closed that day.

Below the tiles, **"What actually moved forward"** is a single timeline combining every sent-mail
thread, every ticket you touched, and every meeting that's already happened that day — earliest
first. A meeting that hasn't happened yet does not appear in this timeline; it only shows in the
separate Meetings section below it. Under the timeline, when there's real activity to describe,
one computed sentence summarizes the day (e.g. "7 actions across 3 clients; 2 of them closed
tickets") — this line is arithmetic on the numbers already shown above it, marked "not AI,"
never a generated summary.

A separate **Meetings** card lists the day's whole calendar — past and upcoming, counted and not
— so you can see what's ahead as well as what's done.

## What counts

**A meeting counts** if it has at least one other attendee, you didn't decline it, it isn't
marked as an all-day event, and you weren't marked "Free" for it on your calendar. An event with
no other attendees (a personal block on your own calendar) is still listed in the Meetings card,
but it's never counted and never shown as a meeting elsewhere. A counted meeting is marked
**past** once its end time has already passed as of when the report was generated — never
"attended," since Daily Progress has no way to know whether you were actually in it. A private
event shows as "Private" in place of its real subject, but otherwise counts the same as any other
meeting.

**Actions taken**'s ticket half, and **Hours logged**'s ticket count, both mean the same thing:
the number of distinct Autotask tickets you touched that day. A ticket counts as touched if you
logged time against it, if it was closed that day, or if you wrote a note on it — but only a
note a person actually typed. Automated notes from RMM tools, workflow rules, integrations, or
any other system process don't count as touching a ticket. Time logged against a general project
task (no specific ticket) still adds to your hours and appears in the timeline as its own
"Project" entry, but it isn't one of the tickets counted in that number.

## Where it's saved

Every report is written to your own OneDrive, not to a shared Hub database — nothing about your
day is ever stored anywhere Mike, an admin, or anyone else can query. It lives in your OneDrive
under **Apps/Anchor Hub**, in a folder Microsoft creates specifically for this app. Inside it:

- `index.json` — one row per day you've ever generated a report for, holding the tile numbers
  plus a few status flags (the client ids, whether the run was partial or empty, and when it was
  generated), so the app can list your history without opening every day's file.
- `<year>/<date>.json` — the actual report for that day, and the source of truth the app reads
  back when you reopen it.
- `<year>/<date>.html` — a standalone, human-readable copy of the same report, viewable on its
  own outside the Hub.

Nothing is ever deleted by the app — retention is whatever your organization's OneDrive/Microsoft
365 storage and retention policy already allows, not anything Daily Progress manages itself. The
**Open in OneDrive** button in the header takes you straight to this folder in your browser, so
it's yours to open, move, or delete any time you want.

## Privacy

Daily Progress reads three things, and only for the signed-in user running it, one day at a
time: your **Sent Items** (not your inbox — just what you sent), your **calendar**, and your own
**Autotask** time entries, ticket notes, and closed tickets. It does not read your inbox, Teams,
or anyone else's mail, calendar, or tickets. The first time you open the tool, before it ever
generates a report, a one-time screen spells this out plainly — what it reads, what it doesn't,
where it's stored, what (if anything) leaves Microsoft, and who can see it — and nothing runs
until you acknowledge it. As of this Phase 1 build, nothing leaves Microsoft or Autotask at all;
a later phase will summarize the emails you sent into one line each via Hatz.ai, ANS's AI vendor,
without storing the underlying text — only the summary line.

## Getting access

Once this ships, getting to Daily Progress takes three things: an Access Management admin has to
grant the `daily-progress` tool to your role (or to you specifically), you then have to turn it
on for yourself in **Settings → Customize** the same way you would any other tool (tools default
to hidden on a fresh install), and — because this tool needs a new Microsoft permission
(`Mail.Read`) that wasn't part of the Hub before — you may need to sign out and sign back in
once after the update lands, so that permission is actually on your session (the app tells you
if it does).

## History and past days

The **◀** and **▶** arrows step one day at a time, and the date picker next to them jumps
straight to any day up through today (you can't pick a future date). If a day you navigate to
was never generated, you'll see a **"No report saved for this day"** message with a **Generate
it** button — Sent Items, calendar, and Autotask still have that day's data, so generating it
later saves it to your OneDrive exactly like any other day. Today's report generates itself
automatically the first time you open the tool that day; past days only generate when you ask.

## When something is unavailable

Each source — Sent Items, calendar, Autotask — is read independently, so one of them being
temporarily unavailable doesn't stop the other two. When that happens, a banner across the top
names the source that failed. **Meetings** and **Hours logged** each depend on exactly one
source (calendar and Autotask respectively), so if that source fails, the tile shows **"—"**
instead of a number, with a note underneath saying that source is unavailable rather than the
day genuinely being empty. **Actions taken** and **Clients touched** each depend on *two*
sources (mail and Autotask); if only one of the two fails, they still show a real number built
from whichever source succeeded, with a note like "email unavailable" appended — they only drop
to "—" if both fail at once. The footer at the bottom of every report also shows a ✓ or ✗ per
source for exactly this reason. **Refresh** (today) or
**Regenerate** (a past day) tries again. Saving to OneDrive is a separate step from generating
the report — if the report itself generates fine but the save to OneDrive fails, you're told
explicitly with an on-screen alert, and the report stays visible so you can try Refresh again
rather than losing it.

## Coming next

Phase 1 is today's report, history and backfill, and this privacy screen — everything above is
what's actually shipped. Not yet in this release:

- **AI one-liners with guardrails** — a one-sentence Hatz.ai summary per email thread, checked
  against the real numbers before it's ever shown.
- **Email me** — an on-demand or scheduled copy of your report sent to your own inbox.
- **End-of-day auto-generate** — the day's report generating itself on a schedule instead of
  only on first open.
- **This week** — a rolled-up view across the current week, not just one day at a time.
- **Baselines** — comparing a day or week against your own recent average, with sparklines.
