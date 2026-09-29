# QuickBooks Connection

The Hub now holds a live connection to QuickBooks Online. It exists so the Hub can read QuickBooks figures itself, instead of someone typing them in from QuickBooks by hand.

In this version the Hub only **reads** QuickBooks. **Nothing is ever written to QuickBooks** — no journal entries, no transactions, nothing. The only thing it reads today is the figure behind Inventory Reports' QB total (see the Inventory Reports guide).

The connection is held on the Hub server (Azure Functions), never on anyone's computer. No copy of the Hub, on any machine, ever holds a QuickBooks token.

## Current status

**As of 2026-09-29, the Hub is not connected to the real ANS QuickBooks company.**

The integration was built and tested end to end against Intuit's test company (a sandbox Intuit provides for developers) on 2026-09-25, then deliberately disconnected. Two things have to happen before the real company is connected:

* **Intuit has to approve the Anchor Hub app for production use.**
* **An in-person session with Heather has to confirm which QuickBooks figure each Inventory Reports tab should read.** The On Hand figure is settled: the Balance Sheet Inventory Asset balance at month end. The Added and Picked figures are not yet.

Until go-live, the **Get from QuickBooks** button in Inventory Reports is either absent or disabled, and the QB total field keeps working exactly as before — you type it in by hand.

## Where to find it and who sees it

Settings → API & Accounts → a card titled **QuickBooks Online**. While the Hub points at Intuit's test company, the title carries "· test company (sandbox)" after it.

Only people who have been given the **QuickBooks Connection** permission in Access Management see the card. It is a system permission granted to named people one at a time — never to a role. Today that is Mike and Heather. Everyone else's Settings page simply has no card there.

The server is the gate, not the screen: someone who somehow reached the card's routes without the permission would get nothing back.

Note the split: connecting, reconnecting, disconnecting and listing accounts need the QuickBooks Connection permission. *Using* the connection — the Get from QuickBooks button — is available to anyone who can use Inventory Reports, because that button only reads one specific figure.

## What the card shows

The card's own hint reads: "The connection is held on the Hub server, not on this computer. Only people given the QuickBooks Connection permission see this card."

**Not connected.** "Not connected. Inventory Reports can't pull QuickBooks figures until someone connects. You'll sign in to QuickBooks in your browser as a QuickBooks admin." If it was disconnected on purpose, it says so. The button is **Connect QuickBooks**.

**Connected.** The card shows:

* "Connected to <company name> by <person> on <date and time>."
* "Intuit's connection limit ends <date>. Reconnect before then."
* The result of the last daily check: "Last daily check: <when> — OK", or the reason it failed.
* "QuickBooks reads this month: N of 50,000 (Hub safety cap)."

The buttons are **Reconnect**, **Disconnect** and **Show QuickBooks accounts**. If the card couldn't load the status at all, it shows the problem and a **Retry** button.

## Connecting

1. Click **Connect QuickBooks** (or **Reconnect**). The Hub opens Intuit's sign-in in your browser.
2. Sign in as a QuickBooks admin and choose the Anchor company.
3. Go back to the Hub. The card watches for the connection to finish and updates itself — you'll see "Connected."

If the card doesn't see the connection finish, it says: "Didn't see the connection finish. If you completed it in the browser, reopen Settings to check."

Two safety checks are built in:

* **The connection is pinned to one QuickBooks company.** If you reconnect and choose a different company, the Hub refuses, says which company it is connected to, and leaves the existing connection exactly as it was.
* **A sign-in page can only be used once, and only for 10 minutes.** If you refresh that browser page or press Back, it says the link has already been used and does nothing a second time. If the link is older than 10 minutes, go back to the Hub and click Connect again.

## Show QuickBooks accounts

Lists each QuickBooks account's name, type, QuickBooks id and whether it is Active — names and ids only, **never balances**. It exists so the right account can be identified when setting up which QuickBooks figure each Inventory Reports tab reads.

## Disconnect

Clicking **Disconnect** asks: "Disconnect QuickBooks? Inventory Reports will stop being able to pull QuickBooks figures until someone reconnects. This also cancels the connection at Intuit."

Afterwards the card says "Disconnected." If Intuit didn't confirm the cancellation, it says: "Disconnected in the Hub. Intuit didn't confirm the cancellation, but the Hub no longer uses the connection." **Reconnect** restores it.

## Needs reconnecting

If the connection breaks, the card says **Needs reconnecting** along with the reason, and Inventory Reports says "QuickBooks isn't connected — ask Mike or Heather."

The most common cause is someone disconnecting the Anchor Hub app from inside QuickBooks itself (Apps → Manage). The Hub notices the next time it tries to use the connection, and at the daily check. The fix is **Reconnect** on the card.

The Hub does not give up on its stored connection just because one refresh was turned down — it looks again first, and only marks the connection as needing to be reconnected once QuickBooks has clearly refused it.

## Daily health check and Teams alerts

Every day at 13:30 UTC (about 7:30 AM Denver time in summer, 6:30 AM in winter) the Hub checks the connection with one small read of the company's details. That read also keeps the connection fresh, so a connection nobody has used for a while is still exercised daily.

The Hub bot sends a Teams message to everyone who holds the QuickBooks Connection permission when:

* **The connection needs reconnecting** — "QuickBooks needs reconnecting".
* **Intuit's five-year limit is 60, 30 or 7 days away** — "QuickBooks connection ends in N days".
* **QuickBooks reads reach 80% of the Hub's monthly cap** — "QuickBooks reads at N% of the Hub's monthly cap".

Each alert is sent once per person per day or threshold — not repeatedly. Each message says where to go to fix it (Settings → API & Accounts → QuickBooks Online).

## The five-year limit

Intuit ends every connection five years after it was made, no matter what. The card shows the end date, and the alerts above warn you at 60, 30 and 7 days. Reconnecting starts a fresh five years.

## The read cap

The Hub limits itself to **50,000 QuickBooks reads per month**. It's a safety cap, well under Intuit's own free allowance; normal use is a few dozen a month, so a high number means something may be looping. At the cap, pulls are refused with "QuickBooks read limit reached for this month (Hub safety cap) — tell Mike." and nothing else happens until the next month. The card shows the running count.

## Test company and the real company

While the Hub points at Intuit's test company:

* The card title says "· test company (sandbox)".
* The Inventory Reports button reads **Get from QuickBooks (test company)**.
* Every pulled figure's detail starts with "Sandbox test company · ", so a test figure can never be mistaken for a real one.

See **Current status** above for where the real-company connection stands.

---

*This was Hub idea #42, "QuickBooks Online API Integration", submitted in Ideas & Bugs by Mike Stewart on 2026-09-08 — the aim being to stop hand-typing QuickBooks numbers into Hub tools, and eventually to write to QuickBooks with a confirmation gate on every write. That write side is a later phase, not this one.*
