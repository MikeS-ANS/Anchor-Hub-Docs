# Timesheet Review

> **Beta.** Marked Beta in the sidebar and in the tool itself (v3.2.0) while we gather feedback from managers before general release. The note-mismatch keyword table and the pattern-tiering rate thresholds are both provisional — see [Notes & limits](#notes--limits) below.

Timesheet Review pulls a manager's (or admin's) team roster from Autotask time entries, classifies every entry as **client-facing**, **internal**, or **time off**, runs a set of deterministic data-quality checks against it, and layers three separate AI features on top for narrative review and free-form Q&A. It's read-only against Autotask — there's no in-Hub "approve" action; approval still happens in Autotask itself.

---

## Team scoping

- **Hub Admins** see every active Autotask resource.
- Everyone else sees only their own **Entra direct reports**, matched to Autotask resources by email (checks both `@anchornetworksolutions.com` and `@anchorns.com`).
- **Excluded resources** — service accounts, contractors, anyone not a real employee to review — are hidden everywhere in this tool via Settings → Excluded resources. This is scoped to Timesheet Review only, not the Company Directory.

> **First time using this as a manager?** If you get a permissions error opening the roster, **sign out and sign back in once** — v3.2.1 added the `User.ReadBasic.All` Graph permission needed to read your direct reports (a non-admin account can't read other users' data with just the default `User.Read` scope, even under "/me"). You may see a one-time consent prompt.

---

## Loading data — cache vs. live pull

Opening the tool is **not** a live Autotask call every time:

1. **Same app session, same period** — reuses whatever's already loaded in memory.
2. **Different period, or app just restarted** — checks the SQL cache (`timesheet_scans`) first. If that period was already scanned before, it loads from there with no Autotask call.
3. **Never scanned before, or you click ↻ Refresh** — pulls live from Autotask/Graph, re-classifies, and overwrites the cache.

**Previous/Next period navigation** always re-fetches your team roster too (a quick Autotask/Graph call), separate from the scan itself.

---

## Classification

Each time entry is bucketed by **account**, not the billable flag — a lot of ANS client work is non-billable fixed-fee but still client-facing:

| Bucket | How it's determined |
|---|---|
| **Client** | Ticket/task's company is not the internal company. |
| **Internal** | Company is the internal company (Autotask ID `0` — a reserved sentinel, not a normal company row), or general time with no matching time-off allocation code. |
| **Time off** | General time entry (no ticket/task) whose allocation code name matches a configured time-off activity (PTO / Sick / Holiday). |

**Client-facing %** = client hours ÷ all logged hours (time off counts in the denominator by design — excessive time off should pull the number down; this is real utilization, not "of the time you worked").

> **⚠ Permission-gap warning.** If an entire scan comes back with zero time-off hours across everyone loaded, the tool shows a banner warning the shared read-only Autotask API key may be missing "can view non-billable time entries" permission — meaning PTO/sick/holiday entries are silently missing and client-facing % is inflated. Check the API user's permissions if you see this.

---

## Data-quality flags

Six deterministic checks run automatically on every timed entry (no AI involved) — click any flag chip on an entry row to see its specific reason.

| Flag | Fires when |
|---|---|
| **Gap** | Uncovered time between two consecutive entries exceeds the gap threshold. Also fires for a gap between business-hours start and the first entry, or the last entry and business-hours end. |
| **Overlap** | An entry's start time is before the previous entry's end time. |
| **Late** | Entry was logged (`createDateTime`) more than N business days after the work date. |
| **Thin Note** | Note is empty, a single word, or shorter than the minimum character length. |
| **After Hours** | Entry starts or ends outside configured business hours. |
| **Note Mismatch** | Note matches a short-task keyword (e.g. "sent email") but logged duration is more than the configured multiplier over that phrase's expected ceiling. |

**Inline gap display.** Independent of the Gap flag's threshold, the day view shows a dashed divider row between entries for *every* uncovered block of time, with its exact duration — including the business-hours boundary gaps. These are color-tiered for at-a-glance severity: **green** (1–5 min, normal ticket-to-ticket transition), **yellow** (6–15 min), **red** (16+ min).

---

## AI Pattern Tiering

Layered on top of the deterministic flags: each flag type is tiered by how often it occurred across the period, so severity judgment is fixed and consistent rather than left to an AI to re-derive each time.

- **Occurrence rate** = distinct days with at least one instance of that flag type ÷ business days in the period.
- Rate above **Pattern CONCERN rate** → **CONCERN**. Above **Pattern WATCH rate** → **WATCH**. Otherwise **NORMAL**.
- **Hours shortfall** (expected total hours for the period, based on business days × expected daily hours, minus actual hours logged) is tiered separately: more than the configured threshold short → **CONCERN**, otherwise **NORMAL**. This is treated as an objective, always-statable number — not subject to "normal MSP noise" judgment the way isolated flags are.

When every pattern is NORMAL and the hours shortfall isn't a CONCERN, the AI review buttons visibly restyle to a dimmed **"✓ No concerns — normal period"** state — and clicking them shows that message instantly with no AI call made at all.

---

## The four AI features (easy to mix up — here's the distinction)

| Feature | What it does | Cost | Where |
|---|---|---|---|
| **Manager Review** | One-shot narrative for a single person's period — states CONCERN/WATCH findings plainly, in priority order. | 1 Hatz.ai call (skipped if fully normal) | Resource detail view, "Review with AI" button |
| **Team Review** | Same idea across everyone in the current selection (or the whole team if nothing's selected) — only calls out people with a CONCERN or WATCH item. | 1 Hatz.ai call (skipped if fully normal) | Team view, "Review Team with AI" button |
| **Ask AI** | Free-form Q&A chat scoped to whatever's currently loaded — answer any question about the loaded data. | 1 Hatz.ai call per question | Floating chat button, bottom-right of the resource detail view |
| **AI Note Review** | A completely separate per-*entry* pass — reads every note's text and classifies it `ok` / `vague` / `mismatch` / `other`, independent of the deterministic checks above. Catches genuinely vague-but-plausible-length notes nothing else can. Capped to a configurable date range per run (batches of ~20–40 entries per Hatz.ai call) since it costs a real AI call per entry, not per person. | N Hatz.ai calls (batched) | Team view, "AI Note Review" button |

**Two different "mismatch" concepts exist and it's easy to confuse them:**
- **Note Mismatch** (amber chip) — deterministic, instant, free. Keyword table + duration ceiling.
- **AI: Mismatch** (purple chip, from AI Note Review) — the AI's own judgment reading the note's actual content. Click the chip to see its one-sentence reason, same interaction as every other flag chip.

AI Note Review results are cached (`timesheet_ai_note_cache`, keyed by a hash of the note text + hours) so re-running doesn't reprocess identical entries.

---

## AI Prompts

All four features call Hatz.ai's `/chat/completions` endpoint (`main/ipc/timesheetReview.js`). Model is admin-configurable in Settings.

### Manager Review

Sends only the tiered `{summary, hoursShortfall, patterns}` payload — no raw entries. Severity is entirely pre-computed; the model's only job is to narrate it briefly.

**System prompt:**
> You are writing a one-paragraph note for {resourceName}'s manager about their timesheet for {periodStart} to {periodEnd}, at a Denver-area MSP. This is for the manager's eyes only.
>
> All severity judgments have already been made by the system — CONCERN, WATCH, or NORMAL tiers below reflect fixed, pre-computed thresholds. Do not re-judge, second-guess, soften, or escalate these tiers yourself. Your only job is to state the CONCERN and WATCH findings in plain language, in priority order, as briefly as possible.
>
> Rules:
> - Start with a one-sentence verdict: normal period, or state the single biggest concern immediately. Do not build up to it.
> - State every CONCERN-tier item plainly, citing the specific dates or entries given. This includes a CONCERN-tier hours shortfall if present — state the shortfall in hours and do not hedge it.
> - Mention WATCH-tier items only if there is at least one — one short sentence each, no more analysis than that.
> - Do NOT mention NORMAL-tier items. They are provided for your context only, not for inclusion in the output.
> - Do not add a "what's working" section unless nothing rises to WATCH or CONCERN — if the period is genuinely clean, one sentence saying so is the entire response.
> - Maximum length: 4 sentences. If there are multiple CONCERN items, prioritize the hours shortfall and highest-rate pattern first and compress the rest into a single combined sentence rather than exceeding the limit.
> - Never invent detail not present in the data below.
>
> DATA: `{summary, hoursShortfall, patterns}`
>
> Write the review now — verdict first, no preamble, no restating these instructions.

### Team Review

Sends an array of each person's tiered payload plus their name — no per-entry detail.

**System prompt:**
> You are writing a short note for the manager of this team about everyone's timesheets for {periodStart} to {periodEnd}, at a Denver-area MSP. This is for the manager's eyes only.
>
> Each person's data below has already been tiered (CONCERN / WATCH / NORMAL) using fixed, pre-computed thresholds. Do not re-judge these tiers. Your only job is to summarize who has CONCERN or WATCH items, in plain language, as briefly as possible.
>
> Rules:
> - One-sentence overall verdict first: team is normal, or name the single most significant outlier immediately.
> - List every person with a CONCERN-tier item (including hours shortfall), one short sentence each, with the actual numbers.
> - Mention WATCH-tier people only if there's room after CONCERN items are covered — one clause each, not a full sentence.
> - Do NOT mention anyone who is entirely NORMAL-tier. Do not pad the response with a full roster recap.
> - If nobody has a CONCERN or WATCH item, say so in one sentence and stop — do not manufacture an outlier.
> - Maximum length: 6 sentences total regardless of team size.
> - Never invent detail not present in the data below.
>
> DATA: `[{ name, summary, hoursShortfall, patterns }, ...]`
>
> Write the review now — verdict first, no preamble.

### Ask AI

Free-form chat, grounded in whatever's currently loaded. Unlike the two reviews above, this one *does* get the raw entry list (so it can answer arbitrary questions), plus the same tiered SUMMARY/PATTERNS/HOURS SHORTFALL blocks so recurrence questions ("has this been a repeat issue?") are tier-grounded instead of re-eyeballed from raw entries. If the loaded set is too large (150+ entries), it falls back to a per-day/per-resource hour summary with no individual notes.

**System prompt:**
> You are helping a manager at an MSP review timesheet data for their team. This dataset is [for {resourceName}][, covering {periodStart} to {periodEnd}]. Answer the question using ONLY the data provided below — do not invent facts. Be concise and specific. If the question names someone other than who this data is for, say so plainly rather than guessing. If SUMMARY includes a total the question is asking about (total hours, client-facing %, flag count, etc.), use that exact number rather than summing the ENTRIES list yourself. If asked whether something is a repeat issue or a real pattern, use PATTERNS' tiers (CONCERN/WATCH/NORMAL) as authoritative — they are already computed from the full period, don't re-judge recurrence from ENTRIES yourself.
>
> SUMMARY (already computed): `{JSON}`
>
> PATTERNS (already tiered — treat these tiers as authoritative): `{JSON}`
>
> HOURS SHORTFALL (already tiered): `{JSON}`
>
> ENTRIES (for citing specific days only): `{JSON}`

### AI Note Review

Runs in batches of ~30 entries.

**System prompt:**
> You are reviewing MSP timesheet notes for quality. For each entry, decide whether the note plausibly matches the hours logged and is specific enough to be useful.
> "vague" = note is too generic to know what was actually done. "mismatch" = the note does not plausibly match the hours logged (e.g. "sent email" for 45 minutes). "ok" = note is fine. "other" = doesn't fit the above but is still worth a manager's attention.
>
> Return ONLY a valid JSON array, one object per entry, no markdown fences, no preamble, no extra text:
> `[{ "entryId": <id>, "flag": "ok|vague|mismatch|other", "reason": "one short sentence" }]`

---

## Settings reference

All centrally managed via Azure App Configuration — Hub Admin only. Everyone else sees current values, read-only.

| Setting | Default | Purpose |
|---|---|---|
| Client-facing target % | 70 | Target line shown against each person's actual % (color-coded, doesn't affect tiering). |
| Expected daily hours | 8 | Drives the Under-8h day badge and the hours-shortfall calculation. |
| Gap threshold (minutes) | 60 (currently set to 1 for testing) | Minutes of uncovered time between entries before the **Gap** flag/chip fires. Does not affect the inline color-tiered gap display, which always shows every gap. |
| Business hours start / end | 08:00 / 17:00 | Window used for the **After Hours** flag and the inline gap display's leading/trailing boundaries. |
| Late entry threshold (business days) | 3 | Days after the work date before an entry is flagged **Late**. |
| Minimum note length (characters) | 15 | Below this, or empty, or a single word → **Thin Note**. |
| Pattern WATCH rate | 0.2 | Occurrence rate above this → WATCH tier. |
| Pattern CONCERN rate | 0.5 | Occurrence rate above this → CONCERN tier. |
| Hours shortfall CONCERN threshold | 4 | Hours short of the period's expected total before it's a CONCERN. |
| Note mismatch multiplier | 2 | How many times over a keyword's expected ceiling counts as a mismatch. |
| Note mismatch keywords | seed list, see below | JSON table of short-task phrases → expected duration ceiling (minutes). |
| AI review max range (months) | 1 | Widest date range **AI Note Review** can run over in one pass. |
| Internal company name / ID | "Anchor Network Solutions" / `0` | Name is a display label only — ID `0` is what actually drives internal/client classification. |
| Excluded resources | (none) | Autotask resource IDs hidden everywhere in this tool. |
| AI model | claude-haiku-4-5 | Used by all four AI features. |

---

## Notes & limits

- **The note-mismatch keyword table is a guess, not calibrated data.** It needs real short-task note phrasing from the SD team to be trustworthy — flag this explicitly if reviewing accuracy with Pat.
- **The gap threshold is currently set to 1 minute** for testing — expect this to move to 5–10 minutes for normal use, at which point far fewer entries will trip the Gap flag/chip (the inline color-tiered gap display is unaffected either way, since it always shows every gap regardless of this setting).
- **Autotask deep link** ("Open in Autotask") is confirmed against this instance's actual timesheet URL format, not a guess.
- **"Mark reviewed in Hub"** is a Hub-only tracking flag, not an Autotask approval of any kind — real approval still happens in Autotask.
- Resources shown here are read via the shared read-only Autotask key; nothing in this tool writes back to Autotask.
