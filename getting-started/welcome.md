# Welcome to Anchor Hub

Anchor Hub is Anchor Network Solutions' internal platform for managing recurring billing, client contracts, and vendor integrations. It connects Pax8, Autotask PSA, Kaseya/Datto, Blackpoint, Duo, and other platforms so the team spends less time reconciling spreadsheets and more time with clients.

## Home Screen

When you open Anchor Hub you land on the **daily start page**:
- **Quick Links** — one-click tiles to your most-used portals (Autotask, IT Glue, RMM, M365, Pax8). Admins manage the base set; each user can add personal links
- **Today's Calendar** — your Outlook calendar events for the day
- **Autotask Tickets** — a live snapshot of your assigned tickets with critical/high count, overdue count, and your top 5 by priority

All tools are accessible from the sidebar or via the **Apps** nav item.

---

## Tools

### Billing & Invoice Processing

| Tool | What it does |
|---|---|
| **Kaseya Invoice Processor** | Loads Kaseya/Datto invoices from SharePoint, generates QuickBooks journal entries, and pushes Workplace and SaaS seat counts to Autotask contracts |
| **Pax8 Invoice Processor** | Loads a Pax8 invoice, breaks down costs by service (Azure, Nerdio, Exclaimer, etc.), and pushes updated pricing and seat counts to Autotask contracts |
| **Pax8 Invoice Comparison** | Compares two Pax8 invoices side-by-side to surface month-over-month changes |
| **Subscription Audit** | Compares active Pax8 subscriptions against Autotask contract services and flags quantity mismatches |

### Financial Analysis

| Tool | What it does |
|---|---|
| **Margin Analyzer** | Reviews Azure partner costs vs. billed prices across all clients |
| **Project Profitability** | Analyzes time-tracked project labor against billed amounts |
| **Project Time Summary** | Summarizes Autotask time entries by project and resource for a given period |
| **MSC Agreements** | Manages Managed Service Client agreement data and reporting |

### Contract Management

| Tool | What it does |
|---|---|
| **Contract Changes** | Tracks changes to Autotask contract services over time |
| **Contract Renewals** | Surfaces upcoming contract renewals and helps manage the renewal workflow |

### Vendor & Security

| Tool | What it does |
|---|---|
| **Blackpoint Usage** | Compares Blackpoint billing against Autotask contracts and pushes seat count updates |
| **Duo Management** | Manages Duo Security sub-accounts, applications, and server enrollment |

### Foundation

| Tool | What it does |
|---|---|
| **Company Mapping** | Links Pax8, Kaseya, and Blackpoint company names to their Autotask counterparts — the centralized hub that makes all push operations work |

---

## Who Uses It

Anchor Hub is for internal ANS staff only. Access requires an `@anchornetworksolutions.com` Microsoft account (SSO via Entra ID).

Roles control what each user can see and do:

| Role | Access |
|---|---|
| **Read-Only** | View reports and breakdowns only |
| **Standard** | Run audits, view invoice data, use all read tools |
| **Admin** | Full access — can push updates to Autotask, manage mappings, configure settings |

Roles are assigned in Entra ID. Contact your admin to request a role change.

---

## Getting Help

If something isn't working as expected:
1. Check the relevant **Tool Guide** in this help center
2. Use the **Request Support** button on the Help page inside the app to send a message directly to your admin
3. Use **Report a Bug** to flag something that looks wrong

For urgent issues, reach out to Mike directly.
