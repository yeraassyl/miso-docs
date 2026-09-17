---
title: "Qasaqana:Miso Privacy Policy"
---

# Privacy Policy for Qasaqana:Miso

**Last updated: September 17, 2026**

Qasaqana is building Miso ("the app"), a personal expense-tracking app. This
policy explains what happens to your data when you use it.

## Summary

Almost everything you do in Miso stays on your phone. Miso has no account
system, no analytics, no advertising, and no payment processor.

There is **one exception**, and it is the important part of this policy:
**when you import a bank statement, that PDF leaves your device.** It is sent
to our server, and from there to NuMind, the company whose extraction model
reads it. What happens to it at each step is set out below.

If you never import a statement, Miso never sends anything anywhere.

## What stays on your device

All of this is stored only on your phone, in a local database, and is never
transmitted:

- Transaction amounts, dates, merchants, and notes
- Category names, icons, and colors
- Groups you create
- Merchants Miso has remembered, and the categories you assigned them
- App settings (currency, language, theme)
- Your import credit balance

Uninstalling the app deletes all of it permanently. There is no cloud copy.

## Importing a bank statement

This is the only feature that sends data off your device. It runs only when
you choose a PDF and tap Upload.

### What is sent

- **The statement PDF itself**, in full and unredacted
- The file name you picked it under
- Your display currency and app language

That is the whole list. Miso does **not** send your category names, your
existing transactions, your notes, a device identifier, an advertising ID, an
account, or a location. Miso has no way to identify you: there is no sign-in,
and uploads are not tied to any user record.

### Step 1 — our server

The PDF is received by a server we run on Google Cloud Run in Frankfurt,
Germany (europe-west3).

- **The PDF is never written to disk.** It is held in memory for the length of
  the job and then discarded.
- **The extracted result** — the merchants, amounts and dates read out of your
  statement — is held briefly so your phone can collect it. It is stored in
  memory only (a temporary filesystem that exists solely while the server
  instance is running), is deleted **one hour** after the job finishes, and
  disappears entirely whenever the instance restarts.
- **Our logs record no financial information.** They contain a random job
  identifier, the size of the upload in bytes, its page count, and error codes.
  Not the file name, not merchants, not amounts.

Once your phone has collected the result, the statement exists only on your
phone again.

### Step 2 — NuMind

To read the statement, our server sends the PDF to **NuMind** (nuextract.ai),
a third-party document-extraction provider. This is the point at which your
statement is handled by a company other than us, so their terms govern what
happens to it.

As of NuMind's SaaS Terms and Conditions effective 20 July 2026:

- NuMind deletes submitted content and extraction results from their active
  production systems **within a maximum of 14 days** of the API call
  (section 7.3.1).
- Residual copies may remain in encrypted backup or history systems for **a
  maximum of 30 days** (section 7.3.2).
- NuMind states it will **not** use submitted content for "training,
  fine-tuning, benchmarking or improving NuMind's AI models" (section 7.4).
- **Your statement is processed in the United States.** NuMind's Data
  Processing Agreement states that each API call transfers the content "to
  NuMind's servers in the United States." That transfer is covered by EU
  Standard Contractual Clauses (Commission Implementing Decision (EU)
  2021/914), which the DPA sets out in full.
- NuMind uses its own sub-processors for hosting, databases and GPU inference,
  named in Annex III of their DPA, and commits to give 30 days' notice of
  changes to that list.
- NuMind offers an endpoint for requesting deletion of submitted content ahead
  of the 14-day window (section 7.3.3); backup copies still expire on the
  schedule above.

These are NuMind's commitments, not ours, and NuMind may change them. Their
current terms are published at
[numind.ai/terms/saas](https://numind.ai/terms/saas), and their data
processing agreement at
[numind.ai/numind-data-processing-agreement.pdf](https://numind.ai/numind-data-processing-agreement.pdf).

**What this means in plain terms:** for up to about two weeks after an import,
and up to about a month in backups, a copy of that bank statement exists on
NuMind's systems. If you are not comfortable with that, do not use statement
import — every other feature of Miso works entirely offline, and you can add
transactions by hand.

## What Miso does not do

- Does not require an account or sign-in
- Does not use analytics, crash reporting, or advertising
- Does not contain a payment processor; import credits are a counter stored on
  your phone
- Does not request access to contacts, location, camera, microphone, or any
  other device permission beyond network access for statement import
- Does not sell or share your data with anyone other than the processing step
  described above

## Data deletion

Everything on your device is deleted when you uninstall the app.

For the server side: our copy is gone within an hour of the import finishing,
without you having to ask. For NuMind's copy, the deletion windows above apply.
If you want a specific import deleted sooner than that, write to us at the
address below and we will pass the request on, though we cannot guarantee a
third party's timing.

## Backups

If your device backs up app data to Google account backup, iCloud, or a
similar OS-level service, Miso's local database may be included in that backup.
This is controlled by your device's operating system, not by Miso — see your
device's backup and restore settings.

## Children's privacy

Miso is not directed at children and does not knowingly collect data from them.

## Changes to this policy

If Miso's data handling changes, this policy will be updated and the date at
the top changed. We intend to move statement extraction onto infrastructure we
run ourselves, which would remove the NuMind step entirely; if and when that
happens, this policy will be updated to match.

## Contact

Questions about this policy: hey@qasaqana.com
