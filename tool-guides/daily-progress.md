# Daily Progress

> **This tool exists only in an unreleased development branch (all three phases built).** It is not in
> the released app you're using today — nothing on this page describes anything you can open
> right now unless someone has specifically told you they're testing the new build with you.
> Once it ships, it will also need a grant from **Access Management** before it shows up in
> anyone's sidebar (see "Getting access" below) — being on a role that normally gets everything
> is not enough on its own. This page describes the tool as Phases 1–3 actually built it.

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

Under each tile, once you have at least two earlier qualifying workdays, a small sparkline
appears: those days' own numbers, ending with the day currently on screen as a highlighted dot.
Once you have at least five such days, a **typical** figure joins it — the median of up to your
last 30 such days before the day you're looking at, counting only complete Monday–Friday reports
and skipping partial runs and days with nothing on them. It only ever compares you with your own
recent days, and the day you're viewing is never counted toward its own baseline. Neither figure
travels with the report — the copy you email yourself or save to OneDrive doesn't carry them,
since they're computed fresh from the history index in your OneDrive each time the page loads,
not stored as part of the report itself.

Below the tiles, **"What actually moved forward"** is a single timeline combining every sent-mail
thread, every ticket you touched, and every meeting that's already happened that day — earliest
first. A meeting that hasn't happened yet does not appear in this timeline; it only shows in the
separate Meetings section below it. Under the timeline, when there's real activity to describe,
one computed sentence summarizes the day (e.g. "7 actions across 3 clients; 2 of them closed
tickets") — this line is arithmetic on the numbers already shown above it, marked "not AI,"
never a generated summary. Each sent-email thread in the timeline can also carry one AI-written
line beneath its subject, marked with a small **AI** chip, with one legend beside the card's
heading noting that these lines are written by Hatz.ai (the copy of the report saved to your
OneDrive and the one you email yourself go a step further and label each line "written by
Hatz.ai" individually) — see [AI one-liners](#ai-one-liners) below. Everything else on this page
— the four tiles, the computed callout, the ticket and meeting entries — is measured, never
generated.

A separate **Meetings** card lists the day's whole calendar — past and upcoming, counted and not
— so you can see what's ahead as well as what's done.

## This week

A **Today / This week** switch sits in the header, next to the date. Switching to This week
shows Monday through the day you have selected — picking a Saturday or Sunday instead shows
that whole week's Monday through Friday, since there's nothing of its own to show for a weekend
day. The four tiles sum the week's saved daily reports (clients touched are counted once across
the whole week, not once per day it came up), and below them, "What actually moved forward"
lists each day's own saved entries grouped under its own heading — oldest day first, with a
quick tally of that day's actions, clients, and hours next to its heading.

A day with no saved report shows "No report saved for this day. Weekly totals above do not
include it." with a **Generate** button next to it — click it and that one day backfills in
place, exactly like generating any past day from the Today view, and the week reloads once it's
saved. A day that couldn't be read instead shows the error it hit and a **Retry** button. Either
way, a callout under the week's totals names which day or days aren't included in the numbers
above it, as long as the saved days have any actions at all — a week where every saved day came
back completely empty shows no callout, even if other days are also missing or unreadable.

The **◀**/**▶** arrows move a week at a time (capped at the current week — you can't go past
today), and the date picker jumps to the week containing whichever day you pick there: Monday
through that day, or that week's Mon–Fri if you pick a weekend. **Refresh** re-reads the days
already saved for the week on screen — it never regenerates them; use the per-day **Generate**
button (or switch to Today and Refresh/Regenerate that single day) if you want a day's numbers
recomputed. **Email me** is disabled in this view, since a week is a rollup of saved reports, not
a saved report of its own to send. The trend strip's figure under each weekly tile reads
"typical N/day" — the same daily baseline described above, not a separate weekly one.

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

Every line that comes back is checked before you ever see it: for a money amount written as
digits or spelled out — a dollar, euro, pound or yen sign in front of a number, or a number followed
by a currency word or code (dollars, bucks, euros, pounds, quid, cents, USD, EUR, GBP), including the
"3k" shorthand; for salary, bonus, payroll, or other compensation language; and for an email
address, link, phone number, a long run of digits, or a credential word (password, PIN, API key,
secret, token, and the like) followed by "is", a colon or an equals sign and then a value —
"helped Michael with a password reset" passes, "password: Winter2026!" does not. A line that
fails any of those checks is withheld, and so is a line Hatz.ai simply couldn't produce — missing,
empty, or too long. A withheld thread shows a dashed **AI** chip with the exact reason instead of
a summary line:

- *"No AI line for this thread — Hatz.ai returned nothing usable. Subject shown instead."*
- *"Line withheld by guardrail — it named an amount. Subject shown instead."*
- *"Line withheld by guardrail — it named a pay or compensation detail. Subject shown instead."*
- *"Line withheld by guardrail — it contained an address, number, link or credential. Subject
  shown instead."*

If Hatz.ai is unavailable for the whole run, every email entry in the timeline just shows its
subject line the way Phase 1 always did, and the report footer adds *"Hatz.ai unavailable — email
entries show their subject."* If Hatz.ai answered but one or more individual lines were withheld
by a guardrail, the footer instead adds *"N lines withheld by guardrail."* — and if one or more
threads simply got nothing usable back from Hatz.ai (not a guardrail refusal, just an empty,
missing, or malformed answer), the footer separately adds *"N threads got no usable AI line."*;
a run that hit both counts adds both sentences. Either way, the footer's source list ends with
**AI ✓** (or, in red, **AI ✗** when the step didn't run this time) alongside the same ✓/✗ shown
for Sent Items, Calendar, and Autotask, so you can tell at a glance whether the lines you're
reading came from this run. A report saved before this update also shows **AI ✗** even though
nothing actually failed — a Phase 1 report simply has no AI data at all, and that reads the same
as "unavailable" until you Refresh or Regenerate that day. On a day with no sent-email threads at
all, both the **AI** legend beside the timeline's heading and the footer's AI ✓/✗ marker are
simply absent — there was nothing to summarise, so there's nothing to report on.

The text you send is not stored by Daily Progress — only the one screened sentence that comes
back is kept, saved as part of the report the same as everything else.

## AI Prompt

The AI line is generated by **one call per report** to Hatz.ai's Anthropic-native passthrough
endpoint (`https://ai.hatz.ai/v1/anthropic/messages`, via the Hub's shared `hatzToolChat.js`
transport), with a 25-second timeout, tried twice at most (about 50 seconds all told) — a stalled
or failing first attempt gets one retry, a second failure just means the AI step didn't run this
time. The call forces Hatz.ai to answer through a tool
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
answer through, `submit_lines`, takes a list of `{ key, line }` pairs — the prompt asks for exactly
one per thread key, and the app keeps only the first line it gets for each key, ignoring duplicates
and any key it never sent — with `line` documented as one plain-text sentence of at most 30 words.

**Guardrails**, checked against every line before it's shown, in plain words: no money amount
written as digits or spelled out — a dollar, euro, pound or yen sign in front of a number, or a
number followed by a currency word or code (dollars, bucks, euros, pounds, quid, cents, USD, EUR,
GBP), the "3k" shorthand included; no salary, bonus, payroll, or other pay/compensation language; no email
address, link, phone number, or long run of digits; no credential word (password, PIN, API key,
secret, token, and the like) followed by "is", a colon or an equals sign and then a value — a
line that merely mentions one of those words in passing, with nothing that looks like a value
after it, passes; and no line that's missing, empty, or too long. **On any failure** — Hatz.ai unreachable, a bad
response, or a line that doesn't pass a guardrail — the affected thread simply shows its subject
line instead, with a note explaining why; nothing else about the report changes, and the report
still saves.

The AI step may also not run at all this time, for reasons that have nothing to do with the model
itself: Sent Items was unavailable this run, there were no email threads that day to summarise,
the privacy notice hasn't been acknowledged yet by the account you're signed in as on this
machine, or the signed-in account changed partway through the run.

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

Two more things it checks before sending: a report only ever emails to the account it belongs to
— if you generated a report while signed in as one Microsoft account and then a different account
is signed in on this same computer, that report is refused rather than emailed to the wrong
mailbox. And a report saved by a different, incompatible version of Anchor Hub than the one
you're currently running is also refused, with a message naming the version that saved it —
Regenerate it, or update the app, rather than emailing something this build can't fully read back.

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

## Generate at end of day

A gear icon in the header, next to Email me, opens a small settings panel with three rows:

- **Generate at end of day** — a toggle, off by default. Its helper line: *"Runs while Anchor
  Hub is open. If it was closed, it catches up on next launch."*
- **Time** — a dropdown, enabled only once the toggle above is on, offering six choices: 3:30 PM,
  4:00 PM, 4:30 PM, 5:00 PM, 5:30 PM, and 6:00 PM.
- **Email me when it generates** — a second toggle, also only enabled once Generate at end of day
  is on. Its helper line: *"Scheduled runs only. Refresh never emails without the button."*

Turning **Generate at end of day** on means today's report generates itself once the time you
picked has passed — no need to open Daily Progress or click anything — as long as Anchor Hub is
open and you're signed in at that moment. It runs every day of the week, not just workdays: if
Anchor Hub is open on a Saturday or Sunday after that time, that day's report (usually empty,
since there's rarely anything to log on a weekend) is generated and saved the same as any other
day, and emailed too if **Email me when it generates** is on. If Anchor Hub wasn't open when
your chosen time arrived, it catches up shortly after your next launch — the check runs about
ten seconds in, and the run itself starts up to a minute and a half later (a deliberate spread
so several open copies of the Hub don't all fire at once) — as long as you're still signed in at
that moment; if you had signed out, nothing runs then, and that day is
instead filled in by the next scheduled run's previous-workday check, described next. The same
run also fills in the **previous workday** if it has no saved report at all: if yesterday — or
Friday, on a Monday — has no report, it's generated too, so a day the Hub was closed still ends
up with one. A scheduled report's header names its source the same way any report does, with
"scheduler" in place of the usual tag — for example "as of 4:32 PM · scheduler" for today, or
"generated Wed 4:31 PM · scheduler" for a filled-in past day.

Two edge cases worth knowing: changing the time to a *later* slot after today's report has
already generated makes today due again at the new time, so a second report — and, with Email me
on, a second email — can go out the same evening. And if the app can't read your saved reports
at the moment a scheduled run fires, that run produces nothing and shows nothing here (Windows
shows its own "Scheduled scan failed" notification instead); the previous-workday fill catches
the missed day the next time the job runs.

The same privacy gate applies to a scheduled run as to any other: if the account signed in on
this machine hasn't acknowledged the current privacy notice, its scheduled report still
generates and saves — just without any AI one-liners — exactly as a manual Refresh would behave
for that same unacknowledged account.

**Email me when it generates** only fires for a run the scheduler itself triggered, only after
that run's report actually saved, and never from a plain Refresh or from generating a day
yourself. If a scheduled run happens to land while you're already running your own Refresh for
that same day, it joins your run instead of starting a second one — that report is yours, it is
not auto-emailed, and nothing is shown about it; it's simply noted in the scheduler's own record.
If a scheduled send fails, you're told about it the next time you open Daily Progress: an inline
message appears once, and only to the account whose scheduled run it was — nobody else signed in
on this same machine sees it. Toggling any of these three settings takes effect immediately;
there's no need to close and reopen Anchor Hub for a change to apply. Both this panel's settings
and the scheduler's own record are saved on this machine only, in two separate local files — and
both are per computer, not per person: the three toggles above, and when the scheduler last ran,
are shared by whoever signs in here, so one person's schedule change controls the scheduled run
no matter which account is signed in when it fires. The scheduler's file also keeps the last
run's outcome — which day or days it covered, whether each one saved and was emailed, and any
error text — and that part is shown only to the account it belonged to.

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
**Open in OneDrive** link at the bottom of the settings panel (the gear in the header) takes you
straight to this folder in your browser, so it's yours to open, move, or delete any time you want.

Two more files live on this machine only, and never in OneDrive: your Daily Progress settings and
the scheduler's own record. The three toggles/time above, and when the scheduler last ran, are
per computer, not per person — shared by whoever signs in here, the same as described in
[Generate at end of day](#generate-at-end-of-day) above. Two things in these same files are kept
per account instead: which privacy-notice version you've acknowledged, and the last scheduled
run's outcome — each visible only to the account it belongs to.

## Privacy

Daily Progress reads three things, and only for the signed-in user running it, one day at a
time: your **Sent Items** (not your inbox — just what you sent), your **calendar**, and your own
**Autotask** time entries, ticket notes, and closed tickets. It does not read your inbox, Teams,
or anyone else's mail, calendar, or tickets. The first time you open the tool, before it ever
generates a report, a one-time screen spells this out plainly — what it reads, what it doesn't,
where it's stored, what (if anything) leaves Microsoft, and who can see it — and nothing runs
until you acknowledge it. A scheduled run reads exactly what a Refresh would, for the
signed-in account it belongs to only, and applies this same acknowledgement check before it will
generate an AI one-liner for anything.

As of Phase 2, one thing does leave Microsoft: the text you wrote in the emails you sent — never
the quoted reply chain underneath it, and never anyone else's message — goes to Hatz.ai, ANS's AI
vendor, to be summarised into a single sentence per thread (see [AI one-liners](#ai-one-liners)
above for exactly what's sent). That text is not stored by Daily Progress; only the one screened
sentence that comes back is kept. Nothing else leaves Microsoft or Autotask. The privacy notice is
versioned, and the app remembers which version you last acknowledged — because this is a real
change in what leaves your machine, not a wording tweak, anyone who had already acknowledged
Phase 1's notice sees the first-run screen again, once, the next time they open Daily Progress.
That re-shown screen is headed "What's changed before your next report," with the one new fact
called out in an amber banner above the same full notice, rather than quietly opting anyone in.
Acknowledging it again is all it takes to keep going, and nothing about what's read, where it's
stored, or who can see it has changed.

Your acknowledgement is recorded against your Microsoft account on each computer you use Daily
Progress on. If a second person signs in to the Hub on the same computer, they see the notice
themselves and acknowledge it for themselves — one person's acknowledgement never speaks for
anyone else's, and until it's given, nothing that person wrote goes to Hatz.ai. One consequence
of this build: if you acknowledged the notice in an earlier build, you'll see it once more, even
if you've already acknowledged this version. The older record was kept per computer rather than
per person, so it is no longer trusted.

## Getting access

Once this ships, getting to Daily Progress takes three things: an Access Management admin has to
grant the `daily-progress` tool to your role (or to you specifically), you then have to turn it
on for yourself in **Settings → Customize** the same way you would any other tool (tools default
to hidden on a fresh install), and — because this tool needs a new Microsoft permission
(`Mail.Read`) that wasn't part of the Hub before — you may need to sign out and sign back in
once after the update lands, so that permission is actually on your session (the app tells you
if it does).

## History and past days

In Today view, the **◀** and **▶** arrows step one day at a time, and the date picker next to
them jumps straight to any day up through today (you can't pick a future date). If a day you
navigate to was never generated, you'll see a **"No report saved for this day"** message with a
**Generate it** button — Sent Items, calendar, and Autotask still have that day's data, so
generating it later saves it to your OneDrive exactly like any other day. Today's report
generates itself automatically the first time you open the tool that day; past days only
generate when you ask. (This week has its own arrow behaviour — see [This week](#this-week)
above.)

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
rather than losing it. In **This week**, a day that couldn't be read shows its own error and a
**Retry** button, without affecting the other days already loaded.

The AI step is a fourth, independent piece with two failure states of its own, and — like the
other three sources — neither one ever marks the whole report partial or stops it from saving.
**Unavailable** means the AI step didn't produce anything usable this run at all: Hatz.ai couldn't
be reached or timed out, the privacy notice hasn't been acknowledged by the signed-in account on
this machine, the signed-in account changed mid-run, or Sent Items itself was unavailable (in which case its own
mail-unavailable note already explains why, so a second AI note isn't added on top of it). When AI
is unavailable, every email entry shows its plain subject line, the footer adds "Hatz.ai
unavailable — email entries show their subject.", and the source glyph reads **AI ✗**.
**Withheld** is different: the AI step ran and answered, but one or more individual lines didn't
pass a guardrail (or came back empty, missing, or too long) — those threads show a dashed **AI**
chip with the specific reason instead of a line, the footer adds "N lines withheld by guardrail."
and/or "N threads got no usable AI line." instead, and the glyph still reads **AI ✓**, since the
step itself succeeded overall. A report with AI unavailable, or with some lines withheld, is
still a complete, saved report like any other.

A scheduled run has three cases worth calling out on their own. If nobody is signed in on this
machine at the scheduled time, nothing runs at all — there's no report to show and no error,
since there's no one for it to belong to. If Anchor Hub itself was closed at that time, it
catches up the moment it's next opened, as long as you're still signed in when it opens, as
described in [Generate at end of day](#generate-at-end-of-day) above — if you had signed out
instead, that day is filled in by the next scheduled run rather than the moment you sign back in.
And if a scheduled run runs into trouble — whether it couldn't generate the report at all, or it
generated fine but couldn't send the email — you're not left to guess: the inline message
described above names which of the two happened, and appears the next time you open Daily
Progress, once, addressed to whoever's run it was.

## Coming next

Phases 1–3 complete the tool as specified: today's report, history and backfill, the first-run
privacy screen, AI one-liners with guardrails, Email me, end-of-day auto-generate with
auto-email, This week, and baselines with the trend strip. One more phase is planned. **Teams**
returns — Mike has decided it comes back with both of the postures the spec describes: (A) who /
when / how many messages per conversation from your own Teams chats, sent and received, live for
today, with message text never read and nothing from Teams ever sent to Hatz.ai; and (B) per-day
totals of messages posted, calls, meetings and audio/video/screen-share minutes from Microsoft's
own Teams activity report, which runs a day or two behind. That is a change to the spec, so it
goes through the Analyst seat (the spec amendment) and then Design (the mockup states) first, and
it becomes **Phase 4** after this release. Nothing in that phase will show anyone else's day —
the no-manager-view, no-team-rollup principle stands.
