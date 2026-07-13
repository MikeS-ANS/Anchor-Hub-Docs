# Values & Norms

Brings ANS's 5 Core Values and 22 Cultural Norms — "The Anchored Way" — into the Hub as a daily touchpoint, instead of living only in Strety and the printed pamphlet.

## Home Screen Widget

A widget above Quick Links on the Home screen features one Value or Norm each day. It's the same item for everyone on the same calendar date — no per-user state, just a deterministic daily rotation through all 27 items.

- **Badge** (top-right) shows whether it's a Value or a Norm.
- **Blurb** shows the item's short description immediately, then swaps in a fuller AI-generated blurb once it's ready. That blurb is generated once per day and shared across the whole team — everyone sees the identical text, and it doesn't re-generate every time someone opens the Hub.
- **Surprise Me 🎲** swaps in a different random item (never today's item) with its short description — not the AI blurb. Click it as many times as you like; reopening the Hub returns to today's item.
- **View All →**, or clicking anywhere else on the widget, opens the full library.

## Library View

Open **Values & Norms** from the left sidebar to see all 27 items as a card grid.

- **Search** filters by title or description in real time.
- **Filter bar** — All / Values / Norms — shows a live count for each and narrows the grid.
- **Click a card** to flip it and read the full definition on the back; click again to flip back. Multiple cards can be flipped at once.
- **Today** chip and a highlighted border mark whichever card matches the home screen widget's daily item.

## Admin: Refresh

A **Refresh** button appears in the library header for `hub.admin` users only. All content (titles, descriptions, active/inactive status) is managed directly in the "Hub Values & Norms" SharePoint list — the Hub is read-only. After editing an item there, click Refresh to clear the local cache and pull the latest version immediately, instead of waiting for the once-a-day automatic refresh.

## AI Prompt

Sent to Hatz.ai (Claude Haiku, via the `/v1/anthropic/messages` endpoint — `main/ipc/valuesNorms.js`, `generateBlurb`) once per day for the featured item. No model picker — single hardcoded model, matching the "no settings tab" v1 scope.

**Prompt (built fresh per day, for the current cycle item):**
> You are helping reinforce the culture of Anchor Network Solutions, a managed service provider (MSP) in Denver, CO with ~35 staff.
>
> Today's featured {Value/Norm} is: "{Title}"
>
> Definition: "{FullDescription}"
>
> Write exactly 2–3 sentences (no more) that bring this to life for today. Connect it to something practical and relatable — it could relate to MSP work, client relationships, team dynamics, or a current professional theme. Be specific, warm, and actionable. Do not repeat the definition verbatim. Do not use corporate jargon. Write as if you're a thoughtful colleague, not a policy document.
>
> Output plain prose only — no title, no heading, no markdown formatting (no #, *, -, or bullet points), just the 2–3 sentences.

The response also gets a defensive strip in code in case the model prepends a markdown heading anyway. If generation fails, the widget falls back to the item's `ShortDescription` — never generated for Surprise Me selections, only the daily cycle item.

## Notes

- Deactivating an item in SharePoint (`Active` = No) removes it from the rotation and the library without deleting it.
- If the AI blurb generation fails for any reason, the widget falls back to the item's short description — it never shows an error on the home screen.
