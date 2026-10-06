---
title: "Qasaqana:Miso Privacy Policy"
---

# Privacy Policy for Qasaqana:Miso

**Last updated: October 6, 2026**

Qasaqana is building Miso ("the app"), a personal expense-tracking app. This
policy explains what happens to your data when you use it.

## Summary

Almost everything you do in Miso stays on your phone. Miso has no account
system, no analytics, and no advertising. Miso takes no payments itself;
credits are bought through Google Play.

The important part of this policy: **when you import a bank statement, that PDF
leaves your device.** It is sent to our server, and from there to NuMind, the
company whose extraction model reads it. What happens to it at each step is set
out below.

Two other things also leave your phone. Each has its own section below:

- **Buying credits:** payment runs through Google Play. The app sends our
  server only which credit pack you bought and a Google Play receipt code.
- **App and device check:** when it starts, the app asks Google to confirm it is
  the genuine Miso app on a genuine Android device. Each request to our server
  carries the result.

If you never import a statement, Miso sends no financial data anywhere. Its
only other network traffic is the Google device check and, when you buy
credits, Google Play and our server's purchase check.

## What stays on your device

All of this is stored only on your phone, in a local database, and is never
sent to us:

- Transaction amounts, dates, merchants, and notes
- Category names, icons, and colors
- Groups you create
- Merchants Miso has remembered, and the categories you assigned them
- App settings (currency, language, theme)
- Your import credit balance

Uninstalling the app deletes all of it from your phone. We keep no copy of it
anywhere; your phone's own backup may include it (see Backups).

## Importing a bank statement

This is the only feature that sends bank data off your device. It runs only
when you choose a PDF and tap Upload.

### What is sent

- **The statement PDF itself**, in full and unredacted
- The file name you picked it under
- Your display currency and app language

That is the whole list. Like every request, the upload also carries the
device-check token and is logged as described below. Miso does **not** send
your category names, your existing transactions, your notes, a device
identifier, an advertising ID, an account, or a location. There is no sign-in,
and uploads are not tied to any user record. The only thing that could link a
request to you is the IP address in the request logs, which are deleted after
30 days.

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
- **Our logs record no financial information.** Google Cloud keeps request
  logs: a line for every request to the server, not only imports. Each line
  holds the time, your IP address, the user-agent string (which names the app's
  HTTP library and its version), the path requested, and the response status,
  size and duration. Our server also writes its own log lines: a random job
  identifier, counts (bytes, pages and transactions), the outcome of the app and
  device check, and error codes. On a purchase verification error, those lines
  also hold an 8-character fingerprint of the token (see Buying credits). No
  statement contents and no transaction details are logged: not the file name,
  not merchants, not amounts. We use the logs for security and to fix errors.
  They are deleted automatically after 30 days.

After your phone collects the result, our server keeps it only until the
one-hour mark above. NuMind's copies follow the timeline in Step 2.

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

## Buying credits

You can buy import credits through Google Play. The payment runs entirely
through Google Play, and Google processes it under
[Google's own privacy policy](https://policies.google.com/privacy). Miso never
sees your card or bank details.

After a purchase, the app sends our server exactly two things about it:

- The product ID, which says which credit pack you bought
- The Google Play purchase token, an opaque receipt code

Our server asks Google's Play Developer API whether the purchase is real, paid
and not already used. It then replies with the number of credits.

- **Our server stores nothing about the purchase.** The only trace is in the
  logs described under Step 1, where the request has a line like any other. On a
  verification error, the server's own log lines keep only an 8-character
  fingerprint (a hash) of the token, never the token itself.
- **Your credit balance is kept only on your phone,** in the app's local
  database, the same place as your transactions.

## App and device check

When it starts, the app asks Google to confirm two things: that it is the
genuine Miso app as distributed by Google Play, and that it is running on a
genuine Android device. Google's answer comes back as a check token, which the
app refreshes periodically. This uses Firebase App Check and Google Play
Integrity, which are both Google services.

- **Google makes the assessment** from information on your device and in Google
  Play, under
  [Google's own privacy policy](https://policies.google.com/privacy).
- **Each request to our server carries the current token.** From the check, our
  server receives only that short-lived signed token, which says whether the
  check passed. Our server verifies the token and does not store it. The token
  carries the app's ID, not a device identifier.
- **Our logs record only the outcome of the check** (valid, missing or invalid)
  and which API path was called.
- **Requests that fail the check are refused.**

## What Miso does not do

- Does not require an account or sign-in
- Does not use analytics, crash reporting, or advertising
- Does not process payments itself: credits are bought through Google Play, and
  the balance is a counter stored on your phone
- Does not request access to contacts, location, camera, microphone, or any
  other device permission beyond network access and Google Play billing
- Does not sell your data, and shares it only with the services this policy
  names: NuMind to read statements, Google Cloud to host our server, and Google
  Play and Firebase for purchases and the device check

## Data deletion

Everything on your device is deleted when you uninstall the app.

For the server side: our copy of the statement is gone within an hour of the
import finishing, without you having to ask. For NuMind's copy, the deletion
windows above apply. If you want a specific import deleted sooner than that,
write to us at the address below and we will pass the request on, though we
cannot guarantee a third party's timing.

Purchase verification and the app and device check leave nothing stored on our
side, apart from the log lines described under Step 1. Request logs have a line
for every request to our server, imports included, and are deleted
automatically after 30 days.

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
