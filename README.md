# Lifetrakcia 2.0 — web app + Cloudflare AI engine

**Branding:** Created by Ronak.  
**Includes:** responsive dark UI, task manager, habit check-ins, goals/progress, daily reflection, local backup/import, installable PWA shell, Licia text AI endpoint, optional browser speech input/output.

## Deploy to Cloudflare Pages (no API key in the website)

1. Upload this project to a Git repository, or use Cloudflare Pages Direct Upload with the project folder/ZIP (Git integration is easiest for Functions).
2. In Cloudflare Dashboard → Workers & Pages → Create → Pages, connect the repository.
3. Build settings: **Framework preset: None**, **Build command: leave blank**, **Build output directory: `public`**. Keep `functions/` at the project root; Cloudflare Pages Functions are deployed from there.
4. In the Pages project → Settings → Variables and Secrets, add **`GEMINI_API_KEY`** as an encrypted **Secret** (not a plain frontend variable). Optionally add `GEMINI_MODEL` such as `gemini-2.5-flash` if available for your API account. Redeploy after changing secrets.
5. Open the deployed site and test Tasks, Habits, Goals, backup export/import, then send a message to Licia. If AI says it is not configured, check the secret name and redeploy.

## Android app / APK

This package includes the responsive installable web app and Cloudflare backend. It does **not** include a compiled APK: this environment has no Android SDK/Gradle toolchain, so I can't honestly claim an APK has been built. The easiest no-code Android install is to open the deployed site in Chrome on Android → browser menu → **Install app** or **Add to Home screen**; this creates an app-like PWA, not a standalone APK.

For a signed standalone APK, wrap the deployed site with Capacitor or use PWABuilder after deployment. Use the deployed HTTPS URL; never embed `GEMINI_API_KEY` in the Android app. The backend route `/api/licia` calls Gemini securely on Cloudflare.

## Current data and privacy behavior

- Tasks, habits, goals, and reflections are stored in browser `localStorage` on the current device. Export a JSON backup before clearing browser storage or switching devices.
- No account sync is included in this first reliable baseline. Do not assume data is backed up remotely.
- Licia receives the message and a compact snapshot of current tasks/habits/goals through the Cloudflare function. Do not submit passwords or sensitive data.
- The API key stays server-side in Cloudflare. Never paste it into `index.html`, `manifest.json`, or an APK.
- The function validates content type, bounds input/context size, returns generic upstream errors, and does not expose provider error payloads. Add Cloudflare rate limiting/Turnstile before public launch to reduce abuse and unexpected API usage.

## Files

- `public/index.html` — app UI and client-side features
- `public/manifest.json`, `public/sw.js`, `public/icon.svg` — PWA install/offline shell
- `functions/api/licia.js` — Cloudflare Pages Function / API proxy for Licia
- `wrangler.toml` — Pages project configuration

## Smoke test checklist

- Add, complete, delete a task
- Add/check/uncheck/delete a habit
- Create a goal and update progress
- Save a daily reflection, refresh, and verify it remains
- Export backup, reset data, import backup
- Send Licia a test prompt after adding the secret
- Test the site on Android Chrome and desktop
