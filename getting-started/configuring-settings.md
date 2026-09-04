# Configuring Your Settings

Most of Anchor Hub works out of the box — shared credentials (Pax8, Autotask read-only) are stored centrally and load automatically. The one thing you need to configure yourself is your **personal Autotask write key**, which is required to push updates to contracts.

## Opening Settings

Click the **gear icon** in the bottom-left navigation bar.

## Autotask Personal Write Key

This is required if you want to use the **Push to Autotask** buttons in the Invoice Processor. Without it, you can still view and export invoice data — you just can't push changes directly.

**To add your key:**

1. In Settings, find the **Autotask PSA — Personal Write Key** section
2. Enter your **API Username** (your Autotask login email)
3. Enter your **API Key** (found in Autotask under Admin → Resources → your profile → API Access)
4. Enter the **Integration Code** (provided by Mike — same for all staff)
5. Click **Save**

Your credentials are stored securely in Windows Credential Manager on your machine. They are never uploaded or shared.

> **Don't have an Autotask API user?** Contact Mike — he needs to create one for you in Autotask under Admin → Resources/Users (HR) → New API User.

## Verifying Your Setup

After saving, navigate to the **Invoice Processor** and load the most recent invoice. If the **Push to Autotask** buttons are active and the invoice loads without errors, your setup is complete.

## Sidebar Layout — Export / Import

**Sidebar Layout** is its own entry in the bottom-left navigation bar, just under Ideas & Bugs. *(On v4.0.0 and earlier it lives inside Settings, as the **Customize** tab — same screen, same buttons.)* Tick a tool to show it, drag the ⠿ handle to reorder, and use **Add Group** to collect tools into a named section — then **Save Layout**.

Which tools show in your sidebar, how they're grouped, and your personal quick links are all saved **on this machine only** — they don't follow your Microsoft login. Reinstalling the Hub or moving to a new computer starts you back at the defaults.

To carry your setup over:

1. On your **old** machine: **Sidebar Layout** → **Export**. Save the file somewhere you can get to from the new machine (OneDrive, a USB drive, email it to yourself).
2. On your **new** machine: sign in, then **Sidebar Layout** → **Import**, and pick the file you exported.

This brings over your sidebar layout (visible tools, groups, order, which groups are collapsed) and your personal quick links in one step — no need to redo either by hand. Do this **before** you start rebuilding your customization from scratch; there's no undo once you've reconfigured everything manually.

## Installs (`hub.admin` only)

Settings → **Installs** lists everyone who's launched the Hub: name, machine, app version, and when they were last (and first) seen — sorted most-recent-first. Useful for confirming everyone's updated past a given version, or spotting a machine that hasn't checked in in a while. Only visible to Hub Admins; hidden entirely for everyone else.

## Other Settings

Other settings (margins, excluded companies, etc.) are tool-specific and covered in each tool's guide. Most users won't need to change them.
