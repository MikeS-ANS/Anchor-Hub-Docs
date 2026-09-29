# Daily On-Site Briefing

> **Not yet in the released app.** This describes the feature as built on the `feature/onsite-briefing` branch. Once it ships, the Teams post appears in the *Who is Where Dispatch* channel and the widget appears on everyone's Hub home screen — there is nothing to turn on per person.

The Daily On-Site Briefing answers one question every weekday morning: **who is on-site with a client today, where, and when?** It reads the service calls dispatch schedules in Autotask and shows them in two places:

- **A Teams post** in the *Who is Where Dispatch* channel (All Staff team), posted at 7:00am and updated in place through the day until 5:00pm.
- **A "Today's on-sites" widget** on the Hub home screen, a full-width card under Today's calendar and My Rocks & To-Dos.

Both come from the same feed, so they always agree.

## What counts as an on-site

A service call is classified in this order, and the first match wins:

1. Its status is **Onsite** (the new status dispatch sets when creating the call) — it is an on-site. A status of **Remote** means it is left out.
2. It was created by TimeZest — those are always remote sessions and are left out.
3. Its description mentions "onsite" or "on-site" — it is an on-site. A description that only says "remote" is left out.

A service call with none of those clues appears in a second group, **"Service calls without a type set"**, so a visit nobody labelled is still visible rather than hidden. When that group is empty for a week, dispatch has fully adopted the Onsite/Remote statuses.

Only service calls are read. Nothing comes from Outlook calendars, and the description of a call is never shown — only the client, city, time window, ticket number and the assigned tech(s).

## The Teams post

- Grouped by tech, earliest start first. A call with two techs shows under both.
- A completed call stays on the list with a ✓ so the card remains a record of the day.
- On a day with nothing scheduled, the card says **"No on-sites scheduled today."**
- If Autotask can't be read at 7:00, nothing is posted yet and it tries again on the next half-hour. If it still can't be read from 8:00 on, the card says so plainly instead of pretending the day is empty. It is replaced by the real list as soon as Autotask answers.
- Every card ends with a reminder and a **Create a service call** button that opens Autotask's new-service-call screen. Heading to a client? Create the service call and you'll be on the card within 30 minutes.
- If today's post is deleted in Teams, it is not re-posted that day.

Don't want the card? Mute the channel in Teams — there are no direct messages.

## The home widget

Same content as the card, one row per tech and visit: time, tech name, then client, city, time window and ticket number underneath. A completed visit shows a ✓ and the row is dimmed with the client and time line struck through (the tech's name is not). Hover a row for the ticket title; click it to open the ticket in Autotask. The widget has the same **Create a service call** button. If the schedule can't be read it says **"On-site schedule unavailable."** — it never shows an empty day it didn't actually confirm.

## For dispatch

- Set **Onsite** or **Remote** as the status when creating a service call. That replaces "New"; completing the call later works exactly as before.
- Recurring on-sites (like a weekly Wednesday visit) need a real recurring service call, not just a placeholder ticket — a ticket alone never appears on the card.
- TimeZest bookings need nothing; they are recognised automatically.

## Behind the scenes

The feed runs in Azure every 30 minutes on weekdays between 7:00am and 5:00pm Denver time, using the Hub's read-only Autotask access. It never writes to Autotask. The only thing it stores is which Teams post belongs to which day and two counts (on-site, untyped), so the post can be updated in place and adoption of the new statuses can be measured. The untyped count includes only visits still open: completing a service call makes Autotask overwrite its Onsite/Remote status, so a completed call with no other clue is not held against dispatch.

---

*This was Hub idea #51, "Daily on-site briefing posted to Teams", submitted in Ideas & Bugs by Mike Stewart, and shaped by him on 2026-09-29 into the channel post plus home widget built here.*
