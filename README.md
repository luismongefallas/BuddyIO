# BuddyIO Version 2B — iPhone background push prototype

**Status:** source code prepared and JavaScript syntax checked. Not deployed to Cloudflare or tested on a physical iPhone. The Worker uses the `web-push` Node package with Cloudflare Workers' `nodejs_compat` setting; verify compatibility during deployment. If deployment reports an unsupported Node API, the push sender needs a compatible implementation before this feature can work.

## What changes

- Keeps the exact existing localStorage key `buddy-pwa-data-v1` to preserve your installed BuddyIO's activities and history.
- Adds a push registration interface in Settings, using the installed iPhone Home Screen app's service worker.
- Includes a Cloudflare Worker backend with a D1 database and a cron job that checks every minute for due reminders.
- Sends a pre-check-in, due reminder, and up to two follow-ups. Quiet hours are applied when generating the schedule. On responses or changes, the app replaces the remote schedule when online.
- The server receives the activity name, scheduled time, push subscription and a random device token; it does not receive the personal motivation, coaching conversations, or full history. Do not enter sensitive information in activity names if using push.

## Step A — Update your existing GitHub Pages site

1. **First export your Buddy data:** in the current app, Settings → Export my Buddy data. Keep the JSON backup.
2. Extract the ZIP and open the `buddyio_v2b` folder.
3. Upload the **contents** of this folder to your existing GitHub repository's root, replacing `index.html`, `app.js`, `style.css`, `sw.js`, `manifest.webmanifest`, `README.md`, and `icons/` as needed. **Do not upload the `worker/` folder into the public GitHub Pages root**; it contains backend source and deployment settings. You can keep it locally or in a separate repository.
4. Commit changes. Wait for GitHub Pages deployment. Open https://luismongefallas.github.io/BuddyIO/ in your existing installed app and verify your data remains. Do not delete or reinstall the Home Screen app.
5. Until you configure the Worker, BuddyIO works as Version 2A, without closed-app push.

## Step B — Set up the free-tier Cloudflare Worker (Windows PowerShell)

Prerequisites: free Cloudflare account, Node.js LTS installed, PowerShell terminal. Cloudflare free plans have usage limits; check current limits before relying on it. A cron job every minute uses roughly 1,440 invocations per day before counting API calls and may not be supported on every account/plan.

1. Copy the `worker` folder to a local development folder, then run:

```powershell
cd worker
npm install
npx wrangler login
npx wrangler d1 create buddyio-push
```

2. Copy the returned D1 `database_id` into `wrangler.toml`, replacing `REPLACE_WITH_YOUR_D1_DATABASE_ID`.
3. Initialize the database:

```powershell
npm run db:init
```

4. Generate VAPID keys locally:

```powershell
npx web-push generate-vapid-keys
```

5. Save the **public and private** keys as Worker secrets using these commands. Wrangler prompts for each value; do not commit secrets to GitHub:

```powershell
npx wrangler secret put VAPID_PUBLIC_KEY
npx wrangler secret put VAPID_PRIVATE_KEY
npx wrangler secret put VAPID_SUBJECT
```

For `VAPID_SUBJECT`, enter a contact URI such as `mailto:your-email@example.com` (replace with your real email). `APP_ORIGIN` is already configured for `https://luismongefallas.github.io` in `wrangler.toml`.

6. Deploy:

```powershell
npm run deploy
```

Copy the HTTPS `*.workers.dev` URL printed by Wrangler.

## Step C — Enable notifications on iPhone

1. iOS 16.4 or later is required. Open BuddyIO **from its Home Screen icon** (not a Safari tab).
2. Go to **Settings** and paste your Cloudflare Worker HTTPS URL into the background notifications field.
3. Tap **Enable background notifications**. Allow notification permission when iOS prompts.
4. Check the confirmation status. If an error appears, use that error to troubleshoot Worker deployment or permission settings.
5. Create a test activity 10–15 minutes in the future. Put BuddyIO in the background or lock the iPhone. Wait for the scheduled notification; Cloudflare cron and Apple push delivery can be delayed.
6. Tap the notification to return to BuddyIO. You can then use its existing conversation and task controls.

## Operational limits and known issues

- The app sends up to **seven days of schedules** each time it syncs. Open BuddyIO at least weekly to refresh upcoming reminders. It does not yet maintain indefinite recurring schedules on the server.
- A push notification is a prompt to open the app; **reply buttons directly inside the iOS notification are not implemented**.
- Background notifications depend on Apple Push delivery, iOS settings, Cloudflare uptime, network access, and successful backend configuration. Exact delivery time is not guaranteed.
- Follow-up notifications are **pre-scheduled** and are canceled when the user responds and BuddyIO can sync online. If a user responds offline or the sync fails, a follow-up may still arrive. This is a prototype, not guaranteed exactly-once delivery.
- Some in-app prompts may duplicate push alerts while the app is open. A future version can coordinate them.
- Notification schedules are only updated when the app opens or changes are made while online. Changing quiet hours or activity times while offline won't immediately update remote reminders.
- The Worker uses a random local bearer token rather than full user accounts. Protect access to your device. There is no multi-device sync or account recovery.
- Cloudflare Worker and D1 configuration require separate deployment; uploading only the frontend to GitHub does **not** activate push.
- Disable in Buddy Settings to unsubscribe and delete the server's stored device and pending reminders.

## Files

- `index.html`, `app.js`, `style.css`, `sw.js`, `manifest.webmanifest`, `icons/` — GitHub Pages frontend.
- `worker/src.js`, `worker/wrangler.toml`, `worker/schema.sql`, `worker/package.json` — Cloudflare backend.

## Rollback

Revert the GitHub repository to the previous Version 2A commit if you need to restore the old interface. Local data is preserved by design because both versions use `buddy-pwa-data-v1`. Keep your exported backup regardless.
