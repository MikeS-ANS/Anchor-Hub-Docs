# Cytracom Invoice Processor

Cytracom is ANS's VoIP/network vendor (product line "ControlOne"). Cytracom has no billing API, so the monthly invoice arrives as a PDF. This tool reads that PDF, matches every client and service against Autotask, shows you exactly what changed, and pushes quantity updates to the right contract line — without you touching a spreadsheet or opening Autotask by hand.

---

## Loading an Invoice

**If the invoice is already in SharePoint:**

1. Navigate to **Cytracom Invoice Processor** in the sidebar
2. Select the **year** from the dropdown
3. Select the **invoice PDF**
4. Click **Process Invoice**

**If you have a new invoice PDF on your computer:** drag it onto the upload zone (or click the zone to browse for it). This processes the file directly from your computer — it does not upload anywhere on its own.

**To keep a copy in SharePoint:** once a locally-loaded file has been processed, an **Archive to SharePoint** button appears next to it. This is optional and separate from processing — it uploads the same file into the correct year folder (creating that year's folder automatically if it's the first invoice archived for it) so the next person to run this invoice can pick it up from the SharePoint list instead of finding the PDF again.

Invoices live in SharePoint under **ANSVendors → Cytracom** (one folder per year). The exact folder is centrally configured, not hardcoded — if it ever needs to move, ask an admin.

The processor reads the PDF directly — there is no vendor API to pull from, so a Cytracom invoice PDF always has to be loaded by hand one of the three ways above.

---

## Already Processed?

Every time an invoice loads, the tool checks whether that exact invoice number has been pushed before and shows a banner:

- **Not processed yet** — no previous push is on record for this invoice.
- **Already processed** — shows who pushed it and when, and how many lines changed.
- **Could not check** — the check itself failed (or the PDF had no readable invoice number). This is shown as **unknown**, never as a quiet all-clear — confirm in Autotask before pushing anything if you see this.

Re-opening and re-processing an already-pushed invoice is allowed on purpose and does **not** block you. Every line that's already correct simply computes to "no change" and nothing gets written for it — so if a previous push partially failed partway through, you can safely run the same invoice again to finish it, rather than being locked out.

Invoices are tracked by **Cytracom's own invoice number** printed on the PDF, not by filename or month — Cytracom sometimes issues a separate prorated invoice mid-month, and that gets its own dedupe check rather than being confused with the regular monthly one.

---

## Reading the Results

Once an invoice loads, the header leads with the invoice's **month** (for example, "August 2026") — matching how the Hub's other invoice processors open — with the invoice number and the date it was posted shown just below.

Below that, a summary strip shows counts across the whole invoice:

| Tile | Meaning |
|---|---|
| **Ready to reconcile** | Client sections with a real, found Autotask contract — these are the only ones you can push from |
| **No contract yet** | Client has no Cytracom contract in Autotask at all — likely a brand-new client |
| **Contract unknown** | The Autotask lookup itself failed or was ambiguous — not the same as "no contract" |
| **Needs mapping** | This Cytracom client name isn't mapped to an Autotask company yet |
| **Excluded** | Deliberately excluded from billing in the Company Directory |
| **ANS internal** | Anchor's own Cytracom usage — never billed to a client, never pushed |
| **Unmatched services** | Invoice line items that didn't match anything in the Autotask catalog |

Below the summary, each client gets its own card, sorted alphabetically by client name — except Anchor's own internal-usage section (**ANS internal**), which always sits last, no matter where it happens to fall on the invoice itself. Only a card whose contract was actually **found** shows a pushable diff table — every other state (no contract, unknown, needs mapping, excluded, ANS internal) explains why it isn't pushable and shows the raw invoice lines instead, so nothing is ever hidden, just not actionable from this screen.

Two banners can also appear above the client cards:

- **Merged sections** — if the invoice printed the same client name in more than one place (usually a page break mid-client), those sections are combined into one reconciliation. This is expected, but it's called out explicitly because if two genuinely *different* Autotask companies ever happened to share the same Cytracom name, the merge would reconcile one client's seats against the other's contract — worth a quick glance at the PDF if this banner shows up.
- **Rows not recognized** — anything the parser couldn't classify at all. These are never silently dropped; expand the banner to see exactly what text it couldn't parse, and confirm none of it is a real billable line before moving on.

---

## The Diff Table

For each reconcilable client, the table shows one row per service:

| Column | Meaning |
|---|---|
| **Service** | The Autotask catalog service this invoice line resolved to |
| **Current → Invoice** | The contract's current quantity vs. what this invoice shows |
| **Δ** | The difference — orange for an increase, blue for a decrease |
| **Cost / Price** | Shown for every line; editable only for a brand-new line — see below |

Rows with a real change (or a brand-new line) are pre-selected for you; a row with no change is left unselected since there's nothing to push.

**Existing lines** — cost and price are never shown as editable, and are never sent to Autotask when you push. The invoice only tells you Cytracom's wholesale cost to ANS, never what a given client is actually charged — so pushing a price here would silently overwrite a real, possibly client-specific retail price with a wrong guess. Only the quantity changes.

**Brand-new lines** — a service the client's contract has never carried before shows a blank cost and price field. Nothing is pre-filled here, on purpose — type in the real values before this row can be selected for push. If you type a $0 or negative cost or price, the tool will still let you push it, but only after you explicitly acknowledge it in the confirmation step (a genuinely bundled $0 line is legitimate, but it's also exactly what a typo looks like).

---

## Company Matching

Cytracom is one of the platforms tracked in the Hub's **Company Mapping** tool, alongside Pax8, Kaseya, BlackPoint, and Meraki. If a client section shows **"needs mapping,"** it means this Cytracom client name has no confirmed match to an Autotask company yet.

Map it right from the card — no need to leave this screen. Type part of the **Autotask company's** name into the search box and click **Search Autotask**, then click **Map to this company** next to the right result. The Cytracom client name is already filled in for you, read straight from the invoice, so you never have to type or copy it by hand. As soon as you confirm the match, the invoice re-processes automatically and that client's section reconciles right there.

A search needs at least 3 characters, only matches active Autotask companies, and shows at most 15 results — narrow your search if the client you're looking for isn't in the list.

**Open Company Mapping** is still on the card as a secondary option, for cases that need the full Company Directory screen — for example, registering a brand-new Autotask company that doesn't exist yet.

Unmapped clients are never guessed at — there's no fuzzy auto-match for Cytracom the way some other platforms have one.

---

## Unmatched Services — Adding to the Catalog

If an invoice line item doesn't match anything in Autotask's Cytracom catalog, it's listed under **Unmatched Services** with an **Add to catalog…** action.

This creates a real, live Autotask catalog service — and it's worth knowing that **Autotask has no way to delete a catalog service through its API.** If you make a mistake here, it has to be fixed by hand in the Autotask web UI afterward, not undone from inside the Hub. Because of that, this action asks for its own explicit confirmation, separate from the push confirmation below, and shows you the exact name, description, cost, and price before anything is created. Cost and price are left blank for you to fill in — nothing is pre-filled.

Once the service exists, the invoice re-processes automatically and that line resolves right away — no need to reload or re-process it yourself.

---

## Pushing to Autotask

1. Review the diff table and tick the rows you want to push (changed/new rows are pre-selected; you can add or remove any row)
2. For any brand-new line, type in the cost and price
3. Click **Push to Autotask**
4. Review the confirmation dialog — it lists every client, service, and exact target contract/line, with **current → new** shown for existing lines and **quantity, cost, and price** shown for new ones
5. If any new line has a $0 or negative cost or price, you'll need to tick a box confirming that's intentional before you can continue
6. Confirm to push

A progress panel then streams each row's result in real time:

| Result | Meaning |
|---|---|
| ✓ pushed | A real quantity change was written to Autotask |
| — no change | Nothing to write — the invoice quantity already matches Autotask |
| ⚠ no contract | This row's client has no Cytracom contract to push to |
| ⚠ no service | The catalog service couldn't be resolved for this row |
| ⚠ would go negative | Refused — pushing this would drive the quantity below zero |
| ✗ error | The write failed — see the detail shown next to the row |

Only sections whose contract was actually **found** can ever be pushed — nothing in a "no contract," "unknown," "needs mapping," or "excluded" state can reach this step, even if you try. This is enforced independently of what's shown on screen, so a stale or leftover selection can never slip through.

Pushing writes real, irreversible adjustments to live Autotask contracts. There is no undo button inside the Hub for a push — a mistaken quantity change has to be corrected with another adjustment (or by hand in Autotask), the same as any other invoice processor in the Hub.

**Process each month's invoice within that month.** Every adjustment is dated the **1st of the month you push in** — not the month the invoice covers. Processing promptly each month keeps those the same, which is how this tool is meant to be used. If an older invoice were pushed months later, the adjustment would still be dated the current month, and the contract it targets would be whichever one is active today rather than the one that was active during the invoice's billing period.

---

## Push History

Expand **Push history** on the results screen to see past pushes for this tool: when each one ran, who ran it, which invoice number it was, and how many lines it changed. This is pulled from the Hub's shared activity log, so it's visible to everyone, not just the person who ran it.

---

## Good to Know

- **ANS's own Cytracom usage** appears as its own section on every invoice, clearly marked "ANS internal." It's shown so the invoice total still makes sense, but it is never reconciled against a contract and can never be pushed.
- **Discount lines** ("100% off" style credits) are read and shown, but are never independently reconciled — the printed line amount is always trusted as-is rather than recalculated.
- **This tool has no AI or AI-generated content anywhere.** Every match, flag, and number on screen is computed directly from the invoice PDF and live Autotask data — nothing here is generated or summarized by a language model.
