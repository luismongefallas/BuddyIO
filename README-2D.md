# BuddyIO Version 2D — recurring schedules and all-day check-ins

## Installation
1. In the **existing installed BuddyIO**, go to Settings → Export my Buddy data. Save the JSON backup. Do **not** uninstall the iPhone Home Screen app or clear its site data.
2. Upload the frontend `index.html`, `app.js`, `style.css`, `sw.js`, `manifest.webmanifest`, and `icons/` to the same GitHub Pages repository, preserving paths. Do not publish `worker/`.
3. Keep the existing Cloudflare Worker, D1 database, VAPID secrets, and push URL unchanged.
4. After GitHub Pages updates, close and reopen the Home Screen app. Existing data stays in localStorage key `buddy-pwa-data-v1`.

## Features
- Recurrence: every day (legacy default), chosen weekdays, every N days from a start date, or once.
- All-day activities: 2–12 evenly spaced check-ins between the selected first and last reminder. Each requires a separate **I'm doing it** confirmation. A repeated tap in the same slot does not count twice; confirmations are saved per day. Completing all slots marks the day complete.
- One-time/single activities: **I'm working on it** records in-progress and offers 10/15/20/30/60-minute follow-ups or manual completion.
- Push events are scheduled for up to seven days, ordered by time, with the existing 150-event backend cap. Open Buddy regularly to refresh the seven-day window.

## Important limitations
- iOS push notifications **open the app**; confirmation requires tapping inside BuddyIO, not on a notification action button.
- All-day confirmations count only after the relevant time slot begins; late confirmations count for the current slot only. Earlier missed slots cannot be backfilled. If you skip slots, daily completion may become impossible; the app still shows partial progress.
- Changing an activity's schedule mid-day does not automatically migrate previous slot confirmations. Edit future schedules carefully.
- Quiet hours suppress reminders rather than defer them; avoid placing required check-ins during quiet hours.
- Existing records are retained; local storage is still device-specific, not cloud backed up.
- The existing Worker deletes due reminders before attempting delivery. Push reliability/security improvements remain separate work.
- Do not upload the `worker` directory to public GitHub Pages.

## Test plan
1. Confirm existing activities, settings, and notification subscription survived.
2. Create an all-day test with 2 check-ins 10 minutes apart, confirm the first, and ensure the second cannot be confirmed early.
3. Confirm the second after its reminder and verify 2/2 complete.
4. Create a selected-weekday activity and verify it appears only on those days.
5. For a single activity, select **I'm working on it → 15 minutes**, close the app, and confirm a push arrives.
