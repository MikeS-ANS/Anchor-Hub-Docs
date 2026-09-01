# Access Management

> **This screen is now the real thing — in an unreleased development build.** The Hub's own
> Access Management tables decide who sees what tool, replacing the **Hub Role Matrix** and
> **Hub User Overrides** SharePoint lists described in
> [Roles & Permissions](../getting-started/roles-and-permissions.md). That cutover is now
> complete on the server side too — every server-side action the Hub takes, not just what
> the sidebar shows, checks these same tables directly, the same way tool visibility does.
> That's all genuinely true today — but only inside an unreleased development branch.
> **The released app you're using today still runs on the old SharePoint-based system**,
> unchanged, until this branch finishes and ships. Nothing in this page describes your
> access right now unless someone has specifically told you they're testing the new build
> with you. Once it ships, this page is simply how the Hub decides access from then on.

Access Management is where roles and individual permissions are managed for the whole Hub.
This page describes everything that exists today, including the parts that only apply once
the branch above ships.

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
Hub's own top administrator Entra role (`hub.admin`) can still get in to fix it — and not
just onto this screen. **That same emergency role now backs up nearly every server-side
check the Hub makes off these tables, everywhere in the app, not only Access Admin itself**
— the same safety net that gets someone into this screen also keeps a config change, a
feedback reply, or another admin-only server action from being permanently blocked by a bad
table row. These tables are always checked first, and the emergency role is only consulted
if that check comes up empty — so someone whose real access already covers what they're
doing is never recorded as having used the emergency path just because they happen to also
hold it. Only an actual use — where the emergency role is the reason something was let
through — gets recorded: the Audit Log tab (below) calls these entries out distinctly
(**Break-glass only**, further down), so it's easy to confirm the emergency path hasn't
quietly become the normal one. Simply loading a page, and having the Hub silently check
whether the emergency role *would* let someone in, is never logged by itself.

**This break-glass is now the only place the Hub checks your live Microsoft 365 sign-in
role directly instead of these tables, anywhere in the app — except the two Payroll tools'
own separate permission, described
[below](#payroll-review-and-payroll-processing-are-a-special-case). See
[What still comes from Entra](#what-still-comes-from-entra-for-now) near the bottom.**

---

## How access is actually decided now

Once the branch above ships, every tool's visibility comes from four tables this screen
manages: who someone is (**Users**), what a role includes (**Roles**), an optional
person-by-person exception on top of a role (**overrides**, part of the Users tab), and
the master list of tools that exist at all (**Tools**). Nothing about visibility is read
from SharePoint or hand-maintained anywhere else — if a tool doesn't appear in these
tables for a person, they don't see it, full stop.

**The default is no access, not "show everything."** A person with no `hub_users` row yet,
or one whose check for access genuinely failed (a network hiccup, say), is never treated as
"we don't know, so let them see it all" — they're treated as having nothing, until a real
answer says otherwise. That's a deliberate reversal from how the old SharePoint-based
system behaved (an unconfigured or unreachable matrix used to mean "show everything"), and
it's the whole reason the sections below exist: a genuinely new person now lands on a
holding screen instead of an empty sidebar with no explanation.

---

## Server-side enforcement, not just the sidebar

Everything above describes what a person *sees* — the sidebar, the holding screen. What
matters more is that the Hub's server checks these same tables before it *does* anything
sensitive, not just before it draws a menu. Hiding a button in the app was never enough by
itself — the underlying server action could always have been reached directly, skipping the
button entirely. That gap is now closed everywhere it used to exist: every server action
that used to check a signed-in person's Microsoft 365 role directly now checks these same
Roles/Users/overrides tables instead, the same way the sidebar does, with the `hub.admin`
break-glass role continuing to work as the emergency fallback described above on nearly all
of them.

A handful of admin-only actions elsewhere in the Hub — saving a global setting, replying
Officially to feedback, seeing who's currently running the app, an admin-only Client Touch
Aging or Contract Review setting, and a few others — check for the **Admin** role from this
screen's own Roles tab, the exact same role you assign someone right here. There's no
separate, second "admin badge" anywhere else in the app any more: give someone the Admin
role on this screen, and every one of those actions opens up for them too, everywhere in
the Hub, the moment the Hub next checks (see [Losing access](#losing-access) above for how
quickly a change like that takes effect, either direction).

When a single action needs to check more than one thing at once — say, whether someone
holds one of two different roles, or holds a role and a specific tool grant, both — the Hub
only actually asks these tables once per action, not once per thing being checked, so
checking more than one requirement never makes an action slower.

---

## The holding screen

Someone signing in for the very first time with no Hub account yet, or someone whose
account exists but holds neither a role nor an individual grant, sees a dedicated holding
screen instead of a bare empty sidebar. It is **not** simply "anyone who currently has no
tools" — a person holding a role that happens to grant nothing still counts as having
arrived, and gets the ordinary Hub (with an empty sidebar), not this screen; see the third
bullet below. Exactly who sees the holding screen, and who doesn't, follows these rules:

- **No Hub account exists for you yet at all** → holding screen, with a "Setting up" badge.
  This is the very first moment after signing in, before the Hub has even finished creating
  your account record.
- **Your account exists, is active, and holds no role and no individual tool grant at all**
  → holding screen, with an "Access pending" badge and the date you first showed up in the
  queue.
- **Your account exists, is active, and holds a role — even one that happens to grant zero
  tools** (an empty or misconfigured role) → you do **not** see the holding screen. Holding
  any role at all, regardless of what it actually grants, is enough to count as "arrived."
  You go to the ordinary Hub instead, just with an empty sidebar — which looks like nothing
  happened, but is a different situation from never having a role in the first place, and
  is fixed the same way any other access gap is: by giving the role some tools, or the
  person a different role.
- **You hold at least one real tool, however you got it** — through a role, or through a
  personal grant with no role at all — you never see this screen. You go straight to the
  Hub with whatever you're entitled to.
- **Your account has been deactivated** → you never see the holding screen, no matter how
  many roles or tools you hold or don't. You see an empty sidebar with no card and no
  explanation instead. This is deliberate: the holding screen's copy talks about "your
  account is being set up" and "you're in the queue," and both of those would be false
  claims about someone who was never mid-onboarding — they were removed. There's nothing
  to check for and nothing to wait on.

The screen checks for new access on its own every 5 minutes while it's on screen, and has
a **Check for access now** button for an immediate check. Either way, the moment a real
role or grant lands, it leaves the holding screen on its own and goes straight to the Hub
with the sidebar already reflecting the new access — no restart, no re-sign-in, no need to
click anything further.

---

## Losing access

Access can also be taken away while someone is already signed in and using the Hub —
whether that's a role removed, an individual grant cleared, or the whole account
deactivated. The Hub notices this on its own, in the background, roughly every 10 minutes
for as long as the app stays open — it doesn't wait for the next sign-in.

If that background check fails for some reason (a network hiccup, say), the Hub simply
keeps using whatever it last confirmed rather than guessing, and tries again about a
minute later instead of waiting the full 10 minutes — so a brief connectivity blip doesn't
strand anyone on a stale answer for long.

**If the tool someone currently has open gets revoked mid-session, they're returned to the
Home screen quietly — no pop-up, no warning dialog.** That's deliberate, not an oversight:
a dialog would announce out loud that access was just pulled, which is exactly the kind of
thing that shouldn't be broadcast to whoever happens to be looking at the screen at that
moment. They simply find themselves back on Home, and the tool is gone from the sidebar.

---

## Automatic offboarding

Once an hour, the Hub checks every active person's Microsoft 365 account directly against
Microsoft, on its own, with no button to click. If that check finds an account has been
**disabled** or **deleted entirely** in Microsoft 365, that person's Hub account is
automatically deactivated — the same as if someone had clicked **Deactivate** for them on
the Users tab — and it's recorded in the Audit Log as a **System** action, not attributed to
any human, with a summary naming the reason (disabled vs. deleted).

**This never reactivates anyone, ever, even if the M365 account comes back on.** A person
may have been deactivated in the Hub for a completely separate reason, and this check isn't
in a position to know that — only a real Access Admin, using the Users tab, can bring
someone back.

**The M365 disable-state is authoritative, and there is no per-person exemption.** The sweep
doesn't skip anyone, and there's no setting or flag anywhere in Access Management that
excuses a specific person from it. If an Access Admin reactivates someone in the Users tab
while their Microsoft 365 account is still disabled, that reactivation will be undone by the
very next hourly run — the sweep re-checks every active person's M365 account each time it
runs, so it will simply deactivate them again within the hour. To keep a Hub row active, the
Microsoft 365 account has to be re-enabled first; reactivating the Hub side alone doesn't stick.

**What this actually buys, stated plainly:** a disabled Microsoft 365 account already can't
sign in or refresh a Hub session on its own — Microsoft 365 is what handles that part, not
the Hub. So this doesn't stop someone from getting in who couldn't already. What it does do
is close the one real gap: if that person was already signed in and using the Hub at the
moment their M365 account got disabled, their session would otherwise have kept working
until it happened to expire on its own. Now, within about an hour of the disable, their Hub
account is deactivated, and within about another 10 minutes on top of that (the same
background check described in [Losing access](#losing-access) above), their sidebar goes
empty and stays that way. Mostly, though, this removes a manual step someone on the IT side
used to have to remember — offboarding an employee in Microsoft 365 no longer requires a
separate trip to this screen to also deactivate their Hub account by hand. The Users tab now
stays accurate on its own.

There's no button in the app for this — it runs on its own, every hour, silently, and there
is nothing to configure. (A way to trigger it on demand exists for troubleshooting, but it
isn't reachable from inside the app itself — see the runbook near the bottom of this page.)

---

## Payroll Review and Payroll Processing are a special case

These two tools never appear on the Roles tab's grid and can't be granted or denied as an
individual override — trying to configure them here isn't an option, and that's not an
oversight. Their sidebar visibility comes from a completely separate mechanism: whether
your live Microsoft 365 sign-in currently carries the specific Entra security role each
tool actually checks —

- **Payroll Review** needs `hub.payroll`.
- **Payroll Processing** needs `hub.payroll` **or** `hub.payrollmanager`.

This is exactly how each tool's own screens have always gated themselves — nothing about
the Access Management rebuild changed that, and nothing on this screen can override it
either way. Whatever your Entra role assignment says is what decides whether these two
tools show up for you, independent of every role and override this screen manages for
every other tool. If you're wondering why you can't grant someone Payroll Processing from
the Users tab the way you'd grant any other tool, this is why.

**This is current design, not a settled permanent decision.** Mike has flagged wanting to
revisit whether these two tools should keep their own separate Entra-based permission at
all, versus folding onto this same Hub-native system like every other tool — in his own
words, "I don't want some things in Entra and some things in the hub." That revisit hasn't
happened yet, and it's its own separate project with its own before/after comparison, not
something folded quietly into this one. Until it does, the rule above is exactly how these
two tools work today.

---

## The Users tab

This is a list of everyone who has ever signed into the Hub, with the role and
permission information currently on file for each of them.

| Column | What it shows |
| --- | --- |
| **Person** | Name and email address |
| **Directory title** | Their job title as recorded in Microsoft 365. This is read automatically — every time someone signs in, the Hub checks their current job title and stores it here |
| **Roles** | Every role currently assigned to them, as chips. Anyone with zero roles gets a **"No role — pending"** chip instead |
| **Source** | Whether the role was set by hand on this screen, came from the one-time import that read the old system when this screen was first built (both show **manual**), or was assigned automatically because a brand-new person's job title matched a pattern on the Title Mapping tab the moment they first signed in (**auto**) |
| **Tools** | How many tools this person's roles and grants add up to. A red **−N** next to it means N of those are explicitly blocked (see below) |
| **Access** | Click anywhere on the row to expand it and see exactly why — see the next section |

A search box filters by name, email, or title; two dropdowns filter by role and by
status (active, deactivated, or pending). A deactivated person's row shows their history
but can't be expanded — there's nothing left to change on it.

**A "pending" person** is someone who has signed in, is active, but currently holds
neither a role nor an individual grant here — the same person who would see the holding
screen described above. When there's at least one, a banner appears at the top of the tab
telling you how many and offering to filter the list down to just them. Dismissing the
banner only lasts for your current session — reopening the app (or signing back in) brings
it back if the same people are still pending.

**When the two systems disagree.** A person's live Entra security-group membership can
drift away from what's recorded in these tables — someone gets added to a group in
Microsoft 365, but nothing here has been told. When that happens, an extra amber chip
appears next to their roles showing what Entra still says (hover it to see when that was
last checked). Worth reviewing periodically so the two stay in agreement.

---

## Automatic role assignment on first sign-in

The very first time a genuinely new person signs into the Hub, Access Management checks
their job title in Microsoft 365 and tries to match it against the Title Mapping tab
(below) — top to bottom, first match wins, the same rule that tab already documents.
If a pattern matches, that role is assigned automatically the instant they sign in,
tagged **auto** in the Source column above so it's always clear it wasn't a person's
deliberate choice. If nothing matches — or their title isn't in the directory at all, or
there simply are no active mapping patterns configured yet — they land on the Pending
queue exactly as before, and an Access Admin assigns a role by hand, same as today.

**This only ever happens once, on someone's genuine first sign-in — and deliberately
never just because someone currently holds no role.** Those sound like the same
condition, but they're not, and the difference matters: if an Access Admin later
deliberately removes someone's role, that person now has no role too — but the whole
point of removing it was for them to hold nothing, and re-checking would silently put a
role right back the next time they opened the app. So this only ever fires for a
person's real, one-time first launch, never for "currently has zero roles" in general. A
role an admin took away stays taken away.

**A manual assignment is never touched by this, full stop.** Automatic assignment only
ever runs against a genuine newcomer who currently holds zero roles at all; it never
runs again for someone already assigned by hand, imported from the old system, or
already auto-assigned once before. The **Re-sync titles** button described just below
only ever refreshes what's stored in the Directory title column — it never assigns,
changes, or removes anyone's role, no matter how its name might sound.

**Nothing in Entra needs to be touched before a new hire's first sign-in.** The Hub's own
app registration in Microsoft 365 doesn't require anyone to be specifically assigned to
it before they can sign in, so there's no separate Entra-side approval step standing
between a brand-new employee's very first launch and their arrival on the Pending queue
here. The moment someone signs in, they're either auto-assigned or Pending — there's
nothing to configure in Entra first. That's also true of anyone who already holds a role
in Entra today but has simply never opened the Hub yet — their eventual first sign-in
runs through this exact same auto-assign-or-Pending path, with nothing pre-loaded for
them ahead of time.

**Every Access Admin gets a Teams message the moment someone new lands on Pending.**
It's deliberately brief and doesn't name the person, their email, or their job title —
on purpose, not as an oversight. A Teams message can be forwarded, screenshotted, or read
on someone's personal phone, and it sits entirely outside every permission check the Hub
itself has. So the message is only ever a pointer, telling you that someone new signed
in and needs a role — the actual name and details stay behind the Access Admin gate,
right here on the Pending banner and the Users tab, same as always. If the message never
arrives for some reason — Teams itself briefly unavailable, say — the Pending Users
banner on this screen is still the reliable, permanent record of who's waiting; the Teams
message is a heads-up, never the record itself.

## The "Re-sync titles" button

At the top of the Users tab, **Re-sync titles** re-reads everyone's current job title
from Microsoft 365 in bulk and updates what's stored in the Directory title column. It's
a title refresh only — it never assigns, removes, or changes anyone's role, regardless
of what a title now matches; a role is only ever assigned automatically at a brand-new
person's first sign-in, described above. If someone's title genuinely comes back empty
from Microsoft 365, this clears what's stored here to match reality; if a person's title
simply couldn't be read at all (a temporary directory hiccup), whatever was already
stored is left alone rather than guessed at — and the summary afterward always says so
honestly rather than reporting a clean sync that didn't happen, naming the specific
failure reason when one occurred.

**This button works.** It went through a period earlier in the build where it failed on
every single person — an internal bug where the Hub asked Microsoft 365 for the right
people, got the right answers back, and then failed to match those answers to its own
records, so it counted everyone as "not found" even though the directory had answered
correctly. That's fixed. Automatic assignment at a new person's first sign-in was never
affected by it either way — that path looks up one person's title at a time rather than
in bulk, and was unaffected by the bulk button's problem throughout.

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

**Payroll Review and Payroll Processing never appear in either list here** — see
[Payroll Review and Payroll Processing are a special case](#payroll-review-and-payroll-processing-are-a-special-case)
above for why, and for how their visibility actually works.

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
  break-glass fallback described above still works.

### Deactivating someone

**Deactivate** marks a person as no longer active, and their real access is taken away
along with it — a deactivated person's sidebar goes empty within the same background
check window described in [Losing access](#losing-access) above, if they're already
signed in, or immediately on their next sign-in. Deactivating someone does not erase
anything — every role they held is kept exactly as it was, so reactivating them later
restores the same picture instantly. A deactivated person also never sees the holding
screen (see above) — they simply see nothing, since the holding-screen copy about "your
account is being set up" would be a false claim about someone who used to have access and
no longer does.

**This isn't the only way someone ends up deactivated.** The [Automatic offboarding](#automatic-offboarding)
sweep further down does the same thing on its own, once an hour, for anyone whose
Microsoft 365 account gets disabled or deleted — worth knowing before assuming every
deactivated row on this tab was someone's deliberate click.

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
outside this system (see [above](#payroll-review-and-payroll-processing-are-a-special-case)),
and Access Management itself is only ever granted to a named person, the way
[Grant Access Admin](#granting-or-revoking-access-admin) above does it — never through a
role. A note at the bottom of the grid names the two payroll tools specifically; Access
Management isn't offered as a grid option at all, so there's nothing to toggle there in
the first place.

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
| **Granted by** | How it becomes visible: a count of the roles that include it; **Dedicated gate** for Payroll Review and Payroll Processing, which keep their own separate Entra-role-based permission outside this whole system (see above); or **By individual grant only** for Access Management itself, which is never handed out through a role |
| **People** | How many currently-active people these tables grant this tool to today, counting roles and individual grants together and letting a deny win the way it always does. **For a Dedicated gate tool (Payroll Review, Payroll Processing) this always reads 0** — these tables never grant a dedicated-gate tool to anyone, by design, so there's nothing here for them to count. It does **not** mean nobody can see the tool; it means this screen isn't where that count comes from. Who actually sees a dedicated-gate tool is decided by Entra sign-in roles instead — see [above](#payroll-review-and-payroll-processing-are-a-special-case) — and this screen doesn't total that up anywhere |

A small "synced" timestamp in the corner shows when this list was last refreshed. That
refresh happens automatically whenever someone holding the **Admin** role (the Roles tab
role described [above](#server-side-enforcement-not-just-the-sidebar)) launches the app —
not on a schedule, and not the moment a new tool ships — so a tool added to the Hub won't
show up here until the next time an Admin opens it. That's a different permission from
Access Admin (the one that gets you onto this screen at all) — Access Admin is granted to a
named person and never through a role, so holding only Access Admin does not, by itself,
trigger this refresh.

---

## The Title Mapping tab

Where a job title is matched to a role automatically, the moment someone signs in for
the very first time — see [Automatic role assignment on first sign-in](#automatic-role-assignment-on-first-sign-in)
above for how that works.

**Before this ships, this tab needs at least one active pattern configured, or nobody
gets auto-assigned.** With no active mappings, every genuinely new sign-in lands on
Pending regardless of their title, which isn't wrong, just not useful — seeding real
patterns here is a needed step before this branch is handed over for real use, not an
optional nice-to-have.

The **Matched** column shows how many currently-recorded directory titles a pattern
would match, calculated live against real data — it's a real, working number, not a
placeholder. It's the fastest way to sanity-check a new pattern: a **0** here today means
nobody *currently in the directory* has a matching title — it does **not** necessarily
mean the pattern is wrong. A pattern written for a role nobody on staff happens to hold
right now (a role meant for a future hire, say) will legitimately read 0 until that
person actually joins and signs in — the column only ever reflects people who have
already signed in and had a title successfully read, never people who might join later.
**Existing people are never re-assigned by editing or adding a mapping** — a mapping only
ever applies the moment someone signs in for the very first time, never retroactively.

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

Every write this screen has ever made — role assignments and removals (including one
assigned automatically at a person's first sign-in, described above — it's recorded here
the same as a manual one, just worded so it's clear nobody actually clicked anything),
granting, denying, or clearing an individual override (including Access Admin itself),
creating a role or saving the permission grid, deactivating or reactivating a person
(including the automatic hourly [offboarding sweep](#automatic-offboarding)'s own
deactivations, in the same **User status** category as a manual one but attributed to
**System — M365 offboarding sweep** rather than a person), every title mapping change, and
a **System & migration** category for everything else this table records automatically
rather than from a click on this screen: the Tools tab's own registry sync (see above), a
bulk **Re-sync titles** run, the one-time historical import that originally seeded these
tables from the old SharePoint lists, and a record every time the emergency Entra
break-glass role is the actual reason some server action was allowed to proceed, anywhere
in the app — not only on this screen — with no real grant from these tables covering it —
in one running feed, newest first.

- A **category** dropdown narrows the feed to one kind of change.
- **Break-glass only** shows just the entries where the emergency Entra fallback described
  above was the reason something was let through, anywhere in the Hub, rather than a real
  grant from these tables — worth checking now and again, since it should normally stay
  empty.
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

## What still comes from Entra, for now

As of this page's latest update, only two things about the Hub still read your live
Microsoft 365 sign-in role directly, rather than these tables:

- **The `hub.admin` break-glass fallback**, described near the top of this page and in
  [Server-side enforcement](#server-side-enforcement-not-just-the-sidebar) above, keeps
  working independently of this screen's own tables, everywhere it's used across the Hub
  — not only on this screen. An emergency door that only worked as long as the tables it
  might need to fix were themselves working wouldn't be much of an emergency door. This
  one is permanent by design; no future phase is expected to remove it.
- **Payroll Review and Payroll Processing's own dedicated `hub.payroll`/
  `hub.payrollmanager` roles**, described
  [above](#payroll-review-and-payroll-processing-are-a-special-case) — current design,
  with a revisit Mike has asked for still pending (see that section for what that means).

The eight retired general roles (`hub.manager`, `hub.delivery`, `hub.tam`,
`hub.strategic`, `hub.projects`, `hub.finance`, `hub.sales`, `hub.wsd`) were **deleted
from the Entra app registration on 2026-09-01**, after the 4.0.0 release made them
unread. Only the three roles above remain defined.

### The "Hub" security groups stay — they do more than assign roles

The ten **Hub \*** security groups in Entra (Hub Admin, Hub Delivery, Hub Finance, Hub
Manager, Hub Payroll Manager, Hub Projects, Hub Sales, HUB Strategic, Hub TAM, Hub WSD)
must **not** be deleted as part of any access cleanup, even though most of the app roles
they used to assign are gone. Verified directly against Azure's own role assignments
(2026-09-01): nine of the ten — every one except Hub Payroll Manager — carry the **Key
Vault access that every employee's copy of the Hub depends on** to read shared
credentials (the Autotask connection, the shared Strety connection, and its
refresh-token self-heal). Deleting a group would silently break those integrations for
everyone in it. On top of that, **Hub Admin** is what assigns the `hub.admin` break-glass
role, and **Hub Payroll Manager** assigns the live Payroll Processing role. Treat the
groups as Key Vault access rosters first and role feeds second — membership changes are
fine; deletion is not.

**The general "Admin" badge several individual tools use for their own admin-only buttons
and settings** — things like Company Mapping, Client Touch Aging, and various Settings
panels — used to be a third item on this list, reading the `hub.admin` Entra role
directly. It no longer is: it now reads the exact same **Admin** role you assign on this
screen's Roles tab, the same way every other tool's visibility works. See
[Server-side enforcement](#server-side-enforcement-not-just-the-sidebar) above.

---

## The two old SharePoint lists are retired from live use

**Hub Role Matrix** and **Hub User Overrides** are no longer read anywhere in the app for
a real access decision — not for the sidebar, not for the holding screen, and not for
either of the Entra-based checks described above (the break-glass fallback or the two
payroll tools' dedicated roles). The only thing that still reads either list at all is a
one-off, read-only comparison tool used to double-check the new tables agree with what the
old lists would have said, before this branch is finally handed over for use — it never
writes to either list and never feeds a live decision.

Once this branch is fully handed over, the plan is to rename both lists in SharePoint with
a **`RETIRED — `** prefix rather than delete them outright, so the historical record stays
available without anyone mistaking either list for something still live.

---

## Cutover runbook (for whoever finishes this branch)

A short list of what's left to do around this feature specifically, before the branch is
actually handed over for real use:

1. **Seed real Title Mapping patterns.** Covered above — with none configured, every
   new sign-in lands on Pending. Do this any time; it's inert until the branch ships and
   harmless in the meantime.
2. **Re-run the parity comparison once more, right before the actual merge/release.**
   Data can drift between now and then, so the last comparison run should be as close to
   the real cutover as practical. This is a DevTools call, not a screen — from the app's
   developer console:
   `await window.api.accessParityRun({ entraCsvPath: '<path to a fresh Entra app-role export>' })`.
   The `entraCsvPath` matters: the Hub's roles are assigned through security **groups**
   (4 people assigned directly plus 10 groups, as of the last real run), not directly to
   people, so an ordinary Entra portal user list is not an equivalent substitute — it
   needs to be a real app-role assignment export, expanded through group membership, the
   same way the Hub's own sign-in token resolves it. Once this branch actually ships,
   this export stops being necessary at all — every sign-in through the new build
   repopulates the Hub's own stored copy of each person's roles on its own.
3. **Three people are missing from the Users tab entirely and need one manual step
   before they can be granted anything here: Brian French, Chris Heck, and Jacee Dobbs.**
   All three hold their Hub roles only through Microsoft 365 security groups, never a
   direct assignment, and the historical import that originally seeded most of this
   screen's data only carried direct assignments, not expanded group membership — so
   their rows were never created. A ready-to-run script
   (`Downloads/seed-french-dobbs-heck-hub-users.sql`) creates their three rows the same
   way the original import would have; run it once in the Azure Portal's Query Editor,
   then assign each of them a role on the Users tab as normal. Their Payroll Processing
   access is unaffected either way — it rides their `hub.payrollmanager` group
   membership directly and doesn't depend on a Users tab row existing at all.
4. **The M365 offboarding sweep (see [Automatic offboarding](#automatic-offboarding)
   above) can be triggered on demand for troubleshooting, but only from outside the
   app** — there's no in-app button or developer-console call for it. Trigger it
   manually from the Azure Portal (open the Function App, find the
   `accessOffboardSweepRun` function, use its built-in Test/Run) or with an
   authenticated call to the underlying route. The hourly automatic run is the normal
   path and needs nothing done to it.
5. **Optional cleanup: one retired (soft-deleted) Title Mapping row.** There's a single
   already-retired mapping sitting in the table from earlier testing — it's fully inert
   today (it never counts toward precedence and is already hidden unless **Show removed**
   is ticked on the Title Mapping tab), so there's nothing that needs doing. If you'd
   rather have it gone from the database entirely rather than just hidden, that has no
   in-app button and needs a one-line SQL delete run in the Azure Portal's Query Editor —
   purely cosmetic, never urgent.
6. **Rename the two retired SharePoint lists** with the `RETIRED — ` prefix described
   above, once the branch is actually live and nothing could still need the old lists for
   comparison. This is Mike's own click in SharePoint, done after the merge — not an
   in-app or code step.

---

## What's not built yet

- **The `hub.admin` break-glass fallback stays permanent, by design** — see
  [What still comes from Entra, for now](#what-still-comes-from-entra-for-now) above for
  why an emergency door has to work independently of the system it might need to fix.
  There's no future phase planned to remove it.
- **Whether Payroll Review and Payroll Processing keep their own separate Entra-based
  permission, or fold onto this same system like every other tool, is an open question
  Mike has asked to revisit** — as its own separate project, with its own before/after
  comparison, not a small follow-on to this one. See
  [Payroll Review and Payroll Processing are a special case](#payroll-review-and-payroll-processing-are-a-special-case)
  above.
- **This whole feature is still on an unreleased development branch.** Everything on this
  page describes how access works inside that branch — the app you're actually using today
  still runs on the old SharePoint-based system until that branch ships. Watch for an
  announcement when it does; nothing about your day-to-day access changes silently.
