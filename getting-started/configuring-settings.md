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

## Other Settings

Other settings (margins, excluded companies, etc.) are tool-specific and covered in each tool's guide. Most users won't need to change them.
