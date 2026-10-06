# Tool Inventory

Tool Inventory shows, per client, how many seats/devices/users ANS is actually providing them on a handful of vendor platforms, alongside that client's MSC-contracted baseline for comparison — read from a frozen monthly snapshot rather than live every time. It draws no conclusion from what it shows: there is no verdict, no severity, and no dollar figure anywhere on the screen. Actual and contracted sit side by side, and you judge the difference.

Today the report covers nine platforms with real counts — **Datto RMM** (devices), **RMM DT&LT** (devices — a breakdown of that same Datto RMM total, showing how many of those devices are desktops and laptops), **RMM Servers, Workstations** (devices — a second breakdown of that same Datto RMM total: every countable RMM device that isn't a network device, i.e. Server + Desktop + Laptop), **Duo Security** (users), **Datto SaaS Protection** (users), **BitDefender** (devices — the endpoint count GravityZone itself reports for that client), **Cyrisma** (devices — the endpoints Cyrisma lists for that client's tenant), **CyberQP Elevate** (users — the enabled end-user accounts CyberQP holds for that client), and **Splashtop Pro** (devices — the computers Splashtop lists for that client's group, from a person's upload of Splashtop's own computer export) — plus two Yes/No indicators showing whether a client is licensed for a service: **Liongard** and **Lifecycle Manager X**. That last one used to be labeled LifeCycle Insights on screen and in the export; it's the same indicator, renamed to name the product it actually reads — the historical quarterly tabs still call that column LifeCycle Insights. It can also push those counts into each client's Service Plan Calculator — see **Calculators** below.

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

A neutral banner appears above the tabs whenever something in the selected snapshot couldn't be read in full — a vendor source failed outright, one or more clients' MSC baselines couldn't be read, or an individual platform read failed for a specific client. It names what went wrong. This is deliberately not styled as a warning or an alert — "incomplete" is a statement about what the snapshot could read, not a judgment about any client. If you see it, the fix is to retake the snapshot once the underlying read problem is resolved; the current, incomplete snapshot is kept either way. It counts only the clients the report is actually showing, so a client Autotask marks inactive can no longer keep it lit (see **Inactive clients** below) — which is what makes it mean something again.

## Inactive clients

A client Autotask marks **inactive** is not shown anywhere in this report. They're absent from the client matrix, from Clients with no MSC record, from Baselines not read, from the client detail, from every tab count, and from the export. The status card's "In this snapshot" figures count only the clients you can actually see, and a line underneath says how many were hidden — a count, never a list.

This is why: the Hub's company directory keeps a company's row after Autotask deactivates it, on purpose, because those rows are used by other tools and their history matters. Nothing was ever removing them from Tool Inventory's list of clients, so a client ANS parted ways with years ago sat there every month with no MSC name mapped, landed on Baselines not read, and — because that list feeds the "This month is incomplete" banner — kept that banner lit forever, even in a month where every read succeeded. Hiding them is what gives that banner its meaning back.

The status comes from Autotask itself. Nothing in the Hub sets it by hand, and marking a client inactive in Autotask is the only way to move them onto or off this list. It's picked up by the company-directory identity sync, which runs at 3:30am Denver time on whichever machine happens to have the Hub open — so if nobody left it running overnight, the change lands the next time someone does, or straight away if you press **Sync Now** on Company Mapping's Companies tab. Until that sync has run at least once, every company reads as active and the report behaves exactly as it did before, rather than emptying.

The status change and the report are one step apart: a snapshot already taken keeps the clients it was taken with in its own stored rows, but the screen reads the status live, so a client marked inactive today drops off every month you look at, not just the next one.

One thing this deliberately does **not** do: it doesn't stop reading them. Their vendor counts are still collected and still stored in every snapshot, which is what makes the next section possible.

## Inactive clients still using licences

If an inactive client still shows a non-zero count on any platform, an alert appears in a banner above the tabs, naming the client, its Autotask ID, and the per-platform counts that raised it. Mike also gets it as a Teams message.

That's the one thing worth knowing about a client who has left: ANS is still paying a vendor for them. It used to be buried among the 33 rows on Baselines not read, reading as noise.

Some details worth knowing:

* **There's no threshold.** Any non-zero count raises it. A single expensive seat can hide under a threshold.
* **A failed read never raises it, and neither does a real zero.** A read that failed is evidence of nothing, not evidence of a licence.
* **A breakdown column isn't listed twice.** RMM DT&LT and RMM Servers, Workstations come out of the same Datto RMM pull, so the alert names Datto RMM's own count once rather than inviting you to add three numbers that describe the same devices.
* **It's counts only** — no dollar figure, no severity, no verdict, the same as the rest of the screen.
* **Acknowledge** removes a client from the banner for that month. It doesn't change the snapshot, and it doesn't change anything in Autotask; it's a note that you've dealt with it. There's no undo, and there doesn't need to be — the finding is derived from a frozen month, so it can't come back on its own, and if the client turns out to still be active the fix is their Autotask status.
* **It re-raises next month if it's still true.** A licence still being paid for is still a finding, and acknowledging one month shouldn't make it disappear for good. The Teams message is sent at most once per client per month, so re-taking or replacing a month doesn't send it again.

## Client matrix

The default tab: one row per client, one column per platform, in this order: Datto RMM, RMM DT&LT, RMM Servers, Workstations, Duo Security, Datto SaaS Protection, BitDefender, Cyrisma, CyberQP Elevate, Splashtop Pro, then the two indicators, Liongard and Lifecycle Manager X. The Client, Section, and Contracted baseline columns stay pinned on the left as you scroll the platform columns across. **Find client** filters the list by name or Autotask ID as you type. The table scrolls sideways inside its own box, so the scrollbar stays on screen rather than sitting below every client row — and **holding Shift while rolling the wheel** scrolls it left and right, which is worth knowing if your mouse's side-tilt doesn't (some mice report a tilt with no direction attached, which nothing can act on). Click any row to open that one client on its own. The RMM DT&LT and RMM Servers, Workstations columns both sit next to Datto RMM, and both headers read "devices · via Datto RMM" — a reminder that each is a breakdown of the column beside it, not a separate vendor or a separate read. **BitDefender**, **Cyrisma** and **CyberQP Elevate** are different: their headers carry no "via," because each is a genuine separate read — against GravityZone, Cyrisma and CyberQP respectively — not a breakdown of anything else. **Splashtop Pro** is the one separately-fed column whose header does say "via": it reads "devices · via manual import", because nothing is read from Splashtop at all — the count comes from the latest export a person uploaded on the Imports tab (see **Splashtop Pro (manual import)** below). BitDefender and Cyrisma read "devices"; CyberQP reads "users", because what it counts is accounts rather than machines.

BitDefender's number is GravityZone's own current endpoint count for that client. It has no history to fall back on — GravityZone's API only ever answers with the client's *current* count, with no way to ask it for a past month — so a BitDefender read that's skipped or missed for a given month is simply gone for that month; it can't be filled in later the way some other platforms' figures can. One ANS client genuinely runs Microsoft Defender instead of BitDefender, with no real BitDefender account behind it — but because BitDefender still has unmapped vendor accounts in this snapshot (see Unmapped vendor accounts, below), that client's BitDefender cell today reads **Unconfirmed** rather than **No account**: the report can't yet rule out that one of those unmapped accounts is actually this client's. Once every BitDefender account is mapped, that will clear starting with BitDefender's next snapshot — this month's own reading is already frozen, the same as every other number in it, and mapping the account afterward won't change it.

The Contracted column shows the client's MSC-contracted baseline as **N users** (for an MRR client) or **N endpoints** (every other section) — always the total, full-time plus part-time combined. A sub-line reading **incl. N part-time** appears underneath only when the MSC workbook actually lists part-time users for that client; otherwise there's no sub-line at all. Two other words can appear in this column instead of a number:

* **Unknown** — this client's MSC baseline couldn't be read for this snapshot. It is not a contract finding — the client may well have a real contracted number, it just isn't known for this month.
* **No MSC record** — the client is mapped to a real Autotask company, and that company has no row in any of the four MSC sections (MRR, TCC, ORR, CoM).

Five things can appear in a platform cell, and they mean different things:

* **A number (including 0)** — a real count a vendor reported for that client. A 0 is a real, vendor-reported zero, not a placeholder.
* **No account** — the vendor's read succeeded, it simply listed no site or sub-account mapped to this client, and every account on that platform is mapped to some client, so the absence is real. This is a fact about the client, not a failed read.
* **Unconfirmed** — the vendor's read also came back with nothing for this client, but this platform still has one or more vendor accounts that haven't been mapped to any client yet (see Unmapped vendor accounts, below). Until those are mapped, the report can't tell whether one of them actually belongs to this client, so it can't yet claim No account either. This is a fact about what the report currently knows, not about the client — and it's platform-wide, not client-specific: it shows on every client's cell for that platform while any account there is unmapped, because the report has no way to know which client an unmapped account actually belongs to. It clears itself automatically the next time that platform is snapshotted, once every account on it is mapped — nothing needs to be re-run to make that happen. A month that's already been taken keeps whatever reading it started with, the same as every other number in a frozen snapshot, so mapping the account won't change a month you've already looked at.
* **Not read** — the read genuinely failed for this client's account in this snapshot.
* **Not in this snapshot** — this column didn't exist in the report yet when the snapshot was taken (or, for Splashtop Pro, no export had been uploaded yet), so the month simply stored no value for it. It's neither a failed read nor a claim that the client has no account — it's a fact about the report, not the client, and it doesn't make the month read as incomplete on its own (the incomplete banner above is only about reads that were attempted and failed).

A month stored before this rebuild doesn't know the difference between "no account" and "read failed" for Datto RMM, Duo, and Datto SaaS Protection — a snapshot that old shows **Not read** for both. Every snapshot taken from this point forward tells the two apart. RMM DT&LT already carries real device counts on the stored 2026-08 month — it became a counted platform a phase earlier and was already live by the time that month was taken, so its cells there are ordinary numbers, **No account**, or **Unconfirmed**, never **Not in this snapshot**. **RMM Servers, Workstations** and **BitDefender** were added to the report after that same month was first taken, but the stored 2026-08 month has since been retaken: both columns now carry ordinary numbers, **No account**, or **Unconfirmed** cells there too, the same as every other platform. The one exception is **Splashtop Pro**. No Splashtop export has ever been uploaded (the Imports tab did not exist), so every month stored before the first upload — 2026-08 and 2026-09 today — reads **Not in this snapshot** in the Splashtop Pro column, and only an admin **Replace** of such a month would fill it in. Apart from that, no column reads **Not in this snapshot** in those months; the state otherwise shows up only for a column added to the report after some older snapshot was taken and never retaken since.

## Single client

Opened by clicking a row anywhere in the app (the matrix, or either of the two lists below). The header repeats the client's name, Autotask ID, the MSC name it's mapped to, its MSC section, its contracted baseline, and which snapshot you're looking at — the contracted baseline is one figure for the client as a whole, not a separate number per platform, so it only appears up here. Below that, one table holds two kinds of rows: one row for each of the nine counted platforms — Datto RMM, RMM DT&LT (the Desktop-and-Laptop devices inside that same client's Datto RMM total, read from the same source, not a separate vendor), RMM Servers, Workstations (that same client's Server + Desktop + Laptop total out of that same Datto RMM total, likewise not a separate vendor), Duo Security, Datto SaaS Protection, BitDefender (a genuine second read, against GravityZone), Cyrisma (a third, against Cyrisma's own tenant and asset lists), CyberQP Elevate, and Splashtop Pro (counted from the latest manual import rather than read from Splashtop — its Read line says **from export uploaded <date>**) — each showing Platform, Unit, Actual, Vendor account, and Read (a date and time if the read succeeded, **No account** if the vendor listed nothing for this client and every account on that platform is mapped, **Not read** if the read failed, or **Not in this snapshot** if the platform was added to the report after this month was taken, or, for Splashtop Pro, no export had been uploaded before the month was taken); and one row for each of the two Yes/No indicators, Liongard and Lifecycle Manager X, with the indicator's source vendor shown in the Vendor account column in place of an account name. When the vendor listed nothing for a client but the platform still has one or more unmapped vendor accounts, the Actual column itself shows the word **Unconfirmed** in place of a count, and the Read column carries a full sentence explaining why instead of one of the four words above, with a link to review the unmapped accounts. A **‹ Back** link above the header returns you to whichever list you opened the client from — the matrix, Clients with no MSC record, or Baselines not read.

## Clients with no MSC record

This tab is a real finding about a real, mapped client: it's mapped to an Autotask company, that company has no row anywhere in the MSC workbook, and the client is nonetheless receiving at least one tool this month according to the vendor reads. A client only lands here if their baseline was actually read and came back empty — if the baseline couldn't be read at all, that client is on Baselines not read instead, never here. A client Autotask marks inactive never lands here either, whatever their baseline says: they aren't a paying client, so "receiving tools for free" isn't the finding to make about them. If one is still consuming licences, that shows up as its own alert instead.

The MSC name mapping column is the thing to check first for each row: if the name shown doesn't match what's actually in the MSC workbook, it's a mapping typo to fix in Company Mapping. If the name is right and the client genuinely isn't in the workbook, that's a candidate to either add to the MSC workbook or add to the exclusions list.

## Unmapped vendor accounts

A different kind of item entirely — this is a mapping to-do, not a client. Each row is a vendor site, sub-account or tenant (from Datto RMM, Duo, Datto SaaS Protection, BitDefender, Cyrisma, CyberQP or a Splashtop group) that didn't match any Autotask company at all. Because there's no client behind it, it never appears on the matrix or in either of the other two lists, and its count isn't included in any client's numbers. Not every workbook client bridges automatically to BitDefender this way — GravityZone doesn't share an identifier with Autotask, only a company name, and not every real client's name matches on both sides. An unmatched name is reported here as unmapped, never silently read as a zero for that client.

While any row for a platform is still listed here, that platform's **No account** cells read **Unconfirmed** instead — for every client on that platform, not just whichever client an unmapped account turns out to belong to, since the report has no way to know that yet. An unmapped Datto RMM row does the same to RMM DT&LT and RMM Servers, Workstations, since both of those columns are broken out of that same Datto RMM read rather than read on their own. See Client matrix, above.

The **How to map** column says where each one gets fixed, and it's different per vendor. A Datto RMM site is mapped by setting the site's Autotask company id inside Datto itself — never by name — so RMM rows show **Not mappable here** rather than an action: a name mapping for RMM would look like it worked and then quietly never take effect. Duo and BitDefender accounts are matched by name instead, as is a Splashtop group that matches no client, and those rows carry a **Map to a client** action you can use right here (see below). Datto SaaS Protection stays the one deliberate exception: an unmatched Datto SaaS account is meant to resolve through whatever Autotask company that client's Kaseya account already maps to, via the existing Kaseya Org Mapping screen, so it never gets a mapping entry point of its own — giving it one would create a second, competing answer to the same question.

If an unmapped account genuinely has no client behind it at all — one of ANS's own test or internal accounts, for example — it belongs on the exclusions list instead of being mapped; see Exclusions below.

### Mapping an account to a client

Click **Map to a client** on any mappable row, or tick several rows first and use the **Map to a client** button in the selection bar to do them all at once. Either way a panel opens on the right:

* Search by client name **or** Autotask ID — typing part of either narrows the list, so you don't need the exact name to find the client. The list is the Hub's own client directory, not a live Autotask search, so every client offered is one the mapping can actually be saved against.
* **Nothing is ever auto-matched.** You pick the client yourself and the mapping is written explicitly; the Hub never guesses or approximates a name to place an account for you. That's deliberate — a wrong automatic match wouldn't fail visibly, it would quietly attribute one client's licence count to another client, on a screen finance reads.
* Pick the client, and a confirmation names exactly what's about to happen before anything is written.
* **The mapping takes effect from the next snapshot, not the one on screen.** Every snapshot is frozen when it's taken, so mapping an account now never changes a month you've already looked at. The panel and the confirmation both say which month it will first apply to.

If the client you pick already has a different account mapped on that platform, the confirmation says so by name — for platforms that hold only one account per client (Duo, Blackpoint, Cytracom), mapping a new one **replaces** the old one, and anything recorded against the old account doesn't carry over.

Nothing is written until you confirm; **Cancel** closes the panel and changes nothing. The same mappings are visible and editable afterwards in Company Mapping, alongside every other platform.

## Baselines not read

Clients whose MSC baseline genuinely couldn't be read this snapshot — the workbook read failed, or (for a company Company Mapping has never assigned an MSC name at all) there was never anything to look up. Clients Autotask marks inactive are not on this list at all any more; they were most of it. Either way, the Contracted column always reads the literal word **Unknown**, never a number and never zero — an unread baseline is not the same as a client having no contract. The vendor counts shown for these clients are still real; only the contracted figure is missing.

## Snapshot diagnostics

Everything the snapshot-taking process couldn't cleanly place, frozen with the month. Sections that have nothing to report simply don't show:

* **Sources that could not be read** — a vendor platform the snapshot couldn't reach at all.
* **Vendor account names that matched no Autotask company** — for Liongard and Lifecycle Manager X specifically; a name that shows up here may just be a stale name sitting in the vendor's own portal. These two are Yes/No indicators rather than counted platforms, so they never appear on the Unmapped vendor accounts tab — this is the only place they show up, and each row carries the same **Map to a client** action described above, opening the same panel and writing the same mapping.
* **Vendor accounts mapped to a non-client** — an account that maps to something in the Hub's records that isn't an actual client company.
* **Vendor-name mapping collisions** and **MSC workbook collisions** — cases where more than one thing matched the same name.
* **Company metadata errors** — problems reading a company's own information.
* **Accounts resolved through a Kaseya name mapping** — how many of a platform's accounts matched a company only through a name mapping recorded for Kaseya, rather than one recorded for that platform itself. Datto SaaS Protection is the only platform this can apply to now: it's a Kaseya product on the Kaseya invoice, so resolving it through Kaseya's own customer names reflects a real relationship. Liongard and Lifecycle Manager X used to ride that same fallback by naming coincidence and no longer do — each now resolves only through a mapping recorded against itself. Not a problem to fix, just worth knowing where a match actually came from.

Every line here needs a human decision of some kind, but acting on one never changes any client's number in this snapshot — the data is already frozen.

## Exclusions

The **Exclusions…** button, next to Export at the top of the screen, opens a dialog with three sections:

* **Companies excluded by Tool Inventory** — search by name or Autotask ID to add a company, or remove one already on the list.
* **Vendor accounts excluded by Tool Inventory** — vendor accounts (Datto RMM, Duo, Datto SaaS Protection, BitDefender, Cyrisma, CyberQP, Splashtop) that have no real client behind them at all, such as ANS's own test or internal accounts. The pick list here is drawn from the unmapped accounts of whichever month is currently loaded — if the dialog says the month isn't loaded, load a month first to see them.
* **Companies excluded in Company Mapping** — read-only here. That Excluded toggle lives on the Company Mapping screen and applies to every tool in the Hub, not just Tool Inventory.

Anyone with access to Tool Inventory can open the dialog and see the current lists, but only a Hub admin can save changes — the Save button only appears for an admin, and everyone else sees a note that these lists are admin-only. Changes take effect starting with the **next** snapshot; a snapshot already stored keeps whichever exclusions were in force when it was taken, so editing the list here never rewrites history. Pressing **Escape** closes the dialog too, unless a save is still in progress.

The intended first real entries for this list are Autotask company 0 (Anchor Network Solutions itself, since its own Datto RMM site holds ANS's own devices) and the handful of ANS test/internal vendor accounts that would otherwise show up as unmapped every month. **CyberQP lists ANS's own tenant as an ordinary customer** ("Anchor Network Solutions, Inc.", 37 accounts when this was written), so it belongs on this list too — it is not a client, and leaving it unmapped keeps every other client's CyberQP cell reading **Unconfirmed**, since the report cannot rule out that the unmapped account is theirs. Cyrisma has the same situation handled for it automatically, because Cyrisma puts the partner at the root of its own hierarchy where it can be recognised; CyberQP gives no such signal, so this one is a human decision.

## Exporting a month

The **Export** button, at the top of the screen, is enabled once a month is loaded, and its label names the month (for example "Export 2026-08") so it's always clear which month is about to be saved. Clicking it opens a normal save-as dialog; the suggested file name is `Anchor Tool Inventory <month>.xlsx`. The file holds exactly the clients the screen is showing — clients Autotask marks inactive are absent from it, the same as everywhere else.

The file has two sheets. The first is named for the month and carries the same eleven columns, in the same name and order, as the existing Anchor Tool Inventory workbook's quarterly tabs — Customer, User Count, RMM, "RMM Servers, Workstations" (a single column, despite the comma in its name), BitDefender, CyberQP Elevate, Splashtop Pro, SaaS, RMM DT&LT, Lifecycle Manager X, and Liongard — with two appended at the end, Duo Security and Cyrisma, since both are tracked in the Hub but neither was on the old spreadsheet's tabs. The Lifecycle Manager X column is the one deliberate exception to "verbatim": the historical quarterly tabs call that same column LifeCycle Insights, and this export knowingly no longer pastes beside them on that one column, because Lifecycle Manager X is the product the Hub actually reads and LifeCycle Insights is its predecessor. RMM DT&LT stays in that same column position (column I), but as of the rebuild that added it, its cells hold a real device count rather than Yes/No — see below. RMM Servers, Workstations, BitDefender, CyberQP Elevate and Splashtop Pro (via the manual import) have since likewise gained real counts rather than being left empty — see below.

What lands in each cell:

* A **number** is a real vendor-reported count. **0** is a real zero, not a placeholder.
* An **empty** cell in a count column means the vendor lists no site or sub-account mapped to that client, and every account on that platform is mapped to some client — the same thing "No account" means on screen.
* The plain lowercase word **unconfirmed** means the vendor also listed nothing for that client, but this platform still has one or more vendor accounts not yet mapped to any client — so the empty-cell claim above can't be made safely for that platform yet. It's written out as the literal word, never left blank and never 0, so it can't be mistaken for either.
* **Not read** means the read failed for that client in this snapshot.
* **Not in this snapshot** means the column was added to the report after this month's snapshot was taken (or, for Splashtop Pro, no export had been uploaded yet), so the month stored no value for it — neither a failed read nor an empty "no account" cell.
* The Yes/No indicator columns — **Lifecycle Manager X** and **Liongard** only — read **Yes**, **No**, or **Not read**. RMM DT&LT, RMM Servers, Workstations, and BitDefender are none of them indicator columns; all three are count columns, with the same five cell rules as any other count column above.
* **User Count** is the client's MSC-contracted baseline number (users for an MRR client, endpoints otherwise), or the words **No MSC record** or **Unknown** when it isn't known — same meaning as on screen.
* **Splashtop Pro** is a real count column now, with the same five cell rules as every other count column above. A month taken before any Splashtop export had been uploaded reads **Not in this snapshot** in that column. The **About this export** sheet carries a **Splashtop Pro source** row naming the upload that month used — its date, who uploaded it and the file name — or saying that no import had been uploaded when the month was taken, or, for a month where the Hub's stored import could not be read (its cells read **Not read**), "The Splashtop import could not be read when this month was taken". BitDefender's column is a real GravityZone-sourced device count, RMM Servers, Workstations is a real Server + Desktop + Laptop count broken out of the same Datto RMM pull as RMM DT&LT, and **CyberQP Elevate** is a real count of enabled end-user accounts.

The second sheet, **About this export**, records the month, when the snapshot was taken (and by whom), whether it was late, when the MSC baseline was read, how many companies and vendor accounts were excluded, whether any source failed to read, how many individual client reads failed, and a short legend explaining the cell rules above. The export carries no pricing anywhere, by design.

## Taking a snapshot

A snapshot of the most recently closed month (Denver time) is now taken automatically: on the 1st of the following month, starting at 6:10 AM Denver time and retried each hour through that day if a vendor couldn't be reached. From the 2nd of the month onward, the automatic attempt stops for good — after that, only a person can take one, using Take Snapshot Now, and that snapshot is marked **late**.

**Take Snapshot Now**, at the top of the screen, always targets the most recently closed month — never the month you currently have selected in the picker. Any user with access to Tool Inventory can take the **first** snapshot of a month, on time or late. **Replacing** a month that already has a snapshot is different — that needs the Hub admin role, and the button is disabled with an explanation for anyone else. Either way, a replaced snapshot is kept, not deleted, so nothing is lost for audit purposes.

Taking a snapshot reads ten sources — Datto RMM, Datto SaaS Protection, Duo, BitDefender, Cyrisma, CyberQP, the MSC workbook, Liongard, Lifecycle Manager X, and the current Splashtop import — and can take several minutes. Nine are read live from the vendor; the Splashtop import is read from the Hub's own store, because Splashtop is never asked anything (see **Splashtop Pro (manual import)** below). Cyrisma is the cheapest of them: three requests, a few seconds. CyberQP is at the other end, needing roughly one request per client, though they are fast ones. (RMM DT&LT and RMM Servers, Workstations aren't separate sources; both are broken out of the same Datto RMM pull as Datto RMM itself.) The button is disabled for the whole time it's running, and you'll see a note explaining that it's in progress. If anything couldn't be read, the new snapshot is still saved, and it's marked incomplete rather than being discarded.

## Cyrisma

Cyrisma counts **devices** — the endpoints Cyrisma lists for that client's tenant. About 29 of ANS's clients have Cyrisma at all, so most rows will legitimately read "No account".

Two things are worth knowing about this column specifically.

**The number has not been cross-checked against anything.** Most other counts here were validated against the hand-kept quarterly tabs in the Anchor Tool Inventory workbook — that is how BitDefender was shown to count devices rather than users. (CyberQP was put through the same exercise and *failed* it; see **CyberQP Elevate** below.) Cyrisma has never been tracked there (all 48 tabs, 2014 Q3 through 2026 Q2, carry no Cyrisma column), so there is no historical figure to check it against. Until someone compares a handful of clients against the Cyrisma portal by hand, this column rests on Cyrisma's own reporting alone. A useful sanity check in the meantime: a client's Cyrisma count should sit at or below their Datto RMM device count.

**ANS's own machines are deliberately excluded.** Cyrisma puts the partner organisation at the top of its own hierarchy, and that root tenant holds ANS's own endpoints — 31 of them when this was built. They are not a client's devices, so they are not counted and do not appear as an unmapped account either.

A client whose Cyrisma tenant exists but has nothing deployed in it reads a real **0**, not "No account". Those are different facts: "No account" means Cyrisma lists nothing for that client at all, while a 0 means Cyrisma has them and reports no endpoints.

Until each Cyrisma tenant name is mapped to a client, unmapped names will mark the Cyrisma column **Unconfirmed** for everyone without an account — that is the Unconfirmed state working as designed, and it clears on the next snapshot once every name is mapped.

## CyberQP Elevate

CyberQP counts **users** — specifically, the **enabled end-user accounts** CyberQP holds for that client. Disabled accounts are not counted, and neither are admin, service, or just-in-time accounts. CyberQP lists 74 customers in total, so a fair number of rows here will legitimately read "No account".

**This column deliberately does not match the CyberQP invoice**, and that is worth understanding before reconciling anything against it. Two other numbers exist, and neither is what this column shows:

* **The invoice bills a round 2,800 units.** A round number is a purchased commitment, not a measurement — it is what ANS agreed to buy, not what ANS is using. It can't be reproduced from usage data, and this column isn't trying to hit it.
* **CyberQP's portal can export a count of *agents*** — 2,958 when this was checked. An agent is a different thing from an account, and CyberQP's API has no way to list agents at all: it exposes accounts, just-in-time accounts, events, an installer download and an install token, and nothing else. So the agent figure simply isn't reachable by the Hub today. CyberQP have been asked whether they can expose one; if they can, this column can move to it.

**This column could not be validated against the hand-kept quarterly tabs** — and it is worth being precise about how that differs from Cyrisma's situation above, because the two look similar and are not. Cyrisma has no historical column at all, so there is nothing to check it against. CyberQP has one, the check *was* run, and it **failed.** Every candidate was tried — each account type, the plausible combinations, and an enabled-only version of each — and the closest sat about 12% above the historical column, with only around one client in eight matching exactly. The historical column doesn't behave like a clean device count or a clean user count either, which makes it a poor thing to calibrate against. Rather than pick the nearest number and let it look verified, this column reports something **defined and reproducible**, and says plainly what that is.

A client whose CyberQP account exists but has no enabled end users reads a real **0**, not "No account" — those are different facts, the same distinction the Cyrisma column draws.

**One known gap.** CyberQP splits its customer list across three directories, and only one of them responds; the other two time out every time they're asked. All 74 customers come back from the working one, so nothing is known to be missing — but the report can't *prove* the list is complete. A client existing only in one of the two dead directories would read "No account" here rather than showing an error. This is being raised with CyberQP.

**Don't take two snapshots close together — allow a clear 90 minutes.** CyberQP's sign-in is renewed once per run, and CyberQP rate-limits that renewal, so a second snapshot too soon after the first fails the CyberQP read while every other platform reads normally. Measured on 2026-09-14: **5 minutes apart failed, 35 minutes apart also failed, 89 minutes apart succeeded.** It is harmless — the stored sign-in is not consumed or damaged — but the column reads **Not read** for that month and the month is marked incomplete, so a retake taken too eagerly loses a good CyberQP reading you already had. **Make every other correction first, then take one snapshot**, rather than retaking after each fix.

**If CyberQP needs reconnecting**, the column reads **Not read** for everyone and the month is marked incomplete, with the reason named under **Sources that could not be read** on Snapshot diagnostics. CyberQP's sign-in works by storing a credential the Hub swaps for a fresh one on every use, and if that chain is ever broken, someone has to sign in to CyberQP in a browser once to restart it. It is deliberately never reported as a zero: a silent zero in a licence report is a client's count quietly disappearing.

## Splashtop Pro (manual import)

Splashtop has no API, so nothing can read it automatically. Instead, a person exports Splashtop's own computer list and uploads it on the **Imports** tab, the sixth tab on this screen. In Splashtop's console, go to **Management → All Computers**, open the **⋯** menu at the top right and choose **Export…** — you get a file named like `Computer_List-<date>.csv`. Then open the Imports tab, press **Upload…**, and pick that file.

**What counts.** Splashtop Pro counts **devices**: every computer enrolled in a client's Splashtop group, online or not, because enrolment is what Splashtop bills. If the same row appears twice in the file exactly, the second copy is collapsed, and the preview tells you how many were.

**Default Group.** Splashtop keeps its own bucket called Default Group, where machines land after a failed install or when a departed client's group was removed. Those computers belong to no client, so they are never counted toward anyone and never offered for mapping. The preview and the Imports card both list them by name, so someone can clean them up in Splashtop.

**The preview.** After you pick a file, a preview shows how many rows were in it, how many computers were counted in how many groups, and, group by group, how each one will resolve: mapped to a client, matched to a client by name, or **unmapped — will appear on the Unmapped tab unless it has been excluded**. Nothing is saved until you press **Save import**.

**Which import a snapshot uses.** There is one current import. Every snapshot taken after an upload uses the newest one, whatever date the file carries. A month that has already been taken is never changed by a later upload — if the most recently closed month already has a snapshot, the preview says so, and only an admin **Replace** of that month would bring the new import into it (which also re-reads every other vendor as of that day).

**Where the numbers came from.** The Splashtop Pro column header reads **devices · via manual import**, and once a snapshot has used an import it carries a second line underneath, **from export uploaded <date>**. A client's **Read** line shows the same **from export uploaded <date>**, and the export's About sheet carries the same information (see Exporting a month). Until the first upload, the Imports tab reads "No Splashtop Pro export has been uploaded yet. Until one is, the Splashtop Pro column reads Not in this snapshot." and the column does read **Not in this snapshot**.

**Who can upload.** Anyone with access to Tool Inventory. The Imports card shows who uploaded the current import and when, and earlier imports stay listed underneath it.

**Where the file goes.** The file is first archived to SharePoint, under **ANS-Vendors / Splashtop / Computer Exports / <year>**, named with the upload date and time followed by the original file name. If the archive fails, nothing is imported — the file is the record of where the number came from, so no number is stored without it. The Imports card links to the archived copy.

**What gets refused.** A file that isn't a .csv, a file that isn't Splashtop's computer export, a file with a header but no computers in it, and a file over 5 MB, and a file in which every computer is in Default Group or has no group (so there is nothing to import — assign them to client groups in Splashtop first) each get a plain message, and nothing is archived or stored.

**Mapping.** A Splashtop group that matches no client appears on the **Unmapped** tab with the usual **Map to a client** action. ANS's own group is not a client: exclude it once, the normal way, under Exclusions.

## Calculators

Each managed client has a *Service Plan Pricing Calculator* workbook in their SharePoint folder, and its first sheet lists the tools in that client's plan with a **Units** column that drives the pricing below it. Those Units used to be typed by hand from the quarterly spreadsheet Tool Inventory replaced, so they went stale. The **Calculators** tab pushes the latest snapshot's counts into those Units cells for you — on demand, never automatically.

**Only a Hub admin can run it.** Anyone with Tool Inventory can open the tab and read the results of the last run.

**How a run goes.** The tab always works from the newest snapshot (named at the top). **Preview changes** reads every client's calculator and shows, per client, which sheet it will use, how many Units cells would change and how many it will skip; open a client to see every tool row with its current value, its new value and what will happen to it. Untick any client you'd rather leave alone, change a client's sheet if the default is wrong, then **Confirm**. Confirm applies to the ticked clients the **Find a client** box is showing, so to update one client, type its name and confirm; **Tick all shown** and **Untick all** sit beside the button. The run walks the clients one at a time, re-reading each file and writing only what still differs, and the results table fills in as it goes. You can leave the tab; the run keeps going and the tab picks it back up when you return. A client that fails never stops the others.

**What is written, and what never is.** Only the Units cells of the tool rows the Hub knows — Datto RMM, Managed Antivirus, Privileged Account Mgmt (PAM), Splashtop Professional, Liongard and LifeCycle Insights (the last two as 1 or 0), plus Datto SaaS Protection for clients whose plan includes it — and a one-line note in cell F5 saying when the Hub updated the counts, from which snapshot, and who ran it. The **DUO MFA** row is never written: Duo is billed separately, so the preview shows it as *skipped — billed separately* and the cell is left exactly as it is. Prices, costs, margins, the user count, the Security+ sheet and every other cell are never touched, never read into the Hub, and never shown. A Units cell that holds a formula (new-client estimates do) is replaced with the real count, and the preview says so.

**What writes and what skips.** A real count writes. A client with no account on a platform writes **0** — but only when every account on that platform is mapped to a client, so the absence is proven; the preview labels these *write 0 — proven absence*. A platform that reads *Unconfirmed*, *Not read* or *Not in this snapshot* is skipped and its cell is left alone. A tool row the calculator doesn't have is skipped too; the Hub never adds rows. One exception to the proven-absence rule: the **PAM** row is never set to 0 from a "no account" reading, because CyberQP's AD and local directories can't be read by the Hub and a client that lives only there looks absent when it isn't — that row changes only on a real count and otherwise reads *skipped — absence can't be proven*.

**Datto SaaS Protection follows the plan, not the vendor count.** Some clients have SaaS included in their plan and some don't, and the Kaseya Invoice Processor already records which (its *bundled* flag and quantity override, kept in the Hub directory). The write-back reads the same record: for a client whose plan includes SaaS, the Units are the processor's bundled quantity — the override if one is set, otherwise that client's Included Users from the MSC — so both tools work from one record of what is bundled. The Included Users figure is the one captured in the snapshot the run works from (the override is read as it is today), so a user-count change made after the snapshot reaches the calculator with the next month's run. A client whose plan does not include SaaS is *skipped — not in this client's plan* and the cell is left alone. A client marked as included but with no user count known is *skipped — no included-user count* until the MSC row or the override supplies one.

**Cannot be served.** Some clients are listed under *Cannot be served* with a reason: no SharePoint folder set in Company Directory, no calculator file in the folder, two calculator files side by side (an old plan's left beside the new — rename the old one with an **ARCHIVE_** prefix and the Hub ignores it from then on), a file named differently from the usual *Client Service Plan Calculator.xlsx*, no plan sheet, a layout the Hub doesn't recognise (the header isn't where it should be, a tool is listed twice, there is no Total row below the tools, or none of the eight tool rows is on the sheet), or something already in F5 that isn't a Hub note. The Hub doesn't guess around any of these — the list is the cleanup list, and the fix happens in SharePoint or Company Directory.

**Which sheet.** Block Hour clients use the *TC Core* sheet; TC, TC+, TC-Lite, Legacy and Needs Some Love! clients use *Total CommITment*; Co Managed clients use *Co-Managed* when the workbook has one, otherwise *Total CommITment*. A client whose classification gives no default (or whose usual sheet isn't in the workbook) isn't refused: it appears in the main preview table with a **Pick a sheet** dropdown, and nothing is written for it until you pick one. Sheets whose names start with *Old* are never offered.

**Running it again is safe.** A second run against the same snapshot finds nothing to change, writes nothing and adds no new note.

## What is not here yet

* Reminders when a new snapshot is ready for a calculator update — a later alerting phase.
* Reminders to the people who should upload before the 1st — a later alerting phase.
* Nothing further on mapping. Duo, BitDefender, Splashtop groups, Liongard and Lifecycle Manager X vendor accounts are all mappable from the screen now — see **Mapping an account to a client** above, and the same action on Snapshot diagnostics for the two indicators. Datto SaaS Protection deliberately has no mapping entry point of its own and never will: it resolves through whatever Autotask company that client's Kaseya account already maps to. Datto RMM has none either, for a different reason — it matches on the Autotask ID held inside Datto, so a name mapping would silently never take effect.

*Tool Inventory, the Splashtop manual import, and the Service Plan Calculator write-back were Mike Stewart's ideas.*
