# Roles & Permissions

Anchor Hub uses **Azure AD App Roles** to control which tools each person can see. The sidebar is filtered automatically at sign-in — users only see tools their role allows. There is nothing to configure in the app itself; everything is managed through Azure AD and two SharePoint lists.

## How it works

1. A user signs into Anchor Hub with their Microsoft account
2. Their Azure AD App Roles are read from their login token (no extra lookup required)
3. Those roles are matched against the **Hub Role Matrix** SharePoint list to determine which tools they can see
4. The sidebar is filtered to show only the allowed tools

Changes to role assignments or the matrix take effect the **next time the app is restarted** — role data is cached for the session to keep the app fast.

## Automatic role assignment

Roles are assigned via **Azure AD Dynamic Groups**. Each Hub role has a corresponding security group with a membership rule based on job title. When someone's title is updated in M365, they're automatically added to or removed from the right group — and their Hub access updates within a few minutes, with no manual steps required.

---

## Hub Roles

| Role | Assigned to | Tools |
|---|---|---|
| `hub.admin` | Directors | All tools |
| `hub.manager` | Manager of Service Delivery, Manager of Professional Services | M365 Subscription Comparison, Project Time Summary, Duo Management, Project Profitability |
| `hub.delivery` | Service Desk Engineers & Leads, Co-Managed Tech Lead, CTS Engineer, Cybersecurity Admin, Office Admin | Duo Management |
| `hub.tam` | Technical Account Manager | Duo Management |
| `hub.strategic` | Account Manager, Client Experience Manager, Technology Strategist | M365 Subscription Comparison, BlackPoint Usage, MSC Agreements, Duo Management |
| `hub.projects` | Technical Project Engineer, Project Engineer | Project Time Summary, Project Profitability |
| `hub.finance` | Accountant, Accounting Assistant | M365 Subscription Comparison, Pax8 Invoice Processor, M365 Margin Analyzer, Pax8 Invoice Comparison, Kaseya Invoice Processor, Autotask Contract Changes, Autotask Contract Renewals, BlackPoint Usage, MSC Agreements |
| `hub.sales` | Business Development | None currently assigned |
| `hub.wsd` | Workstation Deployment Specialist | None currently assigned |

---

## Managing access

### Update which roles can see a tool

The **Hub Role Matrix** SharePoint list is the source of truth for tool access. Each row is one tool; each boolean column (`RoleAdmin`, `RoleManager`, etc.) controls whether that role can see it.

1. Go to **SharePoint Intranet → Lists → Hub Role Matrix**
2. Find the row for the tool you want to change (e.g. *BlackPoint Usage*)
3. Click the row to edit it
4. Toggle the role column to **Yes** or **No**
5. Save — affected users will see the change on their next app restart

**Column names:** `RoleAdmin` · `RoleManager` · `RoleDelivery` · `RoleTam` · `RoleStrategic` · `RoleProjects` · `RoleFinance` · `RoleSales` · `RoleWsd`

---

### Add a new tool to the matrix

When a new tool is added to Anchor Hub, it won't appear for anyone until it has a row in the Hub Role Matrix. The tool key will be included in the release notes for that version.

1. Go to **SharePoint Intranet → Lists → Hub Role Matrix**
2. Click **+ New**
3. Set **Title** to the tool's display name (e.g. *New Tool Name*)
4. Set **ToolKey** to the exact key from the release notes (e.g. `new-tool-key`)
5. Toggle each role column to **Yes** for roles that should see the tool
6. Save — users with a matching role will see the tool on their next restart

---

### Grant one person access to a specific tool

If someone needs access to a single tool that isn't part of their role — without giving them an entirely different role — use the **Hub User Overrides** SharePoint list. Each row grants one person access to one tool.

1. Go to **SharePoint Intranet → Lists → Hub User Overrides**
2. Click **+ New**
3. Set **Title** to the person's full M365 email address (e.g. `patm@anchornetworksolutions.com`)
4. Set **ToolKey** to the exact tool key (see the table below)
5. Save — the person will see the tool on their next app restart

To grant multiple tools to the same person, add one row per tool. To remove access, delete the row.

**Tool key reference:**

| Tool key | Tool name |
|---|---|
| `subscription-audit` | M365 Subscription Comparison |
| `invoice-monitor` | Pax8 Invoice Processor (AI) |
| `margin-analyzer` | M365 Margin Analyzer |
| `company-mapping` | Company Mapping |
| `invoice-processor` | Pax8 Invoice Comparison |
| `kaseya-processor` | Kaseya Invoice Processor |
| `project-time-summary` | Project Time Summary |
| `contract-changes` | Autotask Contract Changes |
| `contract-renewals` | Autotask Contract Renewals |
| `blackpoint-processor` | BlackPoint Usage |
| `msc-agreements` | MSC Agreements |
| `duo-management` | Duo Management |
| `project-profitability` | Project Profitability |

---

### Assign a role to someone not covered by a dynamic group

For users whose job title doesn't match any dynamic group rule, or for temporary access, you can assign a role directly in Azure AD:

1. Go to **Azure AD → Enterprise Applications → Anchor Hub**
2. Click **Users and groups → + Add user/group**
3. Select the person
4. Choose the appropriate App Role
5. Click **Assign**

The user's new role is reflected in their token the next time they sign in to the app.

---

## Fallback behavior

If a user has **no role assigned** and has **no individual overrides**, the sidebar shows all tools. This is the fallback for users who aren't yet configured — it ensures no one is accidentally locked out during initial rollout.

Once the Hub Role Matrix is populated and roles are assigned, only the permitted tools will appear.

---

## Troubleshooting

**"You don't have access to this application" at login**\
The user hasn't been assigned any App Role. Add them to an Azure AD dynamic group or assign a role directly in Enterprise Applications.

**A tool isn't showing for someone who should have access**\
Check two things: (1) the user's App Role is assigned in Azure AD, and (2) the corresponding role column is set to **Yes** in the Hub Role Matrix row for that tool. Then have them restart the app.

**Role changes aren't taking effect**\
Role data is cached for the session. The user needs to fully close and reopen Anchor Hub for changes to apply.

**A new tool is missing from the Hub Role Matrix**\
Add a row for it manually — see [Add a new tool to the matrix](#add-a-new-tool-to-the-matrix) above.
