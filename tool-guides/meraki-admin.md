# Meraki Admin Management

Audit and manage Cisco Meraki dashboard administrators across all client organizations. Requires the **hub.it** or **hub.admin** role.

---

## Audit Tab

The audit runs a gap analysis against the **Organization Template** org, which defines the standard set of ANS admins every client org should have.

Click **Refresh** to fetch all orgs and their admins from the Meraki API. This takes 30–60 seconds depending on the number of orgs.

Once loaded, the table shows each org with:

| Column | Description |
|--------|-------------|
| ANS | Number of ANS admins in this org |
| Client | Number of non-ANS admins |
| Missing | ANS admins in the template but absent from this org |
| Extra | ANS admins in this org but not in the template |
| Status | Baseline / ⚠ issues / ✓ OK |

**Filtering:** Use the filter box to search orgs by name.

**Export CSV:** Downloads the full audit table as a CSV file.

### Expanding an Org

Click any row to expand it. The expanded view shows:

- **Missing ANS Admins** — chips with a green **+ Add** button per admin, plus **+ Add All Missing** to add them all at once
- **Extra ANS Admins** — chips with an amber **Remove** button, plus **Remove All Extra**
- **Full admin table** — Name, Email, Role, Last Active, Type (ANS/Client), with a **Remove** button per row
- **+ Add Admin to this org** — inline form to add a one-off admin (email + name, always full access)

> All remove actions require a two-click confirm. Click Remove once to see "Confirm?" — click again within 3 seconds to proceed.

### Fix All Missing

The **Fix All Missing** toolbar button adds every missing ANS admin across every org with gaps in one operation. Requires a confirmation click. Use this after onboarding a new ANS employee.

---

## Add Employee Tab

Adds a new ANS admin to **all active client orgs** at once (ANS orgs and excluded orgs are skipped automatically).

1. Enter the employee's **Email** and **Full Name**
2. Click **Add to All Client Orgs**
3. A live log shows the result per org (Added / Already exists / Error)

Access level is always **Full** — manage the ANS org directly in the Meraki portal if needed.

---

## Remove Admin Tab

Removes an admin by email from all active client orgs.

1. Enter the admin's **Email**
2. Click **Remove from All Client Orgs**
3. A live log shows the result per org (Removed / Not found / Error)

---

## Excluded Orgs Tab

Organizations that ANS has been offboarded from but hasn't been removed from yet should be **Excluded** rather than deleted from Meraki.

**To exclude an org:** Click the **Exclude** button on the org's row in the Audit tab.

Excluded orgs are:
- Hidden from the Audit tab gap analysis
- Skipped during bulk Add Employee and Remove Admin operations
- Listed in this tab for review

**To un-exclude:** Click **Un-exclude** in this tab. The org will reappear in the next audit.

Exclusions are stored in the Hub Company Directory and shared across all Hub users.

---

## Company Mapping (Meraki Orgs Tab)

Meraki org names often don't match AT company names exactly. The **Meraki Orgs** tab in the Company Mapping tool lets you link them.

**Auto-Match:** Click the **✨ Auto-Match** button to fuzzy-score all unmapped orgs against AT companies. Suggestions are shown with a confidence badge:
- **High (≥75%)** — reliable matches, safe to accept in bulk
- **Med (55–75%)** — review individually
- **Low (<55%)** — low confidence, likely needs manual assignment

Use **Accept All High Confidence** to bulk-accept the obvious matches, then review the rest individually.

**Manual assign:** Click **+ Assign** on any unmapped org to search for its AT company by name.
