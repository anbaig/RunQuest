---
name: verify
description: Run and drive the Run Quest static PWA to check a change actually works.
---

Run Quest is a single dependency-free static site in `site/` (`index.html`, `manifest.json`, `sw.js`, `icons/`). No build step.

## Serve it

```bash
cd site && python3 -m http.server 8791
```

Then open `http://127.0.0.1:8791/index.html` (viewport ~440x900 to match the mobile design).

## Drive it (Playwright)

Chromium is preinstalled at `/opt/pw-browsers/chromium-1194/chrome-linux/chrome`; Playwright itself is only installed globally, so `require()` it from `/opt/node22/lib/node_modules/playwright` (no local `npm install` needed). Launch with `args: ['--no-sandbox']`.

Every interactive element carries its handler in `onclick="App.xxx(...)"` / `oninput="App.xxx(this)"` as a literal DOM attribute — select elements with `button[onclick^="App.methodName"]` rather than by text, since button text/labels change with state (e.g. BUY vs "NEED N MORE").

Key flows to exercise after any change:
- Dashboard → LOG A RUN → fill `#milesInput` → submit → balance/streak/log update.
- Dashboard → OPEN SHOP → buy an affordable item → confirm modal → toast → GM Requests tab shows it pending.
- Gear icon → PIN gate (default PIN `1234`, set in `CONFIG.gmPin` in `index.html`) → wrong PIN shows "WRONG PIN" and clears → correct PIN unlocks GM tabs (Log/Shop/Requests/Sync).
- GM Sync tab: `#syncCode` textarea always holds the current base64 state; paste it into `#syncInput` and hit LOAD UPDATE → "Synced!"; paste garbage → "Invalid code — check and try again".
- Reload the page (hard reload) → balance/streak/log/shop/redemptions persist via `localStorage['runquest-state-v1']`; `screen`/`gmUnlocked` intentionally do NOT persist (always lands back on dashboard, PIN required again).

## PWA assets

Verify these resolve (200) alongside `index.html`: `manifest.json`, `sw.js`, `icons/icon-192.png`, `icons/icon-512.png`, `icons/icon-maskable-512.png`, `icons/apple-touch-icon.png`, `icons/favicon-32.png`. Service worker should reach `active` state shortly after load (`navigator.serviceWorker.getRegistration()`).

## Gotcha

Fonts (Press Start 2P, Silkscreen) are inlined as base64 `@font-face` data URIs directly in `index.html` — there's no network font fetch to break, but it does make the file large (~78KB) and any font change means re-downloading + re-base64ing, not editing the CSS by hand.
