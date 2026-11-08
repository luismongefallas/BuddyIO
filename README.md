# Buddy PWA — MVP v1

Buddy is an offline-capable, installable iPhone/Android web app. No account, API key, build tools, or paid service is required to run it.

## Features
- Daily dashboard with customizable activities and goal motivations
- Time selection, rescheduling, completed/skipped status
- Randomized pre-check-in window (while app is open)
- Branching coaching conversations based on readiness and obstacles
- Local activity history, 7-day progress visualization, streaks
- LocalStorage persistence, offline app shell, installable manifest
- Quiet hours and in-app check-in toggle

## Fastest way to open on iPhone
1. Create a **free GitHub repository** and upload the contents of this folder to its root (not the ZIP itself).
2. In the repository, open **Settings → Pages**, select **Deploy from a branch**, branch **main**, folder **/(root)**, then save.
3. Wait for GitHub Pages to publish the site. Its HTTPS address typically looks like `https://YOUR_USERNAME.github.io/REPOSITORY_NAME/`.
4. Open the published URL in **Safari on iPhone**. Tap **Share → Add to Home Screen → Add**.
5. Open Buddy from your Home Screen. Activities are saved locally in that installed app's storage.

GitHub Pages is public hosting: the app source is public, but activity data is stored on each user's device, not in GitHub. Avoid entering sensitive information in a prototype. Browser data can be cleared by iOS or the user; there is no backup or sync.

## Test locally on Windows
From the directory containing index.html:

```powershell
python -m http.server 8080
```

Open http://localhost:8080 in a desktop browser. The service worker requires HTTPS or localhost.

## Limitations
- **No background push notifications.** In-app check-ins run while the app is open, including when it is brought back to the foreground. iOS will suspend the app when backgrounded; check-ins are not guaranteed to appear at the intended time.
- Check-ins are not a substitute for iOS notification alerts.
- Random pre-check-in times are created per activity per day; they are generated when the app runs, so opening the app late may miss an earlier check-in.
- No cloud sync, account, or data export; data is local to the browser/app installation.
- Coaching is rules-based, not generative AI. The coaching-style preference is saved for future personalization but does not yet change dialogue text.
- This is a prototype, not production-tested software.

## Files
- `index.html` — interface shell
- `style.css` — responsive visual styling
- `app.js` — application logic and local storage
- `sw.js` — offline caching service worker
- `manifest.webmanifest` — install metadata
- `icons/` — home-screen icons
