# Tool Inventory

Tool Inventory shows, per client, how many seats/devices/users ANS is actually providing them on a handful of vendor platforms, alongside that client's MSC-contracted baseline for comparison — read from a frozen monthly snapshot rather than live every time. It draws no conclusion from what it shows: there is no verdict, no severity, and no dollar figure anywhere on the screen. Actual and contracted sit side by side, and you judge the difference.

Today the report covers three platforms with real counts — **Datto RMM** (devices), **Duo Security** (users), and **Datto SaaS Protection** (users) — plus three Yes/No indicators showing whether a client is licensed for a service: **RMM DT&LT**, **Liongard**, and **LifeCycle Insights** (this last one reads from Lifecycle Manager X; the column is labeled LifeCycle Insights on screen).

## Who sees it

Tool Inventory is intended for the Admin, Finance, and Strategic roles, and access is granted in Access Management.

The tool only appears in your sidebar after **two** separate steps, not one:

1. Somebody with admin access grants your role the tool in **Access Management**.
2. You tick **Tool Inventory** in **Settings → Sidebar Layout** and save.

The second step is easy to miss. A newly added tool lands in an existing person's saved sidebar layout switched off, so even after the grant is in place, Tool Inventory stays hidden until you turn it on yourself. If a colleague says they've been granted access and still can't find it, that's almost always the reason.

## Picking a stored month

The month picker at the top of the status card only lists months that actually have a stored snapshot — there's no live mode here the way Inventory Reports has one. Each option is labeled with the month, how the snapshot was produced (manual or scheduled), and the word "incomplete" if that snapshot has something unread in it.

Beside the picker, four more values describe the month you've selected:

* **Snapshot** — when it was taken and by whom, and whether it replaced an earlier snapshot for the same month. Snapshots are frozen: once taken, the numbers in them never move, even if a client's contract or vendor account changes afterward.
* **Baseline read** — when the MSC workbook was read for this snapshot.
* **Exclusions in force** — how many companies were excluded from this snapshot; hover the value to see their Autotask IDs.
* **In this snapshot** — a one-line summary of totals: how many clients, how many platforms, how many have no MSC record, how many vendor accounts are unmapped, and how many baselines couldn't be read. It does not carry the Snapshot diagnostics count — that number is on the diagnostics tab itself.

## This month is incomplete

A neutral banner appears above the tabs whenever something in the selected snapshot couldn't be read in full — a vendor source failed outright, one or more clients' MSC baselines couldn't be read, or an individual platform read failed for a specific client. It names what went wrong. This is deliberately not styled as a warning or an alert — "incomplete" is a statement about what the snapshot could read, not a judgment about any client. If you see it, the fix is to retake the snapshot once the underlying read problem is resolved; the current, incomplete snapshot is kept either way.

Expect this banner on every month for now: for the same reason **Not read** can't yet be told apart from "no account at that vendor," a client who simply isn't on a platform counts as an unread row, so essentially every snapshot is marked incomplete until that distinction exists.

## Client matrix

The default tab: one row per client, one column per platform. The Client, Section, and Contracted baseline columns stay pinned on the left as you scroll the platform columns across. **Find client** filters the list by name or Autotask ID as you type. Click any row to open that one client on its own.

Four things can appear in a platform cell, and they mean different things:

* **A number (including 0)** — a real count a vendor reported for that client. A 0 is a real, vendor-reported zero, not a placeholder.
* **Not read** — the vendor didn't return anything for this client's account in this snapshot. This is not the same as a 0. It also does not mean an account exists and failed: a client who has no account at that vendor at all reads **Not read** too, because the snapshot doesn't yet tell "no account" apart from "read failed." In practice most Not read cells are the first of those — a client who simply isn't on that platform.
* **Unknown** (appears in the Contracted column) — this client's MSC baseline couldn't be read for this snapshot. It is not a contract finding — the client may well have a real contracted number, it just isn't known for this month.
* **No MSC record** (also in the Contracted column) — the client is mapped to a real Autotask company, and that company has no row in any of the four MSC sections (MRR, TCC, ORR, CoM).

## Single client

Opened by clicking a row anywhere in the app (the matrix, or either of the two lists below). The header repeats the client's name, Autotask ID, the MSC name it's mapped to, its MSC section, its contracted baseline, and which snapshot you're looking at. Below that, one table holds two kinds of rows: one row per counted platform (Platform, Unit, Actual, Contracted, Vendor account, and whether/when it was read), and one row per Yes/No indicator, with the indicator's source vendor shown in the Vendor account column in place of an account name.

## Clients with no MSC record

This tab is a real finding about a real, mapped client: it's mapped to an Autotask company, that company has no row anywhere in the MSC workbook, and the client is nonetheless receiving at least one tool this month according to the vendor reads. A client only lands here if their baseline was actually read and came back empty — if the baseline couldn't be read at all, that client is on Baselines not read instead, never here.

The MSC name mapping column is the thing to check first for each row: if the name shown doesn't match what's actually in the MSC workbook, it's a mapping typo to fix in Company Mapping. If the name is right and the client genuinely isn't in the workbook, that's a candidate to either add to the MSC workbook or add to the exclusions list.

## Unmapped vendor accounts

A different kind of item entirely — this is a mapping to-do, not a client. Each row is a vendor site or sub-account (from Datto RMM, Duo, or Datto SaaS Protection) that didn't match any Autotask company at all. Because there's no client behind it, it never appears on the matrix or in either of the other two lists, and its count isn't included in any client's numbers.

The **How to map** column says where each one gets fixed, and it's different per vendor: a Datto RMM site is mapped by setting the site's Autotask company id inside Datto itself — never by name. Duo and Datto SaaS Protection accounts are matched by name instead, so there are two ways one gets placed: the next snapshot picks it up on its own once the account's name in the vendor's portal matches an Autotask company name, or once a confirmed name mapping exists for that company and platform in the Hub's records. There's no screen in the Hub for entering that mapping by hand yet, so an account whose vendor-side name can't be corrected has to wait for a future screen or a direct database entry; either way, the next snapshot is what picks it up.

## Baselines not read

Clients whose MSC baseline genuinely couldn't be read this snapshot — the workbook read failed, or (for a company Company Mapping has never assigned an MSC name at all) there was never anything to look up. Either way, the Contracted column always reads the literal word **Unknown**, never a number and never zero — an unread baseline is not the same as a client having no contract. The vendor counts shown for these clients are still real; only the contracted figure is missing.

## Snapshot diagnostics

Everything the snapshot-taking process couldn't cleanly place, frozen with the month. Sections that have nothing to report simply don't show:

* **Sources that could not be read** — a vendor platform the snapshot couldn't reach at all.
* **Vendor account names that matched no Autotask company** — for Liongard and Lifecycle Manager X specifically; a name that shows up here may just be a stale name sitting in the vendor's own portal.
* **Vendor accounts mapped to a non-client** — an account that maps to something in the Hub's records that isn't an actual client company.
* **Vendor-name mapping collisions** and **MSC workbook collisions** — cases where more than one thing matched the same name.
* **Company metadata errors** — problems reading a company's own information.

Every line here needs a human decision of some kind, but acting on one never changes any client's number in this snapshot — the data is already frozen.

## Taking a snapshot

**Take Snapshot Now**, at the top of the screen, always targets the most recently closed month (in Denver time) — never the month you currently have selected in the picker. If that month already has a snapshot, taking a new one replaces it; the replaced snapshot is kept, not deleted, so nothing is lost for audit purposes.

Taking a snapshot reads six sources live — Datto RMM, Datto SaaS Protection, Duo, the MSC workbook, Liongard, and Lifecycle Manager X — and can take several minutes. The button is disabled for the whole time it's running, and you'll see a note explaining that it's in progress. If anything couldn't be read, the new snapshot is still saved, and it's marked incomplete rather than being discarded.

## What is not here yet

* Exporting any of this to Excel.
* A screen for changing which companies are excluded from a snapshot — exclusions can currently only be set outside the app.
* An automatic monthly snapshot — every snapshot today is taken by hand with Take Snapshot Now.
* BitDefender, CyberQP, and Splashtop counts — nothing for these three platforms appears on screen yet.
