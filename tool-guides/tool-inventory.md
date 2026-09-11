# Tool Inventory

Tool Inventory shows, per client, how many seats/devices/users ANS is actually providing them on a handful of vendor platforms, alongside that client's MSC-contracted baseline for comparison — read from a frozen monthly snapshot rather than live every time. It draws no conclusion from what it shows: there is no verdict, no severity, and no dollar figure anywhere on the screen. Actual and contracted sit side by side, and you judge the difference.

Today the report covers six platforms with real counts — **Datto RMM** (devices), **RMM DT&LT** (devices — a breakdown of that same Datto RMM total, showing how many of those devices are desktops and laptops), **RMM Servers, Workstations** (devices — a second breakdown of that same Datto RMM total: every countable RMM device that isn't a network device, i.e. Server + Desktop + Laptop), **Duo Security** (users), **Datto SaaS Protection** (users), and **BitDefender** (devices — the endpoint count GravityZone itself reports for that client) — plus two Yes/No indicators showing whether a client is licensed for a service: **Liongard** and **Lifecycle Manager X**. That last one used to be labeled LifeCycle Insights on screen and in the export; it's the same indicator, renamed to name the product it actually reads — the historical quarterly tabs still call that column LifeCycle Insights.

## Who sees it

Tool Inventory is intended for the Admin, Finance, and Strategic roles, and access is granted in Access Management.

The tool only appears in your sidebar after **two** separate steps, not one:

1. Somebody with admin access grants your role the tool in **Access Management**.
2. You tick **Tool Inventory** in **Sidebar Layout** (bottom-left navigation bar, under Ideas & Bugs) and save.

The second step is easy to miss. A newly added tool lands in an existing person's saved sidebar layout switched off, so even after the grant is in place, Tool Inventory stays hidden until you turn it on yourself. If a colleague says they've been granted access and still can't find it, that's almost always the reason.

## Picking a stored month

The month picker at the top of the status card only lists months that actually have a stored snapshot — there's no live mode here the way Inventory Reports has one. Each option is labeled with the month, how the snapshot was produced (manual or scheduled), and the word "incomplete" if that snapshot has something unread in it.

Beside the picker, four more values describe the month you've selected:

* **Snapshot** — when it was taken and by whom, whether it replaced an earlier snapshot for the same month, and whether it was taken late (after the 1st of the following month). Snapshots are frozen: once taken, the numbers in them never move, even if a client's contract or vendor account changes afterward.
* **Baseline read** — when the MSC workbook was read for this snapshot.
* **Exclusions in force** — how many companies and how many vendor accounts were excluded from this snapshot, for example "120 companies · 5 vendor accounts excluded"; hover the value to see the Autotask IDs and vendor account names behind those counts. A sub-line underneath can add, for example, "5 of them matched an account this snapshot" — how many of the excluded vendor accounts actually matched a real unmapped account when this snapshot was taken. A month taken before that was measured shows no sub-line at all, which is not the same as zero; it's a number that month never counted.
* **In this snapshot** — a one-line summary of totals: how many clients, how many platforms, how many have no MSC record, how many vendor accounts are unmapped, how many baselines couldn't be read, and — only when something actually failed to read — how many reads failed. It does not carry the Snapshot diagnostics count — that number is on the diagnostics tab itself.

## This month is incomplete

A neutral banner appears above the tabs whenever something in the selected snapshot couldn't be read in full — a vendor source failed outright, one or more clients' MSC baselines couldn't be read, or an individual platform read failed for a specific client. It names what went wrong. This is deliberately not styled as a warning or an alert — "incomplete" is a statement about what the snapshot could read, not a judgment about any client. If you see it, the fix is to retake the snapshot once the underlying read problem is resolved; the current, incomplete snapshot is kept either way.

## Client matrix

The default tab: one row per client, one column per platform, in this order: Datto RMM, RMM DT&LT, RMM Servers, Workstations, Duo Security, Datto SaaS Protection, BitDefender, then the two indicators, Liongard and Lifecycle Manager X. The Client, Section, and Contracted baseline columns stay pinned on the left as you scroll the platform columns across. **Find client** filters the list by name or Autotask ID as you type. Click any row to open that one client on its own. The RMM DT&LT and RMM Servers, Workstations columns both sit next to Datto RMM, and both headers read "devices · via Datto RMM" — a reminder that each is a breakdown of the column beside it, not a separate vendor or a separate read. **BitDefender** is different: its header reads plain "devices" with no "via," because it's a genuine second read against GravityZone, not a breakdown of anything else.

BitDefender's number is GravityZone's own current endpoint count for that client. It has no history to fall back on — GravityZone's API only ever answers with the client's *current* count, with no way to ask it for a past month — so a BitDefender read that's skipped or missed for a given month is simply gone for that month; it can't be filled in later the way some other platforms' figures can. One ANS client legitimately shows **No account** for BitDefender rather than a number, because that client runs Microsoft Defender instead of BitDefender — that's not a mapping problem to chase down, just the true state of that account.

The Contracted column shows the client's MSC-contracted baseline as **N users** (for an MRR client) or **N endpoints** (every other section) — always the total, full-time plus part-time combined. A sub-line reading **incl. N part-time** appears underneath only when the MSC workbook actually lists part-time users for that client; otherwise there's no sub-line at all. Two other words can appear in this column instead of a number:

* **Unknown** — this client's MSC baseline couldn't be read for this snapshot. It is not a contract finding — the client may well have a real contracted number, it just isn't known for this month.
* **No MSC record** — the client is mapped to a real Autotask company, and that company has no row in any of the four MSC sections (MRR, TCC, ORR, CoM).

Four things can appear in a platform cell, and they mean different things:

* **A number (including 0)** — a real count a vendor reported for that client. A 0 is a real, vendor-reported zero, not a placeholder.
* **No account** — the vendor's read succeeded, and it simply listed no site or sub-account mapped to this client. This is a fact about the client, not a failed read.
* **Not read** — the read genuinely failed for this client's account in this snapshot.
* **Not in this snapshot** — this column didn't exist in the report yet when the snapshot was taken, so the month simply stored no value for it. It's neither a failed read nor a claim that the client has no account — it's a fact about the report, not the client, and it doesn't make the month read as incomplete on its own (the incomplete banner above is only about reads that were attempted and failed).

A month stored before this rebuild doesn't know the difference between "no account" and "read failed" for Datto RMM, Duo, and Datto SaaS Protection — a snapshot that old shows **Not read** for both. Every snapshot taken from this point forward tells the two apart. The stored 2026-08 snapshot predates RMM DT&LT, RMM Servers, Workstations, and BitDefender all becoming real, so all three of those columns read **Not in this snapshot** for every client on that month — not a failed read for any of them, just three columns the report gained after that month was taken — until 2026-08 is retaken.

## Single client

Opened by clicking a row anywhere in the app (the matrix, or either of the two lists below). The header repeats the client's name, Autotask ID, the MSC name it's mapped to, its MSC section, its contracted baseline, and which snapshot you're looking at — the contracted baseline is one figure for the client as a whole, not a separate number per platform, so it only appears up here. Below that, one table holds two kinds of rows: one row for each of the six counted platforms — Datto RMM, RMM DT&LT (the Desktop-and-Laptop devices inside that same client's Datto RMM total, read from the same source, not a separate vendor), RMM Servers, Workstations (that same client's Server + Desktop + Laptop total out of that same Datto RMM total, likewise not a separate vendor), Duo Security, Datto SaaS Protection, and BitDefender (a genuine second read, against GravityZone) — each showing Platform, Unit, Actual, Vendor account, and Read (a date and time if the read succeeded, **No account** if the vendor listed nothing for this client, **Not read** if the read failed, or **Not in this snapshot** if the platform was added to the report after this month was taken); and one row for each of the two Yes/No indicators, Liongard and Lifecycle Manager X, with the indicator's source vendor shown in the Vendor account column in place of an account name. A **‹ Back** link above the header returns you to whichever list you opened the client from — the matrix, Clients with no MSC record, or Baselines not read.

## Clients with no MSC record

This tab is a real finding about a real, mapped client: it's mapped to an Autotask company, that company has no row anywhere in the MSC workbook, and the client is nonetheless receiving at least one tool this month according to the vendor reads. A client only lands here if their baseline was actually read and came back empty — if the baseline couldn't be read at all, that client is on Baselines not read instead, never here.

The MSC name mapping column is the thing to check first for each row: if the name shown doesn't match what's actually in the MSC workbook, it's a mapping typo to fix in Company Mapping. If the name is right and the client genuinely isn't in the workbook, that's a candidate to either add to the MSC workbook or add to the exclusions list.

## Unmapped vendor accounts

A different kind of item entirely — this is a mapping to-do, not a client. Each row is a vendor site or sub-account (from Datto RMM, Duo, Datto SaaS Protection, or BitDefender) that didn't match any Autotask company at all. Because there's no client behind it, it never appears on the matrix or in either of the other two lists, and its count isn't included in any client's numbers. Not every workbook client bridges automatically to BitDefender this way — GravityZone doesn't share an identifier with Autotask, only a company name, and not every real client's name matches on both sides. An unmatched name is reported here as unmapped, never silently read as a zero for that client.

The **How to map** column says where each one gets fixed, and it's different per vendor: a Datto RMM site is mapped by setting the site's Autotask company id inside Datto itself — never by name. Duo, Datto SaaS Protection, and BitDefender accounts are matched by name instead, so there are two ways one gets placed: the next snapshot picks it up on its own once the account's name in the vendor's portal matches an Autotask company name, or once a confirmed name mapping exists for that company and platform in the Hub's records. There's no screen in the Hub for entering that mapping by hand yet, so an account whose vendor-side name can't be corrected has to wait for a future screen or a direct database entry; either way, the next snapshot is what picks it up. In the meantime, if an unmapped account genuinely has no client behind it at all — one of ANS's own test or internal accounts, for example — it belongs on the exclusions list instead of waiting to be mapped; see Exclusions below.

## Baselines not read

Clients whose MSC baseline genuinely couldn't be read this snapshot — the workbook read failed, or (for a company Company Mapping has never assigned an MSC name at all) there was never anything to look up. Either way, the Contracted column always reads the literal word **Unknown**, never a number and never zero — an unread baseline is not the same as a client having no contract. The vendor counts shown for these clients are still real; only the contracted figure is missing.

## Snapshot diagnostics

Everything the snapshot-taking process couldn't cleanly place, frozen with the month. Sections that have nothing to report simply don't show:

* **Sources that could not be read** — a vendor platform the snapshot couldn't reach at all.
* **Vendor account names that matched no Autotask company** — for Liongard and Lifecycle Manager X specifically; a name that shows up here may just be a stale name sitting in the vendor's own portal.
* **Vendor accounts mapped to a non-client** — an account that maps to something in the Hub's records that isn't an actual client company.
* **Vendor-name mapping collisions** and **MSC workbook collisions** — cases where more than one thing matched the same name.
* **Company metadata errors** — problems reading a company's own information.
* **Accounts resolved through a Kaseya name mapping** — how many of a platform's accounts (SaaS, Liongard, or Lifecycle Manager X) matched a company only through a name mapping recorded for Kaseya, rather than one recorded for that platform itself. Not a problem to fix, just worth knowing where a match actually came from.

Every line here needs a human decision of some kind, but acting on one never changes any client's number in this snapshot — the data is already frozen.

## Exclusions

The **Exclusions…** button, next to Export at the top of the screen, opens a dialog with three sections:

* **Companies excluded by Tool Inventory** — search by name or Autotask ID to add a company, or remove one already on the list.
* **Vendor accounts excluded by Tool Inventory** — vendor accounts (Datto RMM, Duo, Datto SaaS Protection, BitDefender) that have no real client behind them at all, such as ANS's own test or internal accounts. The pick list here is drawn from the unmapped accounts of whichever month is currently loaded — if the dialog says the month isn't loaded, load a month first to see them.
* **Companies excluded in Company Mapping** — read-only here. That Excluded toggle lives on the Company Mapping screen and applies to every tool in the Hub, not just Tool Inventory.

Anyone with access to Tool Inventory can open the dialog and see the current lists, but only a Hub admin can save changes — the Save button only appears for an admin, and everyone else sees a note that these lists are admin-only. Changes take effect starting with the **next** snapshot; a snapshot already stored keeps whichever exclusions were in force when it was taken, so editing the list here never rewrites history. Pressing **Escape** closes the dialog too, unless a save is still in progress.

The intended first real entries for this list are Autotask company 0 (Anchor Network Solutions itself, since its own Datto RMM site holds ANS's own devices) and the handful of ANS test/internal vendor accounts that would otherwise show up as unmapped every month.

## Exporting a month

The **Export** button, at the top of the screen, is enabled once a month is loaded, and its label names the month (for example "Export 2026-08") so it's always clear which month is about to be saved. Clicking it opens a normal save-as dialog; the suggested file name is `Anchor Tool Inventory <month>.xlsx`.

The file has two sheets. The first is named for the month and carries the same eleven columns, in the same name and order, as the existing Anchor Tool Inventory workbook's quarterly tabs — Customer, User Count, RMM, "RMM Servers, Workstations" (a single column, despite the comma in its name), BitDefender, CyberQP Elevate, Splashtop Pro, SaaS, RMM DT&LT, Lifecycle Manager X, and Liongard — with a twelfth appended at the end, Duo Security, since Duo is tracked in the Hub but wasn't on the old spreadsheet's tabs. The Lifecycle Manager X column is the one deliberate exception to "verbatim": the historical quarterly tabs call that same column LifeCycle Insights, and this export knowingly no longer pastes beside them on that one column, because Lifecycle Manager X is the product the Hub actually reads and LifeCycle Insights is its predecessor. RMM DT&LT stays in that same column position (column I), but as of the rebuild that added it, its cells hold a real device count rather than Yes/No — see below. RMM Servers, Workstations and BitDefender have since likewise gained real device counts rather than being left empty — see below.

What lands in each cell:

* A **number** is a real vendor-reported count. **0** is a real zero, not a placeholder.
* An **empty** cell in a count column means the vendor lists no site or sub-account mapped to that client — the same thing "No account" means on screen.
* **Not read** means the read failed for that client in this snapshot.
* **Not in this snapshot** means the column was added to the report after this month's snapshot was taken, so the month stored no value for it — neither a failed read nor an empty "no account" cell.
* The Yes/No indicator columns — **Lifecycle Manager X** and **Liongard** only — read **Yes**, **No**, or **Not read**. RMM DT&LT, RMM Servers, Workstations, and BitDefender are none of them indicator columns; all three are count columns, with the same four cell rules as any other count column above.
* **User Count** is the client's MSC-contracted baseline number (users for an MRR client, endpoints otherwise), or the words **No MSC record** or **Unknown** when it isn't known — same meaning as on screen.
* **CyberQP Elevate** and **Splashtop Pro** are present but always empty — those two platforms aren't read yet. BitDefender's column is a real GravityZone-sourced device count, and RMM Servers, Workstations is a real Server + Desktop + Laptop count broken out of the same Datto RMM pull as RMM DT&LT — both have since left the always-empty group.

The second sheet, **About this export**, records the month, when the snapshot was taken (and by whom), whether it was late, when the MSC baseline was read, how many companies and vendor accounts were excluded, whether any source failed to read, how many individual client reads failed, and a short legend explaining the cell rules above. The export carries no pricing anywhere, by design.

## Taking a snapshot

A snapshot of the most recently closed month (Denver time) is now taken automatically: on the 1st of the following month, starting at 6:10 AM Denver time and retried each hour through that day if a vendor couldn't be reached. From the 2nd of the month onward, the automatic attempt stops for good — after that, only a person can take one, using Take Snapshot Now, and that snapshot is marked **late**.

**Take Snapshot Now**, at the top of the screen, always targets the most recently closed month — never the month you currently have selected in the picker. Any user with access to Tool Inventory can take the **first** snapshot of a month, on time or late. **Replacing** a month that already has a snapshot is different — that needs the Hub admin role, and the button is disabled with an explanation for anyone else. Either way, a replaced snapshot is kept, not deleted, so nothing is lost for audit purposes.

Taking a snapshot reads seven sources live — Datto RMM, Datto SaaS Protection, Duo, BitDefender, the MSC workbook, Liongard, and Lifecycle Manager X — and can take several minutes. (RMM DT&LT and RMM Servers, Workstations aren't separate sources; both are broken out of the same Datto RMM pull as Datto RMM itself.) The button is disabled for the whole time it's running, and you'll see a note explaining that it's in progress. If anything couldn't be read, the new snapshot is still saved, and it's marked incomplete rather than being discarded.

## What is not here yet

* **CyberQP** counts — CyberQP Elevate remains unread. It's not a capability problem, just a scheduling one: it's pending its own later phase.
* **Splashtop** counts — this one is a capability limit, not a scheduling one. Splashtop has no vendor API at all, so there's nothing to read yet; the column stays present and empty until a manual-import approach is decided.
* A mapping screen for Duo, Datto SaaS Protection, and BitDefender vendor accounts. Today those three platforms are matched by name only; an account whose vendor-side name doesn't match an Autotask company name has to wait for a future screen or a direct database entry.
