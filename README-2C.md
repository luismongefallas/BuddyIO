# BuddyIO Version 2C — Adaptive follow-up update

## Install safely
1. On your current iPhone BuddyIO, go to Settings and **Export my Buddy data**. Save the JSON somewhere safe.
2. Upload the **contents** of this package's `buddyio_v2c` folder to the root of your existing GitHub BuddyIO repository, replacing `app.js`, `index.html`, `sw.js`, and other matching frontend files. Keep the `icons/` directory intact. Do **not** upload `worker/` to GitHub Pages.
3. Wait for GitHub Pages deployment. Reopen your *existing* Home Screen BuddyIO installation. Do not uninstall it or clear Safari website data.
4. Confirm activities, history, and the Cloudflare Worker URL are still present. Your push subscription and original `buddy-pwa-data-v1` localStorage key are retained.
5. Test: choose a task -> Check in -> I'm unsure -> Not enough time -> Give me 15 minutes -> choose a follow-up interval. Close BuddyIO and wait for the iOS banner.
6. Mark a task done and verify future reminders for that task are removed after the next successful sync.

## Changes
- Adds explicit 10/15/30/60 minute follow-up choices and schedules them through the existing `/sync` endpoint.
- Accepting a smaller task schedules a 10-minute check-back.
- Suppresses old pre/due/automatic follow-up notifications after an explicit snooze.
- Preserves existing activities, check-ins, settings, progress, and push credentials.
- Keeps existing Cloudflare Worker source/configuration unchanged. **No Wrangler redeploy, D1 migration, or new VAPID keys required.**

## Limits
- Push delivery is best effort and requires an active iOS Home Screen PWA subscription, connectivity, and permission.
- Quiet hours can suppress an explicitly requested follow-up; it will not be sent at a later time automatically.
- The existing Worker deletes reminders before sending; delivery failures cannot currently be retried.
- The existing app may show in-app check-ins as well as push notifications when open.
- This is a beta release; test before inviting others.
