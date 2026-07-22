# Ideas & Bug Tracker

A single place to submit, vote on, and track ideas and bug reports across the Hub — replacing the old "Report a Bug" email (no visibility, no status, no feedback loop) and the informal idea-submission process.

Pinned in the sidebar between **Help** and **Settings**, visible to every signed-in user regardless of role. It isn't part of the customizable Tools list and can't be hidden or reordered.

## Ideas vs. Bugs

Two separate tabs, not a combined list — the type is fixed by whichever tab you're on when you submit.

Each has its own status lifecycle:

- **Ideas:** New → Under Review → Planned → In Progress → Shipped (Declined and Duplicate are terminal, reachable from any status)
- **Bugs:** Open → Acknowledged → In Progress → Fixed (Won't Fix and Duplicate are terminal, reachable from any status)

## Voting

Click the vote arrow on any item to add your vote — click again to remove it. One vote per person per item; this is a priority signal ("+1"), not a like/approval. Lists sort by vote count (**Priority**) by default, or switch to **Newest** via the sort control.

## Comments

Anyone can comment on any item, including closed or merged ones — there's no reason to lock discussion just because the status changed. Comments show oldest-first.

When a `hub.admin` posts a comment, it's automatically flagged **Official** and rendered with a distinct highlighted style — this isn't a toggle an admin picks, it's derived from their role.

Status changes also show up in the comment thread as a system-generated line (e.g. "Status changed from Open to Acknowledged"), so the history of what happened to an item lives in one place instead of a separate log.

## Attachments

Attach a screenshot or file from an item's detail page (not the initial submit modal — the item needs to exist first since attachments are stored per-item). Files upload directly to SharePoint (`ANS-Company Shared → Anchor Hub → feedback-attachments/`); clicking an attachment opens it in your browser.

## Export

Check the box on any row (or **Select all**) to build a Markdown export of those items — the button shows a count once you've selected at least one. Each exported item includes its title, status, submitter(s), vote count, full description, attachments (as clickable links to their SharePoint file), and the complete comment thread in order. Useful for sharing a set of items outside the Hub or archiving a batch before a cleanup.

## Notifications

The 🔔 bell in the toolbar shows updates on anything you're following — anything you submitted, anything merged into an item you're attached to, or anything you've commented on yourself. It covers new comments and status changes from other people (not your own actions). A red badge on the bell and on the sidebar nav item shows the unread count; click **Mark all read** to clear it.

## Admin actions (`hub.admin`)

- **Change status** — an editable dropdown replaces the read-only status badge on the detail page.
- **Merge duplicates** — "Merge into…" searches for the item to keep, then shows both side by side before confirming. Everything from the duplicate — votes (deduplicated, so someone who voted on both ends up counted once), comments, attachments, and the list of who reported it — moves onto the surviving item. The duplicate is marked `Duplicate` and hidden from the list, but a direct link to it redirects to the item it was merged into instead of showing a dead page.
- **Delete** — permanently removes an item and everything attached to it (votes, comments, attachments). Not part of the original design — added as an admin cleanup utility so test or junk items don't require direct database access. If you delete an item that other items were merged into, those items are reset back to a normal open status rather than left stuck as an orphaned "Duplicate."

## Notes

- This is the first Hub feature built purely against the Sprint 2 Azure SQL + Functions backend (not a migration of an existing tool) — see [Roadmap](../getting-started/roadmap.md) for the Sprint 2 migration this was built on top of.
- No email or Teams notifications yet (in-app only) — that depends on Microsoft Graph scopes not yet in use elsewhere in the Hub.
- **First load after being idle can take up to ~a minute.** The backing SQL database runs on Azure's **Serverless** compute tier, which auto-pauses after a period of no activity to save cost. The first request after a pause has to wait for the database to resume before it can query anything — this shows up as Ideas & Bugs (or anything else touching the Azure SQL backend) taking noticeably longer to load than usual. Once resumed, it stays warm and every subsequent load is fast, even across app restarts, until it's idle long enough to auto-pause again. This is a deliberate cost/latency tradeoff, not a bug — decided to keep as-is rather than switch to an always-on tier.
