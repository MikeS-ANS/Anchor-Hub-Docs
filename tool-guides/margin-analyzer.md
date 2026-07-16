# M365 Margin Analyzer

The Margin Analyzer reviews your Azure partner costs against what clients are being billed, surfacing which clients have margins below your target and which are healthy.

## Running an Analysis

1. Navigate to **Margin Analyzer** in the left sidebar
2. Set your **target margin %** (default is 20%)
3. Click **Run Analysis**
4. Results load per client showing cost, price, and margin percentage

## Reading the Results

Each row shows a client with:
- **Partner cost** — what ANS pays Pax8
- **Billed price** — what the client is charged
- **Margin %** — color coded: green (at or above target), yellow (below target), red (significantly below or inverted)

Clients with red or yellow margins are candidates for a price review in the Invoice Processor.

## Exporting

Click **Export** to download the full margin report as a spreadsheet.

## Settings

Click **Settings** to view:

- **Azure Contract Name** — the contract-name keyword used to identify Azure contracts (default: "Microsoft Azure Cloud Services")
- **Azure Service ID** — the Autotask service ID for Azure line items
- **Scheduled analysis** — day-of-month and enabled/disabled toggle for the automatic monthly run

> **Global setting — shared by everyone.** All four are centrally managed via Azure App Configuration. The Settings screen shows the current values but the Save button won't apply a change — contact Mike (or whoever holds the `hub.admin` role) to update one.

---

> **Guide in progress.** More detail will be added here as the tool evolves.
