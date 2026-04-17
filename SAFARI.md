# Tab Out — Safari build

Safari can't load unpacked extensions the way Chrome can — every Safari extension has to live inside a native macOS (or iOS) app bundle. Apple ships a converter that wraps an existing Web Extension folder into an Xcode project for you.

## What changes in Safari

Safari Web Extensions don't support `chrome_url_overrides.newtab`, so Tab Out can't take over the new tab page. Instead, **click the toolbar button** (or press **⌘⇧Y**) to open the dashboard. If a Tab Out tab is already open, it's focused; otherwise a fresh tab opens.

Everything else (domain grouping, save-for-later, badge counter, confetti) works the same.

## Prerequisites

- macOS with full **Xcode** installed (Command Line Tools alone aren't enough — `xcrun safari-web-extension-converter` ships with Xcode). Install from the App Store or [developer.apple.com/xcode](https://developer.apple.com/xcode/).
- An Apple ID signed into Xcode (Xcode → Settings → Accounts). A free Personal Team is fine for local use.

## Build

From the repo root:

```bash
xcrun safari-web-extension-converter ./extension \
  --project-location ./safari \
  --app-name "Tab Out" \
  --bundle-identifier com.example.tabout \
  --no-open
```

Change the bundle identifier to something you own if you plan to distribute (`com.yourname.tabout`). The `./safari` directory is gitignored by default — regenerate it any time the extension changes.

Then:

```bash
open ./safari/"Tab Out"/"Tab Out".xcodeproj
```

In Xcode: pick a signing team in the target settings (click the project → *Signing & Capabilities* → *Team*), then press **⌘R** to build and run. A tiny container app launches — you can close it. The extension is now registered with Safari.

## Enable the extension in Safari

1. Safari → **Settings → Advanced** → tick **"Show features for web developers"**.
2. Safari → **Develop** menu → **Allow Unsigned Extensions** (required every Safari launch when running an unsigned dev build).
3. Safari → **Settings → Extensions** → tick **Tab Out**.
4. The Tab Out icon appears in the toolbar. Click it, or press **⌘⇧Y**.

## Updating

After editing files in `extension/`, rebuild the container app (**⌘R** in Xcode). Safari picks up the changes automatically — no need to remove/re-add the extension.

## Known differences vs the Chrome build

- No new-tab-page override (Safari limitation — use the toolbar button).
- Extension URL scheme is `safari-web-extension://<UUID>/` instead of `chrome-extension://<ID>/`. The code uses `chrome.runtime.getURL()` so it works in both.
- Favicons are fetched from Google's public favicon service (same as Chrome). Safari's CSP allows this via `img-src https:`.
