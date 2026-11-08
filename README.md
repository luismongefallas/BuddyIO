# BuddyIO — Version 2A

A privacy-focused, offline-capable Progressive Web App for iPhone and Android. This version builds directly on BuddyIO Version 1.

## What's new

- Supportive, balanced, or direct coaching styles
- Richer branching conversations for fatigue, time pressure, and motivation
- User-defined reasons for goals, referenced in conversations
- Randomized pre-check-ins in each activity's chosen lead-time window
- Activity-time check-ins and up to two unanswered follow-ups (configurable)
- Quiet hours and a four-hour cap on follow-ups
- Event history used for lightweight, local personalization
- Downloadable JSON backup under Settings → Export my Buddy data

**Important limitation:** In-app check-ins run only when the PWA is open and visible. iOS does not allow this JavaScript timer to send notifications while the app is closed or locked. True push notifications require a backend scheduler and Web Push setup in Version 2B.

## Preserve your existing iPhone data

The code deliberately uses the **same** localStorage key as V1: `buddy-pwa-data-v1`. It adds fields without clearing the original activities, logs, check-in records, or settings. Existing activities, history, and progress should remain after a same-origin deployment.

**Do not** uninstall the Home Screen app, clear Safari website data, use a different GitHub Pages address, or click **Erase all Buddy data** if you want to keep existing information. iOS may partition storage by installation/browser context; test in the same Home Screen installation you already use. For added safety, preserve a copy of your old GitHub commit and, after updating, export a backup from Settings.

## Update your GitHub Pages site

Repository: https://github.com/luismongefallas/BuddyIO

1. Download and extract the Version 2A ZIP.
2. Open the `buddyio_v2a` folder inside the extracted archive.
3. Open your GitHub repository's **Code** tab and select **Add file → Upload files**.
4. Drag the **contents** of `buddyio_v2a` into the upload area. Upload `index.html`, `app.js`, `style.css`, `sw.js`, `manifest.webmanifest`, `README.md`, and the `icons` folder. Do **not** upload the ZIP or nest the files in an extra folder.
5. GitHub may warn about files with the same names. Confirm replacing the existing files, then click **Commit changes**. If the web upload flow won't overwrite files, edit or delete the existing files first, then upload the replacements and commit.
6. Go to **Actions** and wait for the GitHub Pages deployment to succeed.
7. Visit https://luismongefallas.github.io/BuddyIO/ and refresh. On iPhone, fully close and reopen the installed app if necessary. The service worker cache version changed to `buddy-v2a.0.1`.

## Suggested tests

1. Verify all existing activities and history remain.
2. Set an activity for 30–60 minutes from now; set its advance window to 30 minutes. Keep the app open and visible to test the check-in.
3. Choose **I'm unsure → Low energy → Try a smaller step**.
4. Change the time and confirm the new time appears on the dashboard.
5. Open **Settings** and change the coaching style and follow-up limit.
6. Mark the activity complete and confirm it doesn't receive further follow-ups.
7. Export a JSON backup.

## Technical notes

No server, account, paid APIs, or analytics. User data remains in localStorage on that browser installation. This is a client-side MVP, not a guaranteed notification or medical/wellness intervention system.
