# Receipt Drop

A phone web app that scans receipts into PDFs and files them straight into a OneDrive folder.

- **Scan:** opens your phone camera. Tap Scan again for each extra page. Each page can be rotated, removed, or switched between clean black-and-white and original color.
- **Name:** files are named from the date, vendor, and amount, for example `2026-10-21_Hilton-Chicago_212.40USD.pdf`. They go into `Receipts/2026/10/` by default. You can change both patterns in Settings, and you can add `{trip}` to either.
- **Save to OneDrive:** uploads directly through Microsoft's OneDrive API, so you don't need to email anything. If you're offline, for example on a plane, the receipt waits in the Outbox and uploads by itself when you reconnect.
- **Email:** opens your phone's share sheet with the PDF attached, for the email-to-folder routine described below.

## 1. Host it (one time, free)

The app is 6 static files. Any HTTPS host works. GitHub Pages is the easiest:

1. Create a new GitHub repository, for example `receipt-drop`, and upload all the files in this folder.
2. In the repository, go to Settings → Pages, set Source to "Deploy from a branch", choose `main` / root, and save.
3. Your app will be at `https://<your-username>.github.io/receipt-drop/`.
4. Open that address on your phone and use **Add to Home Screen** (Safari: Share → Add to Home Screen). It then opens like a regular app.

## 2. Connect OneDrive (one time, about 5 minutes)

Microsoft requires every app that writes to OneDrive to be registered. You register this one under your own account, so only you can use it.

1. Go to https://entra.microsoft.com → **App registrations** → **New registration**. Signing in with your personal Microsoft account works.
2. Name: `Receipt Drop`. Supported account types: **Accounts in any organizational directory and personal Microsoft accounts**.
3. Redirect URI: choose platform **Single-page application (SPA)** and paste your app's exact address, for example `https://<your-username>.github.io/receipt-drop/`. The app's Settings screen shows this address so you can copy it.
4. Click Register, then copy the **Application (client) ID**.
5. In the app, open ⚙︎ → Connect OneDrive, paste the ID, and tap **Sign in to OneDrive**. Accept the permission to read and write your files.

The chip in the top corner turns green when you're connected. You stay signed in on that phone.

## 3. Optional: email-to-folder routine

You can't email a OneDrive folder directly. The workaround is an automation that watches your inbox and saves the attachments:

- **Power Automate** (included with most Microsoft 365 work and school accounts): use the trigger *When a new email arrives (V3)* with Subject Filter `#receipt` and Include Attachments = Yes. Add *Apply to each* over Attachments, then *OneDrive: Create file* with Folder = your receipts folder, File name = Attachment Name, and File content = Attachment Content.
- **Personal Outlook.com / OneDrive accounts:** Zapier or IFTTT can do the same thing ("New email with attachment → Upload file to OneDrive").

In the app's Settings, enter the send-to address and subject tag. When you tap Email, the address is copied to your clipboard and the PDF is attached in your mail app. The share sheet can't fill in the recipient, so paste it in.

Direct upload is faster and works offline, so use the email route only if you prefer it.
