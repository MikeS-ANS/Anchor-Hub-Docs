# Who is Where (Daily On-Site Briefing)

> **Not yet in the released app.** This describes the feature as built on the `feature/onsite-briefing` branch. Once it ships, the Teams post appears in the *Who is Where Dispatch* channel and the widget appears on everyone's Hub home screen — there is nothing to turn on per person.

Who is Where (the Daily On-Site Briefing) answers one question every weekday morning: **who is on-site with a client today, where, and when?** It reads the service calls dispatch schedules in Autotask and shows them in two places:

- **A Teams post** in the *Who is Where Dispatch* channel (All Staff team), posted at 7:00am and updated in place through the day until 5:00pm.
- **A "Who is Where" widget** on the Hub home screen, a full-width card under Today's calendar and My Rocks & To-Dos, with Today, Tomorrow, This Week and Next Week views and a month calendar.

Both come from the same feed, so they always agree.

## What counts as an on-site

A service call is classified in this order, and the first match wins:

1. Its status is **Onsite** (the new status dispatch sets when creating the call) — it is an on-site. A status of **Remote** means it is left out.
2. It was created by TimeZest — those are always remote sessions and are left out.
3. Its description mentions "onsite" or "on-site" — it is an on-site. A description that only says "remote" is left out.

A service call with none of those clues appears in a second group, **"Service calls without a type set"**, so a visit nobody labelled is still visible rather than hidden. When that group is empty for a week, dispatch has fully adopted the Onsite/Remote statuses.

Only service calls are read. Nothing comes from Outlook calendars, and the description of a call is never shown — only the client, city, time window, ticket or task number and the assigned tech(s).

## The Teams post

- Strictly chronological, earliest visit first: for each visit, the client (and city) in bold, then a line of **time block · tech(s) · ticket or task number**. A call with two techs lists both on the line.
- The number is a clickable link: a ticket-linked visit links to the ticket in Autotask, and a project-task visit links to its project, followed by the project name.
- A completed call stays on the list with a ✓ so the card remains a record of the day.
- On a day with nothing scheduled, the card says **"No on-sites scheduled today."**
- If Autotask can't be read at 7:00, nothing is posted yet and it tries again on the next half-hour. If it still can't be read from 8:00 on, the card says so plainly instead of pretending the day is empty. It is replaced by the real list as soon as Autotask answers.
- Visits with no Onsite/Remote status and no other clue appear under **"Service calls without a type set (may be on-site)"**.
- Every card ends with a reminder and a **Create a service call** button that opens Autotask's new-service-call screen. Heading to a client? Create the service call and you'll be on the card within 30 minutes.
- If today's post is deleted in Teams, it is not re-posted that day.

Don't want the card? Mute the channel in Teams — there are no direct messages.

## The home widget

The widget is titled **Who is Where**. Toggle between **Today | Tomorrow | This Week | Next Week** (Today is the default; weeks run Sunday to Saturday). The week views group rows under day headings such as "Wed Sep 30" and skip days with nothing scheduled.

Each row is one visit on one day it covers: the time on the left, the client (with city) in bold, and beneath it a ✓ when the visit is complete, the time block, the tech(s) and the ticket or task number. A task-linked visit also shows its project name. A visit that started on an earlier day shows **"↔ cont."** in the time column. A completed visit's row is dimmed with that line struck through.

Hover a row for the tech(s) and the ticket, task or project title. Click a ticket-linked row to open the ticket in Autotask; a task-linked row opens its project. An empty view says **"No on-sites today."**, **"No on-sites tomorrow."**, **"No on-sites this week."** or **"No on-sites next week."** If the schedule can't be read it says **"On-site schedule unavailable."** — it never shows an empty view it didn't actually confirm. The reminder and the **Create a service call** button stay at the bottom.

### The calendar

The **Calendar** button on the widget opens a month view inside the Hub (an overlay, not a separate window):

- Weekday columns run Sunday to Saturday. The forward and back arrows move a month at a time and grey out beyond about a year either way.
- Each day shows a chip per visit with the client name. Hover a chip for the tech(s), ticket or task number, the ticket or task title, project and time; click a chip to open the ticket or project in Autotask.
- A visit spanning several days shows on every day it covers, with **"↔"** on the later days.
- Close it with the **Close** button, the **Escape** key, or by clicking outside it.

### Looking ahead

Tomorrow, This Week, Next Week and the calendar exist so teams can coordinate ahead of time — for example, "Perry's at that client Friday, can he take this?" Recurring dispatches need no extra setup: Autotask exposes each occurrence as its own service call, so a recurring visit appears on every day it is scheduled. Client Success visits logged as service calls against client time-tracking tasks show with the task number and project name.

## For dispatch

- Set **Onsite** or **Remote** as the status when creating a service call. That replaces "New"; completing the call later works exactly as before.
- Recurring on-sites (like a weekly Wednesday visit) need a real recurring service call, not just a placeholder ticket — a ticket alone never appears on the card.
- TimeZest bookings need nothing; they are recognised automatically.

## Behind the scenes

The feed runs in Azure every 30 minutes on weekdays between 7:00am and 5:00pm Denver time, using the Hub's read-only Autotask access. The feed can be asked for any range up to about two months, within about a year either way of today. It never writes to Autotask. The only thing it stores is which Teams post belongs to which day and two counts (on-site, untyped), so the post can be updated in place and adoption of the new statuses can be measured. The untyped count includes only visits still open: completing a service call makes Autotask overwrite its Onsite/Remote status, so a completed call with no other clue is not held against dispatch.

---

*This was Hub idea #51, "Daily on-site briefing posted to Teams", submitted in Ideas & Bugs by Mike Stewart, and shaped by him on 2026-09-29 into the channel post plus home widget built here.*
