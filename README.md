# Tab Out

**Keep tabs on your tabs.**

<p align="center">
  <a href="https://chromewebstore.google.com/detail/tab-out/imocfgofpgjhgklobbbpobhkbkjllegj">
    <img src="https://img.shields.io/chrome-web-store/v/imocfgofpgjhgklobbbpobhkbkjllegj?label=Install%20on%20Chrome%20Web%20Store&logo=googlechrome&logoColor=white&color=4285F4&style=for-the-badge" alt="Install on Chrome Web Store">
  </a>
</p>

Tab Out is a browser extension dashboard for your open tabs — grouped by domain, with homepages (Gmail, X, LinkedIn, etc.) pulled into their own card. Close tabs with a satisfying swoosh + confetti.

No server. No account. No third-party tracking. Just an extension.

This is a **fork of [zarazhangrui/tab-out](https://github.com/zarazhangrui/tab-out)** that adds Safari support and a batch of improvements. See [what's different in this fork](#whats-different-in-this-fork) below.

---

## Features

- **See all your tabs at a glance** on a clean grid, grouped by domain
- **Homepages group** pulls Gmail inbox, X home, YouTube, LinkedIn, GitHub homepages into one card
- **Close tabs with style** — swoosh sound + confetti burst
- **Duplicate detection** flags when you have the same page open twice, with one-click cleanup
- **Click any tab to jump to it** across windows, no new tab opened
- **Save for later** — bookmark tabs to a checklist before closing them
- **"Open all" / "Clear all"** bulk actions for your reading list and archive
- **Localhost grouping** shows port numbers next to each tab so you can tell your vibe-coding projects apart
- **Expandable groups** show the first 8 tabs with a clickable "+N more"
- **Dark mode** — auto-follows your system preference, with a sun-icon toggle for manual override
- **Keyboard accessible** — tab through the dashboard, Enter to focus, Space to trigger actions
- **100% local** — data never leaves your machine

---

## Install

### Safari

Safari Web Extensions can't be loaded unpacked — they have to live inside a native macOS app bundle. See [SAFARI.md](SAFARI.md) for the Xcode build instructions (takes ~5 minutes once Xcode is installed).

Short version:

```bash
git clone https://github.com/birkdev/tab-out.git
cd tab-out
xcrun safari-web-extension-converter ./extension \
  --project-location ./safari \
  --app-name "Tab Out" \
  --bundle-identifier com.yourname.tabout \
  --macos-only
```

Then in Xcode: pick a signing team, **⌘R**, close the container app, enable the extension in Safari's Extensions settings.

Bonus: since Safari doesn't let extensions override the new tab page, you can set your Safari homepage to the extension URL for a per-new-tab Tab Out experience. See SAFARI.md for details.

### Chrome

[**Install from the Chrome Web Store**](https://chromewebstore.google.com/detail/tab-out/imocfgofpgjhgklobbbpobhkbkjllegj) — one click, auto-updates.

Or load from source:

1. Clone the repo
2. Go to `chrome://extensions`
3. Enable **Developer mode**
4. Click **Load unpacked**, pick the `extension/` folder

Open a new tab — you'll see Tab Out.

---

## What's different in this fork

Compared to [upstream](https://github.com/zarazhangrui/tab-out):

**Safari port**
- Toolbar action (or **⌘⇧K**) opens/focuses the dashboard — Safari doesn't allow new-tab-page overrides
- Google Fonts bundled locally (offline, privacy, CSP-clean)
- DuckDuckGo favicon service (Safari doesn't expose `tab.favIconUrl`)

**Dark mode**
- Full palette via CSS `light-dark()`, follows system preference by default
- Sun-icon toggle in the header for manual override
- Preference persists across sessions

**Accessibility**
- ARIA labels, keyboard-accessible chips (Tab + Enter), visible focus rings
- Screen-reader-announced toasts, `<main>` landmark, `aria-hidden` on decorative SVGs
- Honors `prefers-reduced-motion`
- 44×44 touch targets on touch devices (via `@media (pointer: coarse)`)

**UX additions**
- "Open all" / "Clear all" bulk actions for Saved for later + Archive
- Per-card tab counts update live after closing/saving individual tabs
- Empty card auto-removes when its last chip is closed

**Code health**
- `app.js` trimmed ~27% by comment cleanup, shared helpers, and deduping
- `style.css` trimmed ~10% by removing dead rules
- Animations use exponential ease-out (no bounce/elastic)

---

## How it works

```
Click the toolbar icon (Safari) or open a new tab (Chrome)
  → Tab Out shows your open tabs grouped by domain
  → Homepages (Gmail, X, etc.) get their own card at the top
  → Click any tab title to jump to it
  → Close groups you're done with (swoosh + confetti)
  → Save tabs for later before closing them
```

Everything runs inside the extension. No external server, no API calls, no data sent anywhere. Saved tabs are stored in `chrome.storage.local` (Safari aliases this to `browser.storage.local`).

---

## Tech stack

| What | How |
|------|-----|
| Extension | WebExtensions / Manifest V3 (Chrome + Safari) |
| Storage | `chrome.storage.local` |
| Sound | Web Audio API (synthesized, no files) |
| Animations | CSS transitions + JS confetti particles |
| Fonts | Newsreader + DM Sans, bundled locally |
| Favicons | DuckDuckGo's public icon service |

---

## License

MIT. Original work © Zara Zhang, fork modifications © Birk Ihle.

See [LICENSE](LICENSE) — the MIT license applies to both the original and the fork.

---

Original by [Zara](https://x.com/zarazhangrui). Safari port + improvements by [Birk](https://github.com/birkdev).
