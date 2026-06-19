# Company Mapping

Company Mapping is the foundation that connects your Pax8 clients to their Autotask counterparts. Without a mapping, the Invoice Processor can't push updates for a client — it won't know which Autotask company to update.

## How Auto-Match Works

The Invoice Processor automatically tries to map companies every time you load an invoice. You don't need to come to Company Mapping for routine use — it handles itself in the background.

When auto-match finds a strong match (≥85% confidence), it saves it silently. Lower-confidence suggestions appear as a confirmation panel in the Invoice Processor results.

## Running a Manual Sync

If you've added new clients in Pax8 or Autotask and want to refresh mappings outside of an invoice load:

1. Navigate to **Company Mapping** in the left sidebar
2. Click **Sync**
3. The tool queries both Pax8 and Autotask and attempts to match unlinked companies
4. Review any suggested matches and accept or skip them

## Accepting a Match

When the sync surfaces a suggested match:
- Review the Pax8 name and the suggested Autotask company name
- If correct, click **Accept**
- If wrong, click **Skip** — the company stays unmatched and you can map it manually later

## Manual Mapping

For clients that don't auto-match (name is too different between systems):

1. Find the unmatched Pax8 company in the list
2. Click **Map Manually**
3. Search for the correct Autotask company by name
4. Select it and save

## Excluded Companies

Some Pax8 entries aren't real billing clients (internal accounts, test entries, etc.). You can exclude these so they don't show up as unmapped every time:

1. Find the company in the list
2. Click **Exclude**

Excluded companies are hidden from audit results and invoice push targets.

---

> **Guide in progress.** More detail will be added here as the tool evolves.
