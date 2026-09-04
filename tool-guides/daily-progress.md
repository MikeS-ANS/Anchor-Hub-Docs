# Daily Progress

> **This tool exists only in an unreleased development branch (all four phases built).** It is not in
> the released app you're using today — nothing on this page describes anything you can open
> right now unless someone has specifically told you they're testing the new build with you.
> Once it ships, it will also need a grant from **Access Management** before it shows up in
> anyone's sidebar (see "Getting access" below) — being on a role that normally gets everything
> is not enough on its own. This page describes the tool as Phases 1–4 actually built it.

Daily Progress is a personal report of your own day at ANS: what you actually did, not what you
were assigned. It reads your own sent email, your own calendar, your own Autotask activity, and
your own Teams chats — who you talked with and how many messages each way, never what was
said — for one day at a time, and turns them into four headline numbers, a timeline of
everything that moved forward sorted earliest to latest, and a card of the day's chats.
Nothing about it is shared. There is no
manager view, no team rollup, and no admin view of anyone else's day, including yours.

Sidebar tool key: `daily-progress`.

---

## What it shows

Across the top, four tiles:

- **Actions taken** — sent-mail threads plus Autotask tickets you touched that day plus the Teams
  chats you posted in, added together, with the split shown underneath (e.g. "12 email threads ·
  5 tickets · 4 chats · 9 received"). The received figure is how many chat messages reached you
  across all your chats that day, and it is left out entirely when it's zero — it is never added
  into the total, because a chat you only read is never an action.
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

Directly under the tiles sits a one-line **Teams · Microsoft's report for this day** strip —
Microsoft's own daily Teams totals for that day, which are a separate measurement from the chat
count in the Actions tile and are never added to it. See [Teams](#teams) below.

Below that, **"What actually moved forward"** is a single timeline combining every sent-mail
thread, every ticket you touched, and every meeting that's already happened that day — earliest
first. A meeting that hasn't happened yet does not appear in this timeline; it only shows in the
separate Meetings section below it. Each sent-email thread in the timeline
can also carry one AI-written line beneath its subject, marked with a small **AI** chip, with one
legend beside the card's heading noting that these lines are written by Hatz.ai (the copy of the
report saved to your OneDrive and the one you email yourself go a step further and label each line
"written by Hatz.ai" individually) — see [AI one-liners](#ai-one-liners) below. Everything else on
this page — the four tiles, the computed callout, the ticket and meeting entries — is measured,
never generated.

**Teams chats are not in that timeline.** They have their own card directly beneath it, with no
times at all: a chat runs across the whole day, so a single clock time next to one would have said
"this is when you talked," when it was only the first thing you said. The card lists who you talked
with and how many messages went each way instead — the two things that stay true all day — busiest
conversation first. See [Teams](#teams) below.

Under both of those cards, when there's real activity to describe, one computed sentence summarizes
the day (e.g. "7 actions across 3 clients; 2 of them closed tickets") — arithmetic on the numbers
already shown above it, marked "not AI," never a generated summary. It sits below the chats card
rather than inside the timeline, because it counts your chats too, and a line claiming nine actions
directly underneath a list of four would be reconciling two different things.

A separate **Meetings** card lists the day's whole calendar — past and upcoming, counted and not
— so you can see what's ahead as well as what's done.

---

## Teams

Teams shows up in two completely separate places, measuring two different things. Your own chats
are read live, count as actions, and fill the **Teams chats** card. Microsoft's own daily Teams
totals are pulled server-side and shown in the labelled strip under the tiles. **The two are never
added together** — the strip's numbers never reach a tile, and the tiles' chat count never reaches
the strip.

### Your chats (live)

**What is read.** The 1:1, group and meeting chats you are in, through your own account only. It
never reads a Teams channel: the only three things the code asks Microsoft for are your own chat
list, the messages inside one of your own chats, and that chat's member list. The Hub applies no
filter of its own to that list beyond how recently each chat was last used — it walks your chats
newest-first and stops at the first one with no message that day.

**What counts.** Only a conversation you actually posted in that day becomes an action. A chat you only read, or
one where only other people wrote, is never an action — although the messages that came in still
count toward the "received" figure in the Actions tile. Messages are counted by when they were
written, so a much older message that somebody edited or reacted to today doesn't become today's.
System events (a member added, a chat renamed) and deleted messages are not messages and are not
counted at all.

**What is shown.** Each counted chat is one row in the **Teams chats** card, which sits under the
timeline and has no time column: the chat's title, "· meeting chat" after it if it's a meeting's
chat, and "3 sent · 2 received" on the right. Rows run busiest conversation first (the two counts
added together), because with no times there is nothing for earliest-first order to mean. The card
appears on any report whose run asked Teams at all — including a run where Teams failed, or one
where you posted in nothing, both of which say so in a line of their own rather than leaving you
guessing. A report generated before Teams was ever part of the tool has no card at all.
The title is the chat's own topic whenever it has one (e.g. "Exxel
— domain consolidation"); with no topic, the shape names the people instead — "Chat with Kate"
for a 1:1, "Group chat with Kate, Andi +2" for a group (two first names, the rest counted), and
"Meeting chat" for a meeting's chat. First names are all that's ever kept from the member list —
at most eight per chat, never an address and never an id — and a bot or app in a chat is recorded
as "a bot" rather than as a person, so it can never be named as one.

**What is never shown or stored.** The text of any message. Anyone's email address. Any Teams id
— a chat's id is used for those two follow-up reads and then dropped before anything is returned.
And **no AI line, ever**: nothing from Teams goes to Hatz.ai. The AI step is handed email threads
and nothing else, so a chat row has no AI line to withhold in the first place. Nor a time — see
above.

**When the count may be short.** One run looks at up to 40 chats. If there were more than that,
if some individual chats couldn't be read, or if reading them was taking too long (the Teams
read gives itself about 45 seconds and then stops where it is, so a slow day still gets a partial
count instead of nothing), the Teams chats card adds *"Not every chat could be counted this
run — the chat count may be short."* rather than presenting a short number as if it were complete.
The same day is also never recorded as a genuinely quiet one, since the count that would have
proven it was quiet is the count that's in doubt. If *none* of your chats could be read, that is
not a short count at all: Teams is treated as unavailable for that run — the banner and "chats
unavailable" described under [When something is unavailable](#when-something-is-unavailable) —
never as zero chats. While a report is generating, the loading strip shows a **Teams** line
counting off "N of 40 chats."

**About the permission.** Reading your own chats needs Microsoft's `Chat.Read`, and `Chat.Read` is
full content access to every chat you are in — Microsoft offers nothing narrower. The narrower
promise is kept in the Hub's own code instead: every message is reduced to when it was created,
whether you sent it, and whether it's a real message, inside the loop that reads it, before
anything is stored or returned. That's the same shape of promise `Mail.Read` already carries here:
mailbox-wide permission, one folder ever queried (Sent Items).

### Microsoft's report (the strip)

Under the four tiles, one line labelled **Teams · Microsoft's report for this day** (in This week
it reads just **Teams · Microsoft's report**). This is Microsoft's own measurement of your Teams
day — messages posted, calls, meetings, and the time spent in calls and meetings — not the Hub's
count of your chats. "Messages posted" is Microsoft's team-chat and private-chat message counts
added together; the one duration shown is audio time, since video and screen sharing run
alongside audio and summing the three would overstate it.

The strip has one of these to say:

- **Populated** — e.g. "44 messages posted · 3 calls · 2 meetings · 1 h 25 m in calls and
  meetings" (the duration is dropped when there was none). Hovering shows *"Microsoft's report ·
  refreshed"* and the date Microsoft itself last refreshed the report — or just *"Microsoft's
  report"* if the file didn't carry one. A day Microsoft published as all zeros is a real
  measurement and shows here as zeros — the same fact as "no activity" below, just spelled out by
  Microsoft rather than inferred from your absence from the file.
- **No activity** — *"no Teams activity recorded."* Hover: *"Microsoft's report covers this day —
  it has no chats, calls or meetings for you in it."* Microsoft published the day; you simply
  weren't in Teams. This is a real measurement, not a gap.
- **Pending** — *"pending — Microsoft publishes it a day or two later."* Hover: *"Not yet
  published by Microsoft."* A day Microsoft hasn't published at all, while it's three days old or
  less; past that it reads not available.
- **Not available** — *"not available — before the Hub kept this report, or older than 400
  days."* Hover: *"No row exists for this day."*
- **Could not be read** — *"could not be read right now — refresh to retry."* Hover: *"The Hub
  API did not answer."* An unreadable strip never falls back to a zero and never reads "pending."
- While it's still loading, it reads *"reading Microsoft's report…"*

**A quiet day and an unpublished one are told apart deliberately, and that distinction cost a
round of work.** Microsoft is inconsistent about people with nothing to report: on some days its
file lists you with zeros across the board, and on other days it leaves you out of the file
altogether. Measured across three real weekends, both happened. So "you have no row" cannot mean
"Microsoft hasn't published it" — the Hub also checks whether the day's report arrived *at all*,
which it can tell because the day holds rows for other people. A day that arrived but has nothing
for you reads "no Teams activity recorded"; only a day that genuinely hasn't arrived reads
pending. Before this, an ordinary quiet Saturday claimed to be *"not available — before the Hub
kept this report, or older than 400 days,"* which was simply untrue about a day six days old.

In **This week**, the same line sums the week's published days and names only the ones it doesn't
have: e.g. "1 of 3 days in · 44 messages posted · 3 calls · 2 h 05 m — Tue and Wed pending," with
the per-day refresh dates on hover. A quiet day counts toward the "N of N days in" — it's
accounted for, not missing — and is named on hover as "no activity" rather than being called out
as a gap in the line itself. Meetings are deliberately not summed in the weekly line.

**It is read fresh every time you look at a day, and it is never part of the report.** It isn't
saved into the day's JSON, it isn't in the standalone HTML copy in your OneDrive, and it isn't in
the report you email yourself — the totals often don't exist yet at the moment a report is
generated, so they're shown at view time only, the same rule the trend strip follows.

**Where the totals come from.** A scheduled job on the Hub's own Azure Functions server downloads
Microsoft's Teams user-activity report once a day (08:30 UTC — the middle of the night in Denver
either side of daylight saving) and keeps only the totals in one table. That table holds your
Entra object id, the date, the counts, the durations, Microsoft's own refresh date, and when it
was fetched — **and no name, email or user principal name at all**: those columns are read off
Microsoft's file and discarded, so the table has nowhere to learn a name. Each run pulls the two
most recently publishable days (three and two days back) and then fills the newest gaps inside a
28-day window, at most ten days per run, because Microsoft caps these report downloads at
14 per ten minutes for the whole app — so a newly created table fills itself in over several days
rather than all at once. Rows older than 400 days are deleted by the same run. If Microsoft's
report ever comes back with user ids concealed (a tenant-wide report-privacy setting), the run
refuses before writing anything at all, on the grounds that rows nobody can match to a person are
worse than no rows.

**Only you can read your own row.** The one route that reads that table filters on your own
signed-in token's object id and nothing else — you cannot ask it for anybody, and it will only
answer for at most a week at a time. There is no admin view of it, no team rollup, no export, and
nothing else in the Hub reads it. The one other thing that can touch it is an admin-only trigger
to run the same daily pull by hand, which returns counts of what it did and never a row.

---

## This week

A **Today / This week** switch sits in the header, next to the date. Switching to This week
shows Monday through the day you have selected — picking a Saturday or Sunday instead shows
that whole week's Monday through Friday, since there's nothing of its own to show for a weekend
day. The four tiles sum the week's saved daily reports (clients touched are counted once across
the whole week, not once per day it came up), and below them, "What actually moved forward"
lists each day's own saved entries grouped under its own heading — oldest day first, with a
quick tally of that day's actions, clients, chats (for a day that measured them), and hours next
to its heading. The weekly
Actions tile's split adds the week's chat figures the same way the daily one does — "· 8 chats ·
17 received," with received left out at zero. The **Teams ·
Microsoft's report** strip sits under the weekly tiles too, summing the week's published days.

The week gets its own **Teams chats** card under the day groups, on the same no-times principle as
the daily one — but merged across the week rather than repeated per day: one row per conversation,
its messages added up over every saved day it appeared on, with "· 3 days" next to a chat that ran
across several. Busiest conversation first. If Teams happened to be unavailable when one of the
week's days was generated, the card names that day and says its chats aren't included, rather than
quietly leaving them out of the totals.

A week with no chats in it is careful about which claim it makes, because "none" and "nobody
asked" are different facts. It says *"No chats you posted in this week"* only when every day in
the week was actually measured; when some weren't — a day never generated, a day that couldn't be
read, a day saved before Teams was part of the tool, or one whose Teams read failed — it says
*"No chats you posted in on the days that counted them"* instead, and if none of them measured
chats at all, *"No chats were counted for this week."*

A day with no saved report shows "No report saved for this day. Weekly totals above do not
include it." with a **Generate** button next to it — click it and that one day backfills in
place, exactly like generating any past day from the Today view, and the week reloads once it's
saved. A day that couldn't be read instead shows the error it hit and a **Retry** button. A day
that was saved with genuinely nothing on it reads "No sent mail, ticket work, chats or meetings
recorded. The day is saved and left out of your baselines." Either way, a callout under the
week's totals names which day or days aren't included in the numbers above it, and separately
names any saved day that had no recorded activity — "Monday had no recorded activity." — so the
totals read honestly against "N of 5 days." That callout only appears if the saved days have any
actions at all: a week where every saved day came back completely empty shows no callout, even if
other days are also missing or unreadable.

The **◀**/**▶** arrows move a week at a time (capped at the current week — you can't go past
today), and the date picker jumps to the week containing whichever day you pick there: Monday
through that day, or that week's Mon–Fri if you pick a weekend. **Refresh** re-reads the days
already saved for the week on screen — it never regenerates them; use the per-day **Generate**
button (or switch to Today and Refresh/Regenerate that single day) if you want a day's numbers
recomputed. **Email me** is disabled in this view, since a week is a rollup of saved reports, not
a saved report of its own to send. The trend strip's figure under each weekly tile reads
"typical N/day" — the same daily baseline described above, not a separate weekly one.

---

## AI one-liners

Every sent-email thread in the timeline can carry one AI-written line underneath its subject —
one plain sentence describing what you actually did in that thread, written by Hatz.ai, ANS's AI
vendor. What's sent to Hatz.ai for each thread is deliberately narrow: the subject line, the
first names of the people you sent it to, and the text you personally wrote in up to three of
your own emails in that thread that day (newest first), each cut off at 1,500 characters. The
quoted reply chain underneath your own text is never sent, and nothing else about that day ever
reaches Hatz.ai — not your inbox, not anyone else's message, not your calendar, not anything from
Autotask, and **nothing whatsoever from Teams**: the AI step is handed email threads and nothing
else, so no chat, chat title, participant or message count is ever part of the call. One report
means one call to Hatz.ai covering every thread from that day at once, never one call per thread.

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
for Sent Items, Calendar, Autotask, and Teams, so you can tell at a glance whether the lines you're
reading came from this run. A report saved before this update also shows **AI ✗** even though
nothing actually failed — a Phase 1 report simply has no AI data at all, and that reads the same
as "unavailable" until you Refresh or Regenerate that day. On a day with no sent-email threads at
all, both the **AI** legend beside the timeline's heading and the footer's AI ✓/✗ marker are
simply absent — there was nothing to summarise, so there's nothing to report on.

The text you send is not stored by Daily Progress — only the one screened sentence that comes
back is kept, saved as part of the report the same as everything else.

---

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
the thread so nothing resembling a real Graph conversation id is ever sent. Email threads are the
only thing in it: there is no Teams field in this shape at all, so no chat can reach Hatz.ai even
by accident. The tool Hatz.ai must
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

---

## Email this to me

The **Email me** button in the header sends the report currently on your screen to your own
mailbox — never anyone else's — with the report itself as the message body, and the same report
attached again as a standalone `<date>.html` file (e.g. `2026-09-02.html`) so you have a copy that
opens outside the Hub too. The subject line is always `Daily Progress — ` followed by the report's
date, written out like `Daily Progress — Wed, Sep 2, 2026`. The Teams chats card travels with it,
laid out the same way and with no times either; the Teams · Microsoft's report strip does not,
since it isn't part of the report.

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

---

## What counts

**A meeting counts** if it has at least one other attendee, you didn't decline it, it isn't
marked as an all-day event, and you weren't marked "Free" for it on your calendar. An event with
no other attendees (a personal block on your own calendar) is still listed in the Meetings card,
but it's never counted and never shown as a meeting elsewhere. A counted meeting is marked
**past** once its end time has already passed as of when the report was generated — never
"attended," since Daily Progress has no way to know whether you were actually in it. A private
event shows as "Private" in place of its real subject, but otherwise counts the same as any other
meeting. A meeting's own Teams chat is a separate thing from the meeting: if you posted in it, it
appears as its own row in the Teams chats card marked "· meeting chat," alongside the meeting
itself in the Meetings card.

**Actions taken**'s ticket half, and **Hours logged**'s ticket count, both mean the same thing:
the number of distinct Autotask tickets you touched that day. A ticket counts as touched if you
logged time against it, if it was closed that day, or if you wrote a note on it — but only a
note a person actually typed. Automated notes from RMM tools, workflow rules, integrations, or
any other system process don't count as touching a ticket. Time logged against a general project
task (no specific ticket) still adds to your hours and appears in the timeline as its own
"Project" entry, but it isn't one of the tickets counted in that number.

**Actions taken**'s chat half counts conversations, not messages: one chat you posted in fifty
times is one action, and the fifty shows in that entry's own "N sent" instead. See
[Teams](#teams) for what makes a chat count at all.

---

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
open and you're signed in at that moment. It runs Monday through Friday only — a Saturday or
Sunday never gets a report of its own. If Anchor Hub wasn't open when your chosen time arrived,
it catches up shortly after your next launch — the check runs about ten seconds in, and the run
itself starts up to a minute and a half later (a deliberate spread so several open copies of the
Hub don't all fire at once) — as long as you're signed in at that moment, and if you had signed
out, the check runs again the moment you sign back in. The same run also fills in the **previous
workday** if it has no saved report at all: if yesterday — or Friday, on a Monday — has no
report, it's generated too, so a day the Hub was closed still ends up with one. That
previous-workday fill is the one part that isn't limited to weekdays: a run over the weekend
won't generate a Saturday or Sunday report, but it will still fill in Friday if Friday has none.
A scheduled report's header names its source the same way any report does, with "scheduler" in
place of the usual tag — for example "as of 4:32 PM · scheduler" for today, or "generated Wed
4:31 PM · scheduler" for a filled-in past day.

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

---

## Where it's saved

Every report is written to your own OneDrive, not to a shared Hub database — no report about your
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

The one thing the Hub's own server-side database holds is Microsoft's daily Teams totals — a
totals-only table with no name, email or user principal name in it, readable only through a route
that answers for your own account and nobody else's, and purged after 400 days. Nothing from your
report is written to it, nothing in it is written into your report, and no admin, manager or
rollup view of it exists. See [Teams](#teams) and [Privacy](#privacy).

Two more files live on this machine only, and never in OneDrive: your Daily Progress settings and
the scheduler's own record. The three toggles/time above, and when the scheduler last ran, are
per computer, not per person — shared by whoever signs in here, the same as described in
[Generate at end of day](#generate-at-end-of-day) above. Two things in these same files are kept
per account instead: which privacy-notice version you've acknowledged, and the last scheduled
run's outcome — each visible only to the account it belongs to.

---

## Privacy

Daily Progress reads four things, and only for the signed-in user running it, one day at a
time: your **Sent Items** (not your inbox — just what you sent), your **calendar**, your own
**Autotask** time entries, ticket notes, and closed tickets, and your own **Teams chats** — who
you talked with, when, and how many messages each way, never what was said. It does not read your
inbox, the text of any Teams message, any Teams channel, or anyone else's mail, calendar,
tickets, or chats. The first time you open the tool, before it ever generates a report, a one-time
screen spells this out plainly — what it reads, what it doesn't, where it's stored, what ANS's own
report supplies, what (if anything) leaves Microsoft, and who can see it — and nothing runs until
you acknowledge it. A scheduled run reads exactly what a Refresh would, for the signed-in account
it belongs to only, and applies this same acknowledgement check before it will generate an AI
one-liner for anything.

One thing does leave Microsoft, and it's the same one thing as before: the text you wrote in the
emails you sent — never the quoted reply chain underneath it, and never anyone else's message —
goes to Hatz.ai, ANS's AI vendor, to be summarised into a single sentence per thread (see
[AI one-liners](#ai-one-liners) above for exactly what's sent). That text is not stored by Daily
Progress; only the one screened sentence that comes back is kept. **Nothing from Teams leaves
Microsoft** — the AI step never sees a chat — and nothing else leaves Microsoft or Autotask.

The notice is versioned, and the app remembers which version you last acknowledged. This build
raises it to **version 3**, because your Teams chats are now read and because Microsoft's own
Teams report now supplies daily totals to the Hub's server, so anyone who had already
acknowledged an earlier version sees the screen once more, subtitled "Privacy notice · updated
for Teams," rather than being quietly opted in. It opens with "One thing changed since you last
acknowledged this. Here is exactly what it reads, where it keeps the result, and what leaves
Microsoft 365." — "Two things changed" for anyone still on Phase 1's notice, for whom the Hatz.ai
call is new as well — and an amber banner above the full notice names exactly what's new:

> One thing is new: your Teams chats are now counted — who, when and how many messages, never
> what was said — and Microsoft's own Teams usage report supplies your daily totals to the Hub's
> server, where only you can see your row.

or, for someone who last acknowledged Phase 1's notice:

> Two things are new since you acknowledged this: the text of emails you sent now goes to
> Hatz.ai to be summarised into one line each; and your Teams chats are now counted — who, when
> and how many messages, never what was said — with Microsoft's own Teams usage report supplying
> your daily totals to the Hub's server, where only you can see your row.

The notice itself says, in six rows:

- **What it reads** — "Your Sent Items, your calendar, your Autotask time entries, ticket notes
  and closed tickets, and your Teams chats — who you talked with, when, and how many messages
  each way; never what was said. One day at a time."
- **What it does not read** — "Your inbox. The text of any Teams message, or any channel. Anyone
  else's mail, calendar, tickets or chats."
- **Where it is stored** — "Your own OneDrive, in `Apps/Anchor Hub`. Yours to open, move or
  delete."
- **What ANS's own report supplies** — "Microsoft's daily Teams usage report gives the Hub your
  totals — messages posted, calls, meetings, minutes — a day or two later; the Hub's server keeps
  only your totals, and only you can see your row."
- **What leaves Microsoft** — "The text of emails you sent goes to Hatz.ai, ANS's AI vendor, to be
  summarised into one line each. That text is not stored — only the line."
- **Who can see it** — "Only you. There is no manager view, team rollup or admin view."

**The totals table.** Phase 4 gives Daily Progress its first server-side table, and it holds
totals only: an Entra object id, a date, Microsoft's message/call/meeting counts, the audio, video
and screen-share durations, Microsoft's own refresh date, and when the row was fetched. There is
deliberately no column for a name, an email or a user principal name — those columns exist in
Microsoft's file and are discarded in code before a row is built — so the table cannot identify
anyone to anyone who doesn't already hold their object id. It keeps a rolling 400 days and purges
the rest daily. The only thing that reads it is a route that filters on the caller's own validated
token, at most a week at a time, which means there is no admin view, no team rollup, no export
and no comparison with anyone else. That is the same no-manager-view, no-team-rollup principle the
rest of the tool follows, restated against a shared database instead of your OneDrive.

That route answers with one thing beyond your own rows: **the list of dates in the week you asked
about for which the table holds any row at all**. It is dates and nothing else — no name, no
object id, no count, no number, and nothing that changes depending on who is asking — and it is
what lets the strip say "no Teams activity recorded" instead of wrongly calling a quiet day
unavailable. It tells you that Microsoft published a day's report, which is a fact about the
report, not about any person in it.

**Two Microsoft permissions worth naming explicitly.** Reading Microsoft's usage report requires
`Reports.Read.All`, which is a tenant-wide permission covering every Microsoft 365 usage report,
held by the Hub's own server identity rather than by anyone's copy of the app — Mike granted it as
CISO knowing that breadth, and this one report function is the only thing in the Hub that uses it;
reading any other report would be a new decision, not a tweak. Reading your own chats requires
`Chat.Read`, which needed tenant admin consent, and which is full content access to every chat you
are in (see [Teams](#teams) for how the code narrows that to who, when and how many). `Chat.Read`
is only added to the app's sign-in as of this build, but because it is admin-consented tenant-wide,
it is normally added to an existing session silently — confirmed live during the build, with no
re-sign-in and no permission prompt. If for any reason your session can't pick it up, the Teams
source says so in as many words rather than failing quietly; see [When something is
unavailable](#when-something-is-unavailable).

Your acknowledgement is recorded against your Microsoft account on each computer you use Daily
Progress on. If a second person signs in to the Hub on the same computer, they see the notice
themselves and acknowledge it for themselves — one person's acknowledgement never speaks for
anyone else's, and until it's given, nothing that person wrote goes to Hatz.ai. One consequence
of the per-account change made in an earlier build: if you acknowledged the notice before that
change, you'll see it once more regardless, because the older record was kept per computer rather
than per person and is no longer trusted.

---

## Getting access

Once this ships, getting to Daily Progress takes three things: an Access Management admin has to
grant the `daily-progress` tool to your role (or to you specifically), you then have to turn it
on for yourself in **Sidebar Layout** the same way you would any other tool (tools default
to hidden on a fresh install), and — because this tool needs two Microsoft permissions
(`Mail.Read` and `Chat.Read`) that weren't part of the Hub before — a session that somehow doesn't
pick them up silently needs one sign-out and sign-in. Both are admin-consented tenant-wide, so
that should be rare, and the app tells you plainly if it applies to you.

---

## History and past days

In Today view, the **◀** and **▶** arrows step one day at a time, and the date picker next to
them jumps straight to any day up through today (you can't pick a future date). If a day you
navigate to was never generated, you'll see a **"No report saved for this day"** message with a
**Generate it** button — Sent Items, calendar, Autotask, and your Teams chats still have that
day's data, so generating it later saves it to your OneDrive exactly like any other day. Today's
report generates itself automatically the first time you open the tool that day; past days only
generate when you ask. (This week has its own arrow behaviour — see [This week](#this-week)
above.)

One caveat specific to chats when you backfill a day well after the fact: a message written that
day but edited or reacted to more than a couple of days later can drop out of what Microsoft
returns, so a very old backfill can undercount a chat. A day generated on the day itself, or a
day or two later, is unaffected.

---

## When something is unavailable

Each source — Sent Items, calendar, Autotask, Teams — is read independently, so one of them being
temporarily unavailable doesn't stop the other three. When that happens, a banner across the top
names the source that failed. **Meetings** and **Hours logged** each depend on exactly one
source (calendar and Autotask respectively), so if that source fails, the tile shows **"—"**
instead of a number, with a note underneath saying that source is unavailable rather than the
day genuinely being empty. **Actions taken** and **Clients touched** each depend on more than one
source; if only some of them fail, they still show a real number built from whichever sources
succeeded, with a note like "email unavailable" appended — Clients touched only drops to "—" if
both mail and Autotask fail at once, and Actions taken only if mail, Autotask and Teams all do.
The footer at the bottom of every report also shows a ✓ or ✗ per source for exactly this reason.

When **Teams** is the source that failed, the banner reads *"Teams was unavailable this run —
chats are not shown. The email, calendar and ticket sections are complete. Refresh to retry."*,
the Actions tile's split says "chats unavailable" in place of a chat count, the Teams chats card
says the same thing in place of its rows, and the footer reads **Teams ✗**. The chat count is left out rather than shown as a
zero, because nothing was measured. The **Teams · Microsoft's report** strip is completely
independent of that failure — it is read from the Hub's own API, not from your chats — so it can
still be populated on a run where your chats couldn't be read. It has its own failure state
instead: *"could not be read right now — refresh to retry."* A report generated before Phase 4
existed simply has no Teams data at all: no chats segment in the Actions split, no Teams chats
card, and no Teams ✓/✗ in the footer, since that run never tried.

If your sign-in doesn't carry the Teams permission, the Teams source fails with *"Daily Progress
needs a fresh sign-in to read your mail and Teams chats. Sign out and sign back in."* — and doing
that once fixes it. This shouldn't be common: `Chat.Read` is admin-consented tenant-wide and is
normally added to an existing session silently. Everything else in the report is unaffected.

One softer case: if the Autotask *contact* lookup that matches
your email recipients to client companies fails part-way (the rest of Autotask being fine), the
report still generates and is not marked partial, but the **Clients touched** tile gets
"· matching incomplete" appended and the timeline card carries the note "Client matching was
incomplete this run — some recipients may show as External. Refresh to retry." — so a lower
client count is never mistaken for a quiet day. A run that couldn't get through all your chats
carries its own equivalent note, "Not every chat could be counted this run — the chat count may
be short." **Refresh** (today) or
**Regenerate** (a past day) tries again. Saving to OneDrive is a separate step from generating
the report — if the report itself generates fine but the save to OneDrive fails, you're told
explicitly with an on-screen alert, and the report stays visible so you can try Refresh again
rather than losing it. In **This week**, a day that couldn't be read shows its own error and a
**Retry** button, without affecting the other days already loaded.

The AI step is a fifth, independent piece with two failure states of its own, and — like the
other four sources — neither one ever marks the whole report partial or stops it from saving.
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
still a complete, saved report like any other. Chat entries are untouched by any of this — they
never had an AI line to lose.

A scheduled run has three cases worth calling out on their own. If nobody is signed in on this
machine at the scheduled time, nothing runs at all — there's no report to show and no error,
since there's no one for it to belong to. If Anchor Hub itself was closed at that time, it
catches up the moment it's next opened, as long as you're signed in when it opens, as
described in [Generate at end of day](#generate-at-end-of-day) above — and if you had signed out
instead, the check runs again the moment you sign back in. Remember that the day's own report is
generated Monday through Friday only, though the previous-workday fill still runs at weekends.
And if a scheduled run runs into trouble — whether it couldn't generate the report at all, or it
generated fine but couldn't send the email — you're not left to guess: the inline message
described above names which of the two happened, and appears the next time you open Daily
Progress, once, addressed to whoever's run it was.

---

## Coming next

Phase 4 completes the tool as specified. Deliberately not built, and not planned without a new
spec and a CISO decision: Teams channel messages; any AI reading of Teams text (chat topic
summaries); received-only chats as actions; any view of another person's Teams data, report, or
totals — the no-manager-view, no-team-rollup principle stands, including for the server-side
totals table.
