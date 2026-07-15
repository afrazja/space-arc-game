# Space Arc

A 7-level browser space shooter — pilot progression (levels 1–20), five
weapons across three loadout slots, lives, and a unique boss guarding every
sector. One self-contained HTML file; no build step, no dependencies.

Play it, and it installs as an app on Android/desktop (Chrome/Edge) via the
in-game **INSTALL APP** button or the browser's "Add to Home screen".

## Download the Android app (APK)

Grab **space-arc.apk** from the [latest release](../../releases/latest) and
open it on your phone (approve "install unknown apps" the first time). A
GitHub Action rebuilds it automatically on every push — see
`.github/workflows/build-apk.yml`. Note: the APK is a snapshot, so updating it
means downloading the newer one; the web/installed-PWA version updates itself.

## Hosting

It's a static site — `index.html` at the root plus the icons, manifest, and
service worker. Two zero-config options:

- **GitHub Pages:** repo Settings → Pages → Source: *Deploy from a branch* →
  `main` / `/ (root)`. Live at `https://afrazja.github.io/space-arc-game/`.
- **Vercel / Netlify:** import the repo; framework preset "Other". No settings.

## Shipping an update (e.g. adding a level)

Installed players update themselves — no re-download:

1. Edit `index.html` (e.g. raise the level count, add the boss) and bump
   `APP_VERSION` near the bottom of its script.
2. Bump the `CACHE` constant in `sw.js` (`space-arc-v1` → `space-arc-v2` → …).
   **This is the trigger** — the changed worker is how the app notices an update.
3. Commit and push. The host redeploys.

Next time a player opens the app, it detects the new worker and shows an
**UPDATE** banner; one tap reloads them into the new version. Offline the whole
time, thanks to the cached copy.

## Files

| File | Purpose |
|------|---------|
| `index.html` | the entire game (canvas, logic, art, sound) |
| `sw.js` | service worker — offline cache + update delivery |
| `manifest.webmanifest` | PWA metadata (name, icons, fullscreen/portrait) |
| `icon-192.png`, `icon-512.png` | app icons |
