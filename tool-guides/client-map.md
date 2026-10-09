# Client Map

> **On a feature branch and not yet released to staff.** This page is published while the tool is still being built, so you won't see Client Map in your Hub yet.

Client Map puts **every active Autotask customer on a map, one pin per site**, so you can see where clients are and which ones haven't been touched in a while. It is **read-only**: nothing you do on the map changes anything in Autotask.

Companies come **only from Autotask**. If a company isn't a Customer in Autotask, it isn't on the map, and there is no way to add one here. Fix it in Autotask and it shows up on its own.

## Pin colours

Each pin is coloured by how long it has been since that client was last touched (a logged Note or Quick Note, or a Client Meeting):

- **Green:** on track.
- **Amber:** due soon.
- **Red:** overdue.
- **Red, "never touched":** no note or meeting on record at all.
- **Muted:** **not yet synced.** The notes for that client haven't been collected yet. The card says: "The Touch Aging sync runs daily at 5:00 AM Mountain while someone has the Hub open, or an admin can run **Check for new data** in Client Touch Aging."
- **Grey:** **not tracked.** Clients on a T&M, Other or Unclassified plan never get a touch colour or a last-touched line.
- **Blue ring:** a **dispatch visit is booked this week** at that client. "This week" is **Sunday to Saturday, the same week Who is Where uses**.
- **Number badge on the pin:** the client's **open tickets**. "Open" means **every Autotask ticket that is not Complete**. A pin with no badge has none. When the counts can't load, no badges show at all (see the banners below).

**Overdue means either of two things:** the newest note is older than the note threshold, **or** the newest meeting is older than the meeting threshold. It is the same rule [Client Touch Aging](client-touch-aging.md) uses, so the two tools never disagree about who is overdue.

## Stacked pins

When several clients share one address, they show as **one pin with ×N** on it. The pin is filled with the **worst state among them**, so a stack with one overdue client reads red. Click it to fan the clients out around the address. If there are more than six, it opens a list instead.

## Clusters

When you zoom out, nearby pins merge into a **count bubble**. Click a bubble to zoom in. A **red dot** on a bubble means at least one overdue or never-touched client is inside it. Grey (not tracked) clients never trigger the dot.

## Quick views

Choose where the map looks with the quick views:

- **Denver metro** (the default)
- **Colorado & Kansas**
- **All clients**

The Hub remembers the last one you used, along with your filters.

## Filters and search

- **Plan-type chips** narrow the map to the plan types you pick.
- **Account manager** shows only that person's clients.
- **Three toggles.** **Overdue for a touch** shows overdue clients and also the never-touched ones. **Has open tickets** shows only clients with at least one open ticket. **Visit this week** shows only clients with a dispatch visit booked this week (the blue ring). A toggle **greys out** while its data is unavailable, and it is treated as off for that load. **Your saved choice is kept**, so it applies again once the data is back.
- **Search** finds a client by name. When you pick one, the map zooms to it.

A result count shows how many clients match what you've chosen.

## When counts or visits are unavailable

Open tickets and dispatch visits load separately, and either can fail on its own. When one does, a banner appears above the map:

- **"Open-ticket counts unavailable right now."** Pins show **no badge** until the count loads. **A missing badge here does not mean zero.** The Has open tickets filter is off until then. **Retry counts** reloads it.
- **"Dispatch visits unavailable right now."** Pins show no visit ring and the Visit this week filter is off. **Retry visits** reloads it.

The legend swaps its open-ticket entry for "Open-ticket badges unavailable" while the counts are down.

## The "couldn't place" list

Some sites can't be put on the map. The **count is always visible**; click it to open a list of the **client, the site and the reason**. The reason tells you what to fix in Autotask:

- **No street address:** the site has nothing to look up.
- **PO box:** a PO box can't be placed on a map.
- **Address not found:** the address couldn't be matched to a location.
- **Low-confidence match:** the closest match wasn't certain enough to trust.
- **Lookup failed:** the address lookup itself had a problem.
- **Not placed yet:** the address hasn't been looked up yet.

Fix the address in Autotask. New or changed addresses are placed **within the hour**.

## The client card

Click a pin to open the client's card:

- **Plan type**, with the real Autotask classification.
- **Sites**, with a switcher when the client has more than one.
- **Address**, with a **Copy** button.
- **Approximate location.** When Azure Maps could only place the pin at street level rather than the building, the card says so. Add a street number to the location in Autotask for an exact pin.
- **Last touched.** Every touch-tracked client (**Total CommITment, TC Light, Block Hours and Co-Managed**) is now part of the Touch Aging note sync, not only clients in the MSC workbook. A client that has only just been covered shows **Not yet synced** until the next sync runs: daily at 5:00 AM Mountain while someone has the Hub open, or an admin's **Check for new data** in Client Touch Aging. The **first sync after this update takes longer**, because it pulls 24 months of notes once for the newly covered clients.
- **Account manager.**
- **Seats:** the number **contracted** in the MSC workbook, **not a live count** of users or devices.
- **Open in Autotask** to jump to the company record.
- **Open tickets.** A count pill, then the client's open tickets, **newest first**. The count is **across all of the client's sites**, because Autotask tickets rarely carry a site. A long list scrolls. Click a ticket to open it in Autotask. With none, it says "No open tickets. Every ticket for this client is Complete."
- **Upcoming visits.** The next 14 days from the dispatch calendar: the date and time (Denver time), who is going, and the linked ticket or task. Each visit is labelled **On-site** or **Type not set**, exactly as Who is Where labels them, and a visit this week carries a **This week** chip. This section loads from dispatch and **can take a while on the office network**.
- **Last on-site visit.** Looks back 12 months at completed on-site calls and shows the date and who went. If there is none it says "No on-site visit in the last 12 months". It has the same known caveat as Who is Where: completing a call can overwrite its on-site status, so a completed visit is only recognised when its description marks it on-site. If the technician lookup fails, it shows **techs unavailable** instead of names.

Each of these three sections loads on its own and shows its own error with **Retry**; the others still render.

## Coming in the next phases

Not built yet: the visit-day route planner. It will be added in the next phase, and this page will be updated when it is.

## If the map won't load

If the map itself can't load, the **client list appears instead**, with the same filters and the same card. Use **Retry map** to try again. If the client load itself fails, you get an error with **Retry**. You will never see a stale map presented as current.

---

*Client Map was Susan Castle's idea (Hub idea #50).*
