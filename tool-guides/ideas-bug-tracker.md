# Ideas & Bug Tracker

A single place to submit, vote on, and track ideas and bug reports across the Hub — replacing the old "Report a Bug" email (no visibility, no status, no feedback loop) and the informal idea-submission process.

Pinned in the sidebar between **Help** and **Settings**, visible to every signed-in user regardless of role. It isn't part of the customizable Tools list and can't be hidden or reordered.

## Ideas vs. Bugs

Two separate tabs, not a combined list — the type is fixed by whichever tab you're on when you submit.

Each has its own status lifecycle:

- **Ideas:** New → Under Review → Planned → In Progress → Shipped (Declined and Duplicate are terminal, reachable from any status)
- **Bugs:** Open → Acknowledged → In Progress → Fixed (Won't Fix and Duplicate are terminal, reachable from any status)

## Voting

Click the vote arrow on any item to add your vote — click again to remove it. One vote per person per item; this is a priority signal ("+1"), not a like/approval.

## Sorting, closed items, and hiding

- **Sort** — the dropdown in the toolbar offers **Priority** (vote count, default), **Newest**, **Status**, **Alphabetical**, and **Created By**.
- **Closed section** — items in a terminal status (Shipped/Declined/Duplicate for ideas; Fixed/Won't Fix/Duplicate for bugs) automatically move into a collapsed **Closed** section below the active list, so resolved items don't clutter what you're actively tracking. Click to expand.
- **Hide** — click the eye-slash icon on any row to hide it from your own view. This is **local to your machine only** — not synced across devices, not visible to anyone else, and doesn't affect the item itself in any way. A **Hidden** section (same collapsed pattern) lets you bring hidden items back into view.

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

### Getting told when someone posts something new

The bell only ever covered items you were *already* involved with, which meant a brand-new idea or bug notified nobody — there was no way to find out people were posting things without going and looking.

**⚙ Notify me**, next to the bell, is where you turn that on. It has two settings, and **both are off for everyone until you switch them on yourself** — nobody is opted in by default, including admins, and nobody can opt you in.

- **New ideas and bugs** — when anyone posts something new, you get a **Teams message** from the Hub bot *and* it appears in your bell here. This is the one that closes the gap above.
- **Replies and updates on things I follow** — a **Teams message** when someone comments on an item you follow, or when its status changes. Replies have always shown up in your bell regardless of this setting; what this switches on is the Teams message.

Three things worth knowing:

- **You're never notified about your own actions.** Submitting an idea, commenting on one, or changing a status doesn't Teams-message you or add to your own badge, even with the settings on.
- **A status change counts as an update.** If you follow a bug and someone marks it Fixed, that reaches you the same way a reply does — you don't need to watch the item to find out what happened to it.
- **If you opt in and no Teams message ever arrives**, the Hub bot probably isn't set up for you personally yet — mention it to Mike rather than assuming it's off. Everything still shows in the bell either way.

It's deliberately here beside the bell rather than buried in Settings: since nothing notifies anyone until they opt in, a switch nobody can find would be a feature that does nothing.

### Following an item you didn't submit

Open any idea or bug and there's a **Follow** button under its status. It works both ways:

- **Follow** an item you care about but had nothing to do with — someone else's bug that affects your clients, an idea you want to see land. You then get its updates like any item of your own.
- **Following ✓** means you're already getting updates, either because you clicked Follow or because you're involved — you submitted it, you were merged into it, or you've commented on it. Clicking it again **stops** them.

That second half is the part that didn't exist before. Commenting on something used to sign you up permanently, with no way off; now one click walks away from a thread that's gone noisy, and it holds — muting beats being the submitter, beats being merged in, beats having commented.

Two details that come up:

- **Changing a status doesn't sign you up.** Admins move a lot of items through statuses, and being enrolled in every one of them would be unmanageable. If you triage something and *do* want to track it, click Follow.
- **Merging keeps your choice.** If an item you muted gets merged into another, the mute follows it — you don't get quietly re-subscribed by someone else's cleanup.

## Admin actions (`hub.admin`)

- **Change status** — an editable dropdown replaces the read-only status badge on the detail page.
- **Merge duplicates** — "Merge into…" searches for the item to keep, then shows both side by side before confirming. Everything from the duplicate — votes (deduplicated, so someone who voted on both ends up counted once), comments, attachments, and the list of who reported it — moves onto the surviving item. The duplicate is marked `Duplicate` and hidden from the list, but a direct link to it redirects to the item it was merged into instead of showing a dead page.
- **Delete** — permanently removes an item and everything attached to it (votes, comments, attachments). Not part of the original design — added as an admin cleanup utility so test or junk items don't require direct database access. If you delete an item that other items were merged into, those items are reset back to a normal open status rather than left stuck as an orphaned "Duplicate."

## Notes

- This is the first Hub feature built purely against the Sprint 2 Azure SQL + Functions backend (not a migration of an existing tool) — see [Roadmap](../getting-started/roadmap.md) for the Sprint 2 migration this was built on top of.
- **Teams messages are live for new ideas and bugs, for replies, and for status changes** — all opt-in, see [Notifications](#notifications) above. Email digests are still not a thing. Teams messages are sent by the Hub's own bot rather than through Microsoft Graph, because Graph has no way for an unattended app to send a Teams chat message at all — every permission that can do it has to act as a signed-in person.
- **First load after being idle can take up to ~a minute.** The backing SQL database runs on Azure's **Serverless** compute tier, which auto-pauses after a period of no activity to save cost. The first request after a pause has to wait for the database to resume before it can query anything — this shows up as Ideas & Bugs (or anything else touching the Azure SQL backend) taking noticeably longer to load than usual. Once resumed, it stays warm and every subsequent load is fast, even across app restarts, until it's idle long enough to auto-pause again. This is a deliberate cost/latency tradeoff, not a bug — decided to keep as-is rather than switch to an always-on tier.
