# Roadmap & Coming Soon

This page tracks what's being worked on and what's planned next for Anchor Hub.

---

## In Progress

- **Brian's tools (~5 total, 1 done)** — Flask port of Brian's toolset into Anchor Hub
- **Contract Name column — Project Time Summary** — Adding contract name visibility to PTS table
- **Azure Key Vault — Claude & Blackpoint API keys** — Moving Claude and Blackpoint API credentials into Key Vault (Infrastructure)

---

## Platform Priorities

These are ordered. Do EV cert + Sentry first. Restructure after Brian's tools.

| Priority | Item | Notes |
|---|---|---|
| P1 | EV Code Signing Cert | Fixes SmartScreen / AV false positives on install |
| P1 | Sentry Error Monitoring | Crash/error reporting infrastructure |
| P2 | Modular File Restructure | Split main.js / app.js into modules — after Brian's tools land |
| P2 | Intune / MDM Deployment | Push app via Intune instead of manual install |
| P3 | Audit Trail / Action Log | Log of actions taken in the app per user |

---

## Ideas & Backlog

### Home Screen
- **Tool run schedule & status badges** — Set a run frequency per tool; badge turns red when overdue (On time / Due soon / Overdue)
- **Notifications center** — Bell icon with badge; overdue alerts, release notes, and announcements
- **In-app idea submission** — Button for employees to submit tool ideas without messaging Mike directly

### Access
- **Role-based tool access** — Permissions, roles, and groups setup; builds on top of SSO
- **Central API key revocation** — Revoke credentials from one place

### Tools
- **New employee onboarding checklist** — Guided checklist for new hires
- **In-app bug reporter** — Let users report issues from inside the app

---

## Recently Shipped

- [v1.4.4 — Project Profitability Improvements](v1.4.4.md)
- [v1.4.3 — Project Profitability](v1.4.3.md)
- [v1.4.2 — Fuzzy Match & Push History](v1.4.2.md)
- [v1.4.1 — Duo Management](v1.4.1.md)

**All shipped tools:** Subscription Audit, Invoice Monitor, Margin Analyzer, Company Mapping, Kaseya Invoice Processor, Invoice Processor, Contract Changes, Contract Renewals, BlackPoint / CompassOne, Project Time Summary, MSC Agreements, Project Profitability, Sidebar Customization, Microsoft SSO / Entra ID
