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

Because whoever holds Access Admin can grant it to anyone else — including themselves —
it's treated as the single most sensitive permission in the Hub. There's also a safety
net: if this permission were ever accidentally removed from everyone, someone holding the
Hub's own top administrator role can still get into this screen to fix it. That's an
emergency path, not a normal one, and using it is recorded the same way everything else
here is.

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
when, even though there's no page yet to browse that log directly (that's coming in a
later update).

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

### Granting Access Admin

The **Add override** button grants the Access Admin permission described above — the one
action on this screen that can hand over control of the screen itself. Because of that,
it asks you to **type the person's full name** before it will proceed, on top of the
usual confirmation. There is currently no button to take this back once granted; removing
it has to be done outside this screen.

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
is only ever granted to a named person, the way [Grant Access Admin](#granting-access-admin)
above does it — never through a role. A note at the bottom of the grid names the two
payroll tools specifically; Access Management isn't offered as a grid option at all, so
there's nothing to toggle there in the first place.

### Creating a role

**+ New role** asks for a display name (what people see, e.g. "Field Services"), a role
key (a short lowercase id, auto-filled from the name — **this can't be changed once
created**, so double-check it), and an optional description. A brand-new role starts
with no tools granted; add them on the grid afterward.

---

## What's not built yet

- **Two more tabs exist but aren't built out** — Tools (a read-only list of every tool in
  the registry) and Title Mapping (auto-assigning a role based on someone's job title)
  both currently show a placeholder. So does the Audit Log tab — every change made on
  this screen today is already being recorded permanently, there just isn't a page yet to
  browse that history in the app itself.
- **No automatic role assignment from job title yet.** That's why the Directory title
  column is blank for everyone and the "Re-sync titles" button doesn't do anything yet —
  both are wired up for a later update, not broken today.
- **Access Admin, once granted, can't be revoked from this screen.** The "Add override"
  button only grants it; there's no matching button to take it away, and no general way
  yet to grant or block any other individual tool for one person the way the old Hub User
  Overrides list could.
- **None of this changes what anyone actually sees in the Hub yet** — see the notice at
  the top of this page. That's the biggest thing to keep in mind while this screen is
  being built out: it's safe to explore, assign roles, and get familiar with it, without
  worrying that a mistake here immediately changes someone's real access.
