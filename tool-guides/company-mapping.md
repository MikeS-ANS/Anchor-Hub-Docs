# Company Mapping

Company Mapping is the foundation that connects your Pax8 clients to their Autotask counterparts. All tools that push data to Autotask — the Kaseya Invoice Processor, Blackpoint, and others — rely on this mapping to know which Autotask company to update.

Mappings are stored in a centralized hub directory on SharePoint (`hub-company-mappings.json`). Confirm a match once and every tool uses it — no per-tool re-mapping needed.

---

## How Auto-Match Works

Matching happens automatically in the background whenever a tool processes an invoice or runs a sync. You don't need to come to Company Mapping for routine use.

The matcher uses token-based name similarity (Jaccard algorithm):
- **≥85% confidence** — accepted and saved silently
- **<85% confidence** — surfaced as a suggestion for you to review

Confidence is color-coded: green (≥85%), amber (≥70%), gray (<70%).

---

## Running a Manual Sync

If you've added new clients in Pax8 or Autotask and want to refresh mappings outside of an invoice run:

1. Navigate to **Company Mapping** in the left sidebar
2. Click **Sync**
3. The tool queries Pax8 and Autotask and attempts to match any unlinked companies
4. Review suggested matches in the panel that appears

---

## Accepting and Rejecting Matches

For each suggested match you'll see the Pax8 name, the suggested Autotask company, and the confidence score.

- **Accept** — confirms the match and saves it to the hub directory
- **Skip** — dismisses the suggestion; the company stays unmatched and can be mapped manually later
- **Accept All (High Confidence)** — one click to accept all suggestions at ≥85%

---

## Manual Mapping

For clients whose names differ too much between systems for auto-matching:

1. Find the unmatched Pax8 company in the list
2. Click **Map Manually**
3. Type part of the Autotask company name to search (live search, up to 15 results)
4. Select the correct company and confirm

---

## Excluded Companies

Some Pax8 entries aren't real billing clients (internal test accounts, reseller pass-throughs, etc.). Excluding them hides them from all audit results and push targets.

1. Find the company in the list
2. Click **Exclude**

To restore an excluded company, find it in the Excluded section and click **Restore**.

---

## Shared Across Tools

Because mappings live in the centralized hub directory, confirming a match in Company Mapping makes it immediately available to:

- **Kaseya Invoice Processor** — workplace and SaaS pushes
- **Blackpoint** — billing push
- Any future tool that needs Pax8 → Autotask resolution

The hub directory stores each AT company once, with platform-specific sub-entries (Pax8, Kaseya, Blackpoint) hanging off it. This means one exclusion or confirmation applies everywhere.

---

## Storage

Mappings are stored in SharePoint at:

> `ANS-Company Shared → Anchor Hub → hub-company-mappings.json`

Local caching is used as a fallback if SharePoint is temporarily unavailable. The SharePoint copy is always authoritative.
