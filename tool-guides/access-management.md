# Access Management

> **What this screen actually controls today: nothing yet.** Access Management is real —
> every change made on it is saved permanently and logged. What it does **not** do yet is
> decide what anyone actually sees in the Hub. Sidebar access for every tool, for every
> person, still comes from the same SharePoint lists described in
> [Roles & Permissions](../getting-started/roles-and-permissions.md) — nothing on this
> screen has any effect on that yet. A later update will switch the app over to read from
> here instead; until then, treat this page as a preview of how access will work, not as
> something that changes anyone's access if you use it today.

Access Management is where roles and individual permissions will eventually be managed
for the whole Hub, replacing the **Hub Role Matrix** and **Hub User Overrides**
SharePoint lists. It's being built and rolled out in stages so it can be checked
carefully before it's switched on — this page describes the part that exists so far.

---

## Who can open it

Access Management is a permission of its own, called **Access Admin**. It's granted to a
**specific person**, never to a role — nobody gets it automatically by job title,
department, or by holding any other permission in the Hub, including the general Admin
role. Right now, only Mike holds it, since it's brand new.

If you have it, you'll see an **Access Management** entry at the bottom of the sidebar,
below the regular tool list. If you don't have it, there's nothing to see — the entry
simply isn't there, and opening the screen directly shows a plain "you don't have
access" message.

**A brief "Checking access…" label can appear in that same spot right after the app
launches**, in place of the real entry. That's not an error — the app confirms your
permission with a quick check every time it starts, and that check occasionally takes a
moment longer than usual. The label resolves into the real entry within a few seconds; it
never grants anything by itself, and it's only ever shown to someone this machine has
previously seen holding Access Admin.

Because whoever holds Access Admin can grant it to anyone else — including themselves —
it's treated as the single most sensitive permission in the Hub. There's also a safety
net: if this permission were ever accidentally removed from everyone, someone holding the
Hub's own top administrator role can still get into this screen to fix it. That's an
emergency path, not a normal one, and using it is recorded the same way everything else
here is — the Audit Log tab (below) calls these entries out distinctly, so it's easy to
confirm the emergency path hasn't quietly become the normal one.

---

## The Users tab

This is a list of everyone who has ever signed into the Hub, with the role and
permission information currently on file for each of them.

| Column | What it shows |
| --- | --- |
| **Person** | Name and email address |
| **Directory title** | Their job title from Microsoft 365. This is blank for **everyone** right now — pulling titles in automatically hasn't been built yet, so a blank title isn't a data problem, it's just not wired up |
| **Roles** | Every role currently assigned to them, as chips. Anyone with zero roles gets a **"No role — pending"** chip instead |
| **Source** | Whether the role came from the one-time import that read the old system when this screen was first built (**auto**), or was set by hand on this screen (**manual**) |
| **Tools** | How many tools this person's roles and grants add up to. A red **−N** next to it means N of those are explicitly blocked (see below) |
| **Access** | Click anywhere on the row to expand it and see exactly why — see the next section |

A search box filters by name, email, or title; two dropdowns filter by role and by
status (active, deactivated, or pending). A deactivated person's row shows their history
but can't be expanded — there's nothing left to change on it.

**A "pending" person** is someone who has signed in, is active, but currently holds
neither a role nor an individual grant here. When there's at least one, a banner appears
at the top of the tab telling you how many and offering to filter the list down to just
them. Dismissing the banner only lasts for your current session — reopening the app (or
signing back in) brings it back if the same people are still pending.

**When the two systems disagree.** Because this screen was seeded from a one-time
snapshot of the old system, a person's real Entra security-group membership can drift
away from what's recorded here afterward — someone gets added to a group in Microsoft
365, but nothing here has been told. When that happens, an extra amber chip appears next
to their roles showing what Entra still says (hover it to see when that was last
checked). It's worth reviewing before the day this screen starts actually controlling
access, so those two records agree by then — it isn't an error today, just a heads-up.

---

## Reading what someone can actually see

Click a person's row to expand it. This is the most detailed part of the screen, and the
reason it exists is to answer one question precisely: **exactly which tools can this
person reach, and why — including which ones are actively blocked versus simply never
granted in the first place.**

The panel splits into two lists:

**Granted** — every tool this person can currently reach, with a small label on each
explaining how:
- **via [Role name]** — they get it because they hold that role, and that role includes
  this tool.
- **Individual grant** — it was given to them personally, regardless of their role.

**Not visible — and why** — this starts with anything **explicitly blocked**, each
labeled either **"Denied — overrides [Role]"** (they'd normally get it through that role,
but it's been specifically turned off for them) or plain **"Denied"** (it was never
available through a role either). Underneath that sits one more line, always shown even
when nothing is blocked: *"N more tools simply aren't in this person's roles. Nothing is
denying them; no role grants them."*

That last line is the whole point of this panel. A tool a person can't see might be
something actively taken away from them, or it might simply be something they were never
given — those are two very different situations, and this is where you tell them apart
rather than guessing from a blank space in a table.

A blocked tool always wins over a grant. If someone somehow had a tool both granted and
denied at once, denied is what applies.

---

## Changing what someone can do

Everything below writes a permanent record — every change is logged with who made it and
when, and every one of them can be reviewed afterward on the **Audit Log** tab (below).

### Assigning or removing an ordinary role

Inside the expanded panel, under **Roles held**, each role appears as a chip with a small
**×** to remove it, and a dropdown plus **Assign** button lets you add another one. Both
ask for a quick yes/no confirmation — nothing more, since adding or removing an ordinary
role isn't treated as a special event. A person can hold more than one role at a time.

### Promoting someone to Admin

Admin is treated differently from every other role, because it's the broadest one — it
grants effectively everything except the handful of tools described below. Picking
**Admin** from the role dropdown (or using the **Change role** button in the panel's
header) opens a dedicated confirmation instead of the plain one, spelling out exactly who
this is, what they currently hold, and what they're gaining.

**Important: this adds Admin on top of what someone already has — it does not replace
their existing roles.** If you want their old roles gone too, remove them separately.

### Granting, denying, or clearing an individual tool

The **Add override** button opens a small editor, separate from roles entirely, for
handing someone (or taking away from them) one specific tool regardless of what their
roles say. Pick a tool from the list — organized by the same categories the Tools tab
shows — and choose:

- **Grant** — gives them this one tool personally, even if none of their roles include it.
- **Deny** — blocks this one tool for them specifically, even if a role they hold would
  otherwise grant it. This shows up on the Granted/Not visible panel as **"Denied —
  overrides [Role]."**
- **Clear** — removes whichever override is currently set, so their access reverts to
  whatever their roles alone provide.

Existing overrides for this person are listed underneath, each with who set it and when,
and its own **Clear** button. A tool that keeps its own separate, dedicated permission —
Payroll Review and Payroll Processing today — isn't offered here at all, since granting
or denying it on this screen would create a row that looks like it does something and
doesn't.

Re-selecting the effect a tool already has (granting an already-granted tool, for
example) is disabled rather than silently re-run — doing nothing still writes a fresh
record otherwise, discarding the original reason and date the override was actually set.

### Granting or revoking Access Admin

Access Admin — the permission that controls this screen itself — gets its own treatment
everywhere in this editor, because it's the one action in the Hub that can hand over
control of the Hub. Selecting it always routes through a dedicated confirmation that
requires **typing the person's full name** before it will proceed, in both directions:

- **Granting** it works the same way as any other override, just with the extra
  confirmation.
- **Revoking** it works the same way — pick **Clear** for this permission from an
  existing override row (or from the editor above), confirm by typing the name, and it's
  removed immediately. There's no separate "Deny" for this one permission: since no role
  ever grants it in the first place, blocking it would only add a permanent note saying
  so, so **Clear** is the only option offered, and it's the honest one.
- **You cannot remove your own Access Admin permission from this screen**, even by
  mistake — the button for it simply isn't offered on your own row. Another Access Admin
  can remove it for you, or, if nobody else currently holds it, the emergency Entra
  fallback described above still works.

### Deactivating someone

**Deactivate** marks a person as no longer active. It doesn't erase anything — every role
they held is kept exactly as it was, so reactivating them later restores the same
picture. Since this screen doesn't control real access yet, deactivating someone here
doesn't take anything away from them today; it only updates the record here, ready for
when it does.

---

## The Roles tab

This is where a role's own tools are defined — what it grants, before it's assigned to
anyone.

On the left is the list of every role, each showing how many people hold it and how many
tools it currently grants, plus a **+ New role** button. On the right, once you pick a
role, is the actual grid of tools for that role — and this is offered **three different
ways to look at and edit the exact same thing**, switchable at the top:

- **Matrix** — every role as a column, every tool as a row, click any checkbox to toggle
  it. Because every role is visible at once here, this is the one view where you can
  change a *different* role than the one currently selected on the left.
- **Checklist** — the selected role's tools only, grouped into categories with an
  on/total count per group.
- **Chips** — the selected role's tools only, split into two simple lists: what's in the
  role now (click to remove) and what else is available (click to add).

Whichever one you switch to is remembered for you the next time you open this tab.
Nothing about which tools are actually granted depends on which view you're looking at —
switch freely.

Changes don't save as you go — they stage until you press **Save grid**, which is
disabled until you've actually changed something. **If someone else saves a change to
the same data while you're mid-edit, your save is refused** with a plain explanation
rather than silently overwriting what they just did; the screen re-reads the current
state so you can see what actually changed before trying again.

**Three tools never appear in any role's grid, on any of the three views:** Payroll
Review and Payroll Processing keep their own separate, dedicated permission entirely
outside this system (matching how they already work today), and Access Management itself
is only ever granted to a named person, the way [Grant Access Admin](#granting-or-revoking-access-admin)
above does it — never through a role. A note at the bottom of the grid names the two
payroll tools specifically; Access Management isn't offered as a grid option at all, so
there's nothing to toggle there in the first place.

### Creating a role

**+ New role** asks for a display name (what people see, e.g. "Field Services"), a role
key (a short lowercase id, auto-filled from the name — **this can't be changed once
created**, so double-check it), and an optional description. A brand-new role starts
with no tools granted; add them on the grid afterward.

---

## The Tools tab

A read-only list of every tool the Hub's own tool registry currently knows about, and how
each one becomes visible to someone. There are no controls on this tab on purpose — a
tool key can't be hand-typed, added, renamed, or deleted here, so a role or an override
elsewhere on this screen can never reference a tool that doesn't actually exist.

| Column | What it shows |
| --- | --- |
| **Tool key** | The internal identifier a role or override actually references |
| **Display name** | What the tool is called in the sidebar |
| **Category** | Its grouping — the same categories the Roles tab's Checklist view and the override editor's tool picker use |
| **Granted by** | How it becomes visible: a count of the roles that include it; **Dedicated gate** for Payroll Review and Payroll Processing, which keep their own separate permission outside this whole system; or **By individual grant only** for Access Management itself, which is never handed out through a role |
| **People** | How many currently-active people these tables *would* grant this tool to today, counting roles and individual grants together and letting a deny win the way it always does. This is **not** the same as how many people can actually open the tool right now — nothing on this screen controls real access yet |

A small "synced" timestamp in the corner shows when this list was last refreshed. That
refresh happens automatically whenever a Hub admin launches the app — not on a
schedule, and not the moment a new tool ships — so a tool added to the Hub won't show up
here until the next time someone with the Hub admin role opens it. That's a different
permission from Access Admin (the one that gets you onto this screen at all) — Access
Admin is granted to a named person and never through a role, so holding only Access Admin
does not, by itself, trigger this refresh.

---

## The Title Mapping tab

Where a job title will eventually be mapped to a role automatically, once that's switched
on in a later update. **Nothing here does anything yet** — nobody's job title is stored
in the Hub today, so no mapping has ever been checked against a real person. The
**Matched** column always shows a plain dash for that reason, on every row, and will keep
doing so until titles arrive. Think of this tab as configuration being prepared ahead of
time, not a feature that's currently live — existing people are never re-assigned by a
mapping either way; a mapping would only ever apply the moment someone first signs in.

A mapping is a **pattern** matched against a job title, plus the role it assigns if that
pattern matches. The pattern grammar:

- The grammar is case-insensitive, and extra spaces are collapsed, so `Network Engineer`
  and `network   engineer` would be treated the same once there's a real title to
  compare against.
- `*` matches any run of characters, including none — a wildcard.
- `|` separates several alternative patterns in one mapping, e.g. `*network
  engineer*|*noc technician*` matches either.
- Everything else is literal — a pattern containing `.`, `(`, or `+` matches those exact
  characters, not a special meaning.
- A pattern made of nothing but wildcards and spaces (`*`, `**`, `* *`) is refused —
  matching literally everyone isn't really "matching a title."

Mappings are checked **top to bottom**, in the order shown, and the **first one that
matches wins** — everything below it never gets a chance for that title. The **▲**/**▼**
buttons next to each row reorder them. If someone else changes the list of mappings while
you're reordering, your reorder is refused with a plain explanation rather than silently
landing on top of theirs, and the table refreshes to whatever the current order actually
is so you can try again from there.

**Remove** doesn't delete a mapping outright — it retires it, and a retired mapping stops
counting toward precedence entirely (its Order cell shows a dash instead of a number).
Tick **Show removed** to see retired mappings alongside the active ones, and **Restore**
brings one back into the active list.

**Payroll can't be assigned through a mapping** — that role is only ever assigned by
hand, the same rule the Roles tab enforces for its own grid, and a manual assignment is
never overwritten later by an automatic title match.

---

## The Audit Log tab

Every write this screen has ever made — role assignments and removals, granting,
denying, or clearing an individual override (including Access Admin itself), creating a
role or saving the permission grid, deactivating or reactivating a person (nothing on
this screen controls real access yet, so neither changes what they can actually reach
today), every title mapping change, and a **System & migration** category for
everything else this table records automatically rather than from a click on this
screen: the Tools tab's own registry sync (see above), the one-time historical import
that seeded these tables from the old Hub Role Matrix and Hub User Overrides SharePoint
lists at cutover, and a record every time someone reaches this screen through the
emergency Entra fallback with no real Access Admin grant — in one running feed, newest
first.

- A **category** dropdown narrows the feed to one kind of change.
- **Break-glass only** shows just the entries where someone reached this screen through
  the emergency Entra fallback described above, rather than a real Access Admin grant —
  worth checking now and again, since it should normally stay empty.
- The **search box** matches against an entry's summary, the person it affected, and who
  made the change.
- Two date fields narrow the feed to a range.

All of these run on the server against the full log, not against whatever page happens to
already be loaded in your browser — so narrowing to an older date, or a category with a
lot of history, can never silently make an entry disappear just because it fell outside a
page that was already fetched. A running count ("Showing N of M matching entries") always
shows whether there's more than what's currently on screen, and **Load more** fetches the
next page without losing your place.

An entry with more to show carries a **Details** section — expand it to see the exact
before/after values the server recorded for that change (for example, a reorder's full
before-and-after list of mapping ids), rather than a sentence reconstructed after the
fact.

This tab has its own access rule, separate from the rest of the Hub: it's readable only
by someone holding Access Admin, unlike the Hub's shared activity log elsewhere in the
app, which any signed-in user can query.

---

## What's not built yet

- **No automatic role assignment from job title yet.** That's why the Directory title
  column on the Users tab is blank for everyone, the Title Mapping tab's Matched column
  always shows a dash, and the "Re-sync titles" button doesn't do anything yet — all of
  that is wired up for a later update, not broken today.
- **None of this changes what anyone actually sees in the Hub yet** — see the notice at
  the top of this page. That's the biggest thing to keep in mind while this screen is
  being built out: it's safe to explore, assign roles, and get familiar with it, without
  worrying that a mistake here immediately changes someone's real access.
