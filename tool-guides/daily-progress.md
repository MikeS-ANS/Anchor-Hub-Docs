# Daily Progress

> **This tool exists only in an unreleased development branch (Phases 1–2 of 3).** It is not in
> the released app you're using today — nothing on this page describes anything you can open
> right now unless someone has specifically told you they're testing the new build with you.
> Once it ships, it will also need a grant from **Access Management** before it shows up in
> anyone's sidebar (see "Getting access" below) — being on a role that normally gets everything
> is not enough on its own. This page describes the tool as Phases 1 and 2 actually built it.

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
never a generated summary. Each sent-email thread in the timeline can also carry one AI-written
line beneath its subject, marked with a small **AI** chip and the words "written by Hatz.ai" —
see [AI one-liners](#ai-one-liners) below. Everything else on this page — the four tiles, the
computed callout, the ticket and meeting entries — is measured, never generated.

A separate **Meetings** card lists the day's whole calendar — past and upcoming, counted and not
— so you can see what's ahead as well as what's done.

## AI one-liners

Every sent-email thread in the timeline can carry one AI-written line underneath its subject —
one plain sentence describing what you actually did in that thread, written by Hatz.ai, ANS's AI
vendor. What's sent to Hatz.ai for each thread is deliberately narrow: the subject line, the
first names of the people you sent it to, and the text you personally wrote in up to three of
your own emails in that thread that day (newest first), each cut off at 1,500 characters. The
quoted reply chain underneath your own text is never sent, and nothing else about that day ever
reaches Hatz.ai — not your inbox, not anyone else's message, not your calendar, not anything from
Autotask. One report means one call to Hatz.ai covering every thread from that day at once, never
one call per thread.

Every line that comes back is checked before you ever see it: for a dollar figure or amount of
money in any currency, as digits or spelled out; for salary, bonus, payroll, or other
compensation language; and for an email address, link, phone number, long run of digits, or a
credential. A line that fails any of those checks is withheld, and so is a line Hatz.ai simply
couldn't produce — missing, empty, or too long. A withheld thread shows a dashed **AI** chip with
the exact reason instead of a summary line:

- *"No AI line for this thread — Hatz.ai returned nothing usable. Subject shown instead."*
- *"Line withheld by guardrail — it named an amount. Subject shown instead."*
- *"Line withheld by guardrail — it named a pay or compensation detail. Subject shown instead."*
- *"Line withheld by guardrail — it contained an address, number, link or credential. Subject
  shown instead."*

If Hatz.ai is unavailable for the whole run, every email entry in the timeline just shows its
subject line the way Phase 1 always did, and the report footer adds *"Hatz.ai unavailable — email
entries show their subject."* If Hatz.ai answered but one or more individual lines were withheld
by a guardrail, the footer instead adds *"N lines withheld by guardrail."* Either way, the
footer's source list ends with **AI ✓** (or, in red, **AI ✗** when the step didn't run this time)
alongside the same ✓/✗ shown for Sent Items, Calendar, and Autotask, so you can tell at a glance
whether the lines you're reading came from this run. A report saved before this update also shows
**AI ✗** even though nothing actually failed — a Phase 1 report simply has no AI data at all, and
that reads the same as "unavailable" until you Refresh or Regenerate that day.

The text sent to Hatz.ai is never stored anywhere, by this tool or by Hatz.ai — only the one
screened sentence that comes back is kept, saved as part of the report the same as everything
else.

## AI Prompt

The AI line is generated by **one call per report** to Hatz.ai's Anthropic-native passthrough
endpoint (`https://ai.hatz.ai/v1/anthropic/messages`, via the Hub's shared `hatzToolChat.js`
transport), with a 55-second timeout. The call forces Hatz.ai to answer through a tool
(`tool_choice`) rather than free text, with a plain-text-JSON reading of the same shape accepted
as a fallback in case a proxy in front of Hatz.ai ever drops that instruction. Model:
`anthropic.claude-haiku-4-5`.

**System prompt** (quoted verbatim from the shipped code):

> You write one-line summaries of email threads for the person who sent them. The lines appear in a private end-of-day report that only that person can see.
>
> For each thread you receive its subject, the first names of the recipients, and the text the sender wrote in up to three of their own emails from that day (newest first). Nothing else about the thread is available to you.
>
> Write exactly one sentence per thread, in this register:
> - Sent Wendy the contract copy she asked for — closes out her ask.
> - Recommended a Microsoft 365 SaaS backup for SharePoint and offered to connect Michael with the SoftChoice rep.
> - Asked Andrew for a timeline so the install does not stall.
>
> Rules:
> 1. Describe what the sender did. State an outcome only when the sender's own text makes it evident; never invent one.
> 2. Do not use the first person (no "I", "we", "my", "our"). Begin with a past-tense verb.
> 3. Never include a dollar figure, price, rate, or amount of money in any currency, as digits or as words.
> 4. Never include salary, bonus, raise, payroll, benefits or other compensation details.
> 5. Never include account numbers, phone numbers, email addresses, links, passwords, or any personal detail beyond the first names you were given.
> 6. If the text is too short or unclear to summarise, restate the subject line as a sentence.
> 7. One sentence, at most 30 words, plain text: no quotation marks, no markdown, no emoji.
>
> Submit every thread exactly once with the submit_lines tool, using each thread's key exactly as given.

The **user turn** is a single JSON object shaped `{ threads: [{ key, subject, to, sent }] }` —
one entry per email thread from that day, with a short local key (`t1`, `t2`, …) standing in for
the thread so nothing resembling a real Graph conversation id is ever sent. The tool Hatz.ai must
answer through, `submit_lines`, requires exactly one `{ key, line }` pair per thread key, with
`line` documented as one plain-text sentence of at most 30 words.

**Guardrails**, checked against every line before it's shown, in plain words: no money amount in
any currency, as digits or spelled out; no salary, bonus, payroll, or other pay/compensation
language; no email address, link, phone number, long run of digits, or credential; and no line
that's missing, empty, or too long. **On any failure** — Hatz.ai unreachable, a bad response, or a
line that doesn't pass a guardrail — the affected thread simply shows its subject line instead,
with a note explaining why; nothing else about the report changes, and the report still saves.

The AI step may also not run at all this time, for reasons that have nothing to do with the model
itself: Sent Items was unavailable this run, there were no email threads that day to summarise,
the privacy notice hasn't been acknowledged on this machine yet, or the signed-in account changed
partway through the run.

## Email this to me

The **Email me** button in the header sends the report currently on your screen to your own
mailbox — never anyone else's — with the report itself as the message body, and the same report
attached again as a standalone `<date>.html` file (e.g. `2026-09-02.html`) so you have a copy that
opens outside the Hub too. The subject line is always `Daily Progress — ` followed by the report's
date, written out like `Daily Progress — Wed, Sep 2, 2026`.

Click it and the button reads "Sending…" while the send is in flight. On success, a toast reading
"Emailed to `<your address>`" appears if you're still viewing Daily Progress when it lands, and the
button changes to "Emailed ✓" for that exact report — the same day and the same run of it; a
Refresh or Regenerate that produces a fresh version resets the button back to "Email me." Emailing
the same report a second time asks you to confirm first, naming when you last sent it, since a
repeat send is easy to trigger by accident.

If the send fails, you're told plainly: an inline message naming the report's date appears where
you're currently looking if you're still on the Daily Progress screen, or the same message pops up
as its own dialog if you've since navigated to a different tool — a failure is never silent
wherever you are. The only thing that suppresses the outcome entirely, success or failure, is
signing out (or someone else signing in) before the send finishes, since the result no longer
belongs to anyone's session by then.

The email is always sent from your own mailbox to your own mailbox, and — deliberately — it is
never saved to your Sent Items. A Sent Items copy would be read back by the very next report you
generate that same day as if it were a real email you'd sent, with its own subject line showing up
as an extra entry in that day's timeline; emailing yourself the report is never counted as one of
the day's actions. **Email me** always sends whatever is currently on screen, so Refresh first if
you want the latest version of today's report before sending it.

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
until you acknowledge it.

As of Phase 2, one thing does leave Microsoft: the text you wrote in the emails you sent — never
the quoted reply chain underneath it, and never anyone else's message — goes to Hatz.ai, ANS's AI
vendor, to be summarised into a single sentence per thread (see [AI one-liners](#ai-one-liners)
above for exactly what's sent). That text is never stored anywhere; only the one screened sentence
that comes back is kept. Nothing else leaves Microsoft or Autotask. The privacy notice is
versioned, and the app remembers which version you last acknowledged — because this is a real
change in what leaves your machine, not a wording tweak, anyone who had already acknowledged
Phase 1's notice sees the first-run screen again, once, the next time they open Daily Progress.
That re-shown screen is headed "What's changed before your next report," with the one new fact
called out in an amber banner above the same full notice, rather than quietly opting anyone in.
Acknowledging it again is all it takes to keep going, and nothing about what's read, where it's
stored, or who can see it has changed.

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
source for exactly this reason. One softer case: if the Autotask *contact* lookup that matches
your email recipients to client companies fails part-way (the rest of Autotask being fine), the
report still generates and is not marked partial, but the **Clients touched** tile gets
"· matching incomplete" appended and the timeline card carries the note "Client matching was
incomplete this run — some recipients may show as External. Refresh to retry." — so a lower
client count is never mistaken for a quiet day. **Refresh** (today) or
**Regenerate** (a past day) tries again. Saving to OneDrive is a separate step from generating
the report — if the report itself generates fine but the save to OneDrive fails, you're told
explicitly with an on-screen alert, and the report stays visible so you can try Refresh again
rather than losing it.

The AI step is a fourth, independent piece with two failure states of its own, and — like the
other three sources — neither one ever marks the whole report partial or stops it from saving.
**Unavailable** means the AI step didn't produce anything usable this run at all: Hatz.ai couldn't
be reached or timed out, the privacy notice hasn't been acknowledged on this machine, the
signed-in account changed mid-run, or Sent Items itself was unavailable (in which case its own
mail-unavailable note already explains why, so a second AI note isn't added on top of it). When AI
is unavailable, every email entry shows its plain subject line, the footer adds "Hatz.ai
unavailable — email entries show their subject.", and the source glyph reads **AI ✗**.
**Withheld** is different: the AI step ran and answered, but one or more individual lines didn't
pass a guardrail (or came back empty, missing, or too long) — those threads show a dashed **AI**
chip with the specific reason instead of a line, the footer adds "N lines withheld by guardrail."
instead, and the glyph still reads **AI ✓**, since the step itself succeeded overall. A report
with AI unavailable, or with some lines withheld, is still a complete, saved report like any
other.

## Coming next

Phases 1 and 2 are today's report, history and backfill, the first-run privacy screen, AI
one-liners with guardrails, and Email me — everything above is what's actually shipped. Not yet
in this release, planned for **Phase 3**:

- **End-of-day auto-generate** — the day's report generating itself on a schedule instead of
  only on first open.
- **This week** — a rolled-up view across the current week, not just one day at a time.
- **Baselines** — comparing a day or week against your own recent average, with sparklines.
