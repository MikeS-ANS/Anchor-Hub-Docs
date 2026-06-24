# BlackPoint Usage

Automates monthly Blackpoint billing reconciliation against Autotask. Load the Account Usage Report CSV from SharePoint, compare it to current Autotask contract units, and push adjustments — all without leaving Anchor Hub.

---

## Prerequisites

- Sign into Anchor Hub with your Microsoft account (SSO required — the tool reads from SharePoint)
- The monthly Account Usage Report CSV must be exported from Blackpoint and saved to `ANS-Vendors/Blackpoint/invoices/{year}/` in SharePoint

---

## Compare & Push Tab

### 1. Load a file

1. Select a **Year** from the dropdown — years are pulled live from SharePoint
2. Select the **CSV file** for the month you're reconciling
3. Click **Load & Compare**

The tool downloads the CSV, then loads all active Autotask Managed Security Services contracts and unit counts in a single bulk query. Expect 30–60 seconds on first load; subsequent runs in the same session are faster because AT data is cached for 30 minutes.

### 2. Read the comparison table

Each row is one Blackpoint company. Columns:

| Column | Meaning |
|--------|---------|
| **Company** | Blackpoint name · matched AT company shown below |
| **Match** | ✓ Confirmed / ~ Auto / Fix / No match |
| **MDR Devices** | Device count from the Blackpoint CSV |
| **Billed Security+** | Units currently on the AT contract |
| **Security+ Change** | Delta (orange = increase, blue = decrease) |
| **CR Included** | Cloud Response seats included free = `floor(MDR × 1.3)` |
| **CR Used** | Cloud Response devices from the CSV |
| **CR Billable** | `max(0, CR Used − CR Included)` — what gets billed |
| **Billed 365 Defense** | 365 Defense units currently on the AT contract |
| **365D Change** | Delta for 365 Defense |

### 3. Filter the table

Click any badge in the filter bar to focus the table:

- **All** — show everything
- **matched** — companies with a confirmed or auto match
- **need match** — companies needing manual matching
- **AT errors** — rows that failed to load from Autotask
- **SP changes** — companies with a Security+ delta
- **365D changes** — companies with a 365 Defense delta
- **excluded** — companies marked as excluded from billing

Click the same badge again (or **✕ clear**) to remove the filter.

### 4. Fix company matches

When Blackpoint and Autotask use different company names, the tool fuzzy-matches them automatically. High-confidence matches are auto-confirmed. For anything marked **Fix** or **No match**:

1. Click the **Fix** or **Set** button on that row
2. Confirm a suggested candidate or search by name
3. Click **Confirm Match**

Confirmed matches are saved to SharePoint and shared with all users — you only need to fix each company once.

### 5. Exclude a company

Click **✕** next to any company to exclude it from billing (useful for internal or non-billable accounts). Click **↩** to un-exclude. Saved to SharePoint.

### 6. Push changes

Once the table looks right:

1. Click **Push Security+** or **Push 365 Defense** (only enabled when there are changes)
2. Review the confirmation dialog — it shows every company that will be updated, with current units, new units, and the change
3. Click **Confirm & Push** to proceed, or **Cancel** to abort

The tool posts `ContractServiceAdjustments` to Autotask:
- **Increases** take effect the 1st of the current month
- **Decreases** take effect the 1st of next month

If a company's contract is missing the Security+ or 365 Defense service line, the tool creates it automatically from the catalog price.

A progress modal streams results as each company is processed. A push audit log is saved to `ANS-Vendors/Blackpoint/push-log.json` on SharePoint after every run.

### 7. Export and utilities

- **Export to Excel** — exports the full comparison table with color-coded delta columns to your Downloads folder
- **↻** (refresh icon) — re-fetches the SharePoint file list without leaving the tool
- **↺ Retry N errors** — appears in the filter bar when rows have AT errors; re-runs just those rows using cached data

---

## Endpoint Usage Tab

Shows live device counts per tenant pulled directly from the Blackpoint API. Useful for spot-checking counts or investigating tenant-level discrepancies. This tab requires the Blackpoint API key to be configured in Azure Key Vault (`blackpoint-api-key`).

---

## How billing is calculated

### Security+
The MDR Devices count from the CSV is the billable unit count for Security+. Compare directly to the AT contract unit.

### 365 Defense
Cloud Response devices include a free tier based on Security+:

```
Included (free) = floor(MDR Devices × 1.3)
Billable        = max(0, Cloud Response Used − Included)
```

**Example:** 50 MDR devices → 65 Cloud Response included free. If the customer used 70 CR devices, bill **5** (70 − 65). If they used 60, bill **0**.
