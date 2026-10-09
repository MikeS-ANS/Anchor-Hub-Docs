# Connected Services

Anchor Hub saves you from logging into a dozen vendor portals. Instead of opening Autotask, Pax8, Duo, Meraki and the rest one by one, the Hub talks to them for you and brings what you need into one place.

This page lists every outside service the Hub talks to, what it does there, and whether it only **reads** information or can also **make changes**.

## Two ways the Hub connects

### "As you" — Microsoft 365

When you sign in to the Hub with your Microsoft account, everything it does in Microsoft 365 is done **as you**. It can only see what you can already see: your own mail, your own calendar, your own files, and the SharePoint folders you already have access to. If you can't open something in SharePoint yourself, the Hub can't open it for you either.

### "The Hub's own access" — a shared connection

For most other vendors, the Hub holds one shared connection that it uses on behalf of everyone. Either the Hub's server makes the call for you, or the Hub fetches the connection securely at the moment it's needed. The credentials are never stored inside the app, and you won't see them in normal use — you never have to paste in a vendor key yourself.

## Your own Autotask key

There is one exception, and it matters: **anything the Hub writes to Autotask is written with your own Autotask API key**, saved on your machine in Settings (see [Configuring Your Settings](configuring-settings.md)). That means any change the Hub makes in Autotask — a contract update, a note, a time entry — shows up **under your name**, exactly as if you had made it yourself. Without your own key you can still view Autotask information in the Hub; you just can't push changes.

## What the Hub talks to

### Microsoft 365

- **Microsoft 365 (as you)** — signs you in, shows your photo, your calendar on Home, and the SharePoint files each tool works from (invoices, reports, workbooks). It **reads and saves files** in SharePoint folders you already have access to, and **sends email from your mailbox** when you click send on something like a client report or a bug report — and, for Hub admins who switch it on, the monthly Tool Inventory calculator preview email and the Fractional IT Leadership report, which go out automatically from that admin's mailbox. Daily Progress, only if you use it, also reads your own sent mail, calendar and Teams chats to build your personal report. Timesheet Review looks up who reports to you. When a Hub admin runs Tool Inventory's calculator update, it also **updates the tool-count cells** in each client's Service Plan Calculator workbook in SharePoint — as that admin, after they've previewed every change, or automatically each month when a Hub admin switches that on (the first admin machine open after the month's snapshot lands does the update and emails what changed).
- **Microsoft 365 (the Hub's own access)** — the Hub's server has a small amount of its own access: it looks up staff job titles and whether an account is still active (for access management), pulls Microsoft's daily Teams activity totals for Daily Progress, and reads one billing spreadsheet for Tool Inventory. **Reads only.**

### Autotask & billing

- **Autotask** — the Hub reads companies, contracts, tickets, projects, time and more from Autotask all over the app. With **your own key**, some tools also **make changes**: contract service quantities, services, contacts, notes, projects, tasks, configuration items, tickets and time entries. The Hub's shared Autotask connection is **read-only**.
- **Pax8** — reads subscriptions and invoices for the Pax8 and M365 tools. **Reads only.**
- **QuickBooks** — reads accounting figures for the tools that need them. **Reads only** for now; it's currently connected to a practice company while the real connection is set up.

Some invoice processors (Kaseya, Cytracom and Duo invoices) don't connect to that vendor at all — they work from the invoice file saved in SharePoint, plus Autotask.

### Security & identity

- **Duo** — Duo Management reads clients' Duo setup and **can also make changes**: adding or updating admins, phones, users, client accounts and integrations. Tool Inventory also reads Duo user counts.
- **Blackpoint** — reads endpoint usage for the BlackPoint usage tab. **Reads only.**
- **BitDefender** — reads device counts for Tool Inventory. **Reads only.**
- **CyberQP** — reads counts for Tool Inventory. **Reads only.**
- **Cyrisma** — reads information for Tool Inventory. **Reads only.**
- **Splashtop** — not connected at all (Splashtop has no API). Tool Inventory counts it from a computer list a person exports from Splashtop and uploads on its Imports tab; the file is kept in SharePoint. **No connection.**

### Monitoring & networking

- **Datto RMM** — reads device information for the Hub's tools and Tool Inventory. It can also **run one specific job on a device**: installing the Duo agent.
- **Datto SaaS Protection** — reads backup counts for Tool Inventory. **Reads only.**
- **Meraki** — reads license status; admins can also **add and remove Meraki admins and move licenses**.
- **Liongard** — reads information that Tool Inventory uses to flag things worth a look. **Reads only.**
- **ScalePad** — reads information that Tool Inventory uses to flag things worth a look. **Reads only.**

### Maps

- **Azure Maps** — draws the Client Map, turns client site addresses into map pins, and works out visit-day routes. Client business addresses (and a start address you type for a route, used once and never kept) go to Microsoft's map service in our own Azure subscription. **Reads only.**

### Team & AI

- **Strety** — reads Rocks and To-Dos so you can see them in the Hub. **Reads only.**
- **Claude (Anthropic)** — writes the AI summaries and lines in Pax8 Invoice Comparison, Values & Norms and Daily Progress, through the Hub's own server. Other AI features still use Hatz.ai for now.
- **Hatz.ai** — powers the Hub's other AI features (summaries, drafts, answers) that haven't moved to Claude yet. See the AI note below.
- **Microsoft Teams notifications** — the Hub's own Teams bot sends you messages, such as payroll reminders and Ideas & Bugs updates, and posts the daily Who is Where briefing to its channel. It only sends; it doesn't read your chats.
- **Document text-reading (Project Analysis)** — when you run Project Analysis on a document, the document is sent to Microsoft's document-reading service to pull the text out of it.

### CIPP

- **CIPP** — reads Microsoft 365 information about clients for User Audit and Company Mapping. **Reads only.**

### App updates

- **GitHub** — the Hub checks here for new versions of itself and downloads them. **Reads only.**

### Help

- **This Help Center** — the Help screen, and the round **?** at the top right of the window, show these pages inside the Hub. It's a public site, so the Hub sends no sign-in or account details to it. **Reads only.**

## A note on AI

Features that use **Claude (Anthropic)** or **Hatz.ai** send the relevant data to them so they can write the summary, draft or answer you asked for. Where a tool's guide has an **AI Prompt** section, it shows the instructions the Hub gives the AI. The Hub keeps a record of each AI request — which feature, how much text went in and out (counted in tokens), and who asked — never the text itself.

## Questions?

If you have a question about what the Hub connects to, or about something on this page, ask **Mike**.

---

*This was Mike Stewart's idea.*
