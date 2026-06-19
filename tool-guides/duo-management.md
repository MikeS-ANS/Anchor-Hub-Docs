# Duo Management

Duo Management automates the setup and management of Duo Security sub-accounts for clients. It handles creating new sub-accounts and enrolling servers without requiring manual steps in the Duo admin portal.

## Adding a New Sub-Account

Use the **New Sub Account** wizard when onboarding a client to Duo:

1. Navigate to **Duo Management** in the left sidebar
2. Click **New Sub Account**
3. Follow the wizard steps:
   - Enter the client company name
   - The account is created in Duo and configured automatically
4. Save the credentials — they'll be needed for the Add Duo to Server step

## Adding Duo to a Server

Use this when a client's server needs to be enrolled in their existing Duo account:

1. In Duo Management, click **Add Duo to Server**
2. Select the client from the dropdown
3. Enter the server name/details
4. The tool runs the enrollment via Datto RMM Quick Job — no manual RDP required

## Excluded Accounts

Some accounts in Duo are internal or should be skipped during bulk operations. To exclude an account:

1. Find it in the accounts list
2. Click **Exclude**

Excluded accounts are hidden from the default view but can be shown by toggling **Show Excluded**.

---

> **Guide in progress.** More detail will be added here as the tool evolves.
