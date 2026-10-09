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
- **Three toggles.** **Overdue for a touch** shows overdue clients and also the never-touched ones. **Has open tickets** and **Visit this week** arrive with the next phase.
- **Search** finds a client by name. When you pick one, the map zooms to it.

A result count shows how many clients match what you've chosen.

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
- **Last touched.**
- **Account manager.**
- **Seats:** the number **contracted** in the MSC workbook, **not a live count** of users or devices.
- **Open in Autotask** to jump to the company record.

## Coming in the next phases

Not built yet: open-ticket badges and lists, dispatch visits on the card and the pin, and a visit-day route planner. They will be added in later phases, and this page will be updated when they are.

## If the map won't load

If the map itself can't load, the **client list appears instead**, with the same filters and the same card. Use **Retry map** to try again. If the client load itself fails, you get an error with **Retry**. You will never see a stale map presented as current.

---

*Client Map was Susan Castle's idea (Hub idea #50).*
