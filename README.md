# 📚 FANA — Free Audiobooks

A fast, private audiobook player. Stream thousands of free public-domain books from
[LibriVox](https://librivox.org), **or** add audio files straight from your own device and
keep them in a local library. No account, no ads, no tracking.

Ships as a **single `index.html`** — no build step, no server required.

---

## Features

- ✨ **Splash + welcome** — a brief animated splash, then a first-run landing page (shown once)
- 🎧 **Discover** — browse and search LibriVox (public-domain, volunteer-read)
- 🔍 **Live search** — results drop down as you type
- 📥 **Add from device** — pick local audio files; they're stored in your browser and play offline forever
- ⬇️ **Download for offline** — save any LibriVox book to your device from the book's detail screen and play it with no connection; a badge marks downloaded books in your library
- 📚 **Personal library** — save books, resume exactly where you left off
- 🎵 **Full player** — drag-to-seek waveform, chapter list, speed (0.75×–2×), skip ±30s, bookmarks, repeat, lock-screen controls
- 📦 **Gap-free playback** — each chapter downloads in full before it plays (with a progress bar and a "start playing now" option), and the next chapter is fetched ahead so the book carries on seamlessly — even with the screen locked
- 💾 **Download for offline** — a book starts downloading to your device the moment you **save** it or **play** it, and is kept there so it plays instantly (and offline) next time. The book you're playing goes first; saved books follow in the order you saved them. Downloads carry on after you close and reopen the app, yield to whatever you're hearing so they never cause stutter, and respect Data Saver. Library cards show progress (⬇ 3/12) and an **Offline** badge when done; pause or resume from the player or the book's details
- 🔗 **Share** — share a book, or a bookmarked moment, through your device's share menu (or FANA's own share sheet where the browser has none). The link opens FANA at that exact chapter and time
- 🌙 **Sleep timer** — quick presets, a custom hours/minutes scroll wheel, or end of chapter
- 🌓 **Three themes** — Light, Dark, and Sepia, all clean and easy to read; the top-bar toggle shows a moon on light themes and a sun on Dark, and cycles through them
- 📱 **Responsive** — mobile-first; works on phone, tablet, desktop
- ⚡ **Instant load** — one HTML file, everything inline

---

## Run it locally

You *can* double-click `index.html` to try it, but opening a page as `file://` makes some
browsers **block IndexedDB and localStorage** — so your library, uploads, and progress won't
be saved between sessions (the app detects this and keeps working in memory for the session,
with a heads-up toast). For full persistence, serve it over `http://` with one command:

```bash
python3 -m http.server 8080   # then open http://localhost:8080
# or:  npx serve .
```

Hosting on GitHub Pages (below) also gives you full persistence automatically.

## Sharing links

Shared links look like `https://abbel-dawit.github.io/FANA/?book=<id>&ch=3&t=754` and open
FANA at that book, chapter and second. The address is set near the top of the Sharing
section in `index.html`:

```js
const FANA_PUBLIC_URL='https://abbel-dawit.github.io/FANA/';
```

Because it's set, links always point at the live site — even if you share from a local
copy. If the site ever moves (a new repo name or a custom domain), update this line.
GitHub Pages paths are case-sensitive, so the repository must be named exactly `FANA`.

The system share menu needs HTTPS, which GitHub Pages provides. Browsers without one —
most desktop browsers — get FANA's own share sheet instead.

## Host on GitHub Pages

```bash
git init FANA && cd FANA
# copy index.html, favicon.ico, apple-touch-icon.png here
git add . && git commit -m "FANA audiobook app"
git remote add origin https://github.com/abbel-dawit/FANA.git
git push -u origin main
```

Then **Settings → Pages → Deploy from a branch → `main` / `/ (root)` → Save**.
Live at `https://abbel-dawit.github.io/FANA/` within a minute.

---

## Files

```
fana/
├── index.html            — the entire app (HTML + CSS + JS inline)
├── favicon.ico           — book icon (multi-size .ico)
├── apple-touch-icon.png  — home-screen icon
└── README.md
```

---

## How it works

- **Storage** — your library, listening progress, bookmarks, and any uploaded audio files
  are saved in the browser's **IndexedDB**. Nothing leaves your device.
- **LibriVox + CORS** — LibriVox's API sends no CORS headers, so metadata requests are
  **raced across several public CORS proxies**; the first to respond wins. Audio tracks
  stream directly (media playback isn't subject to CORS).
- **Local files** — selected audio files are stored as blobs in IndexedDB and played via
  object URLs, so they keep working with no network at all.

> **Note on online search:** public CORS proxies are free and occasionally rate-limited or
> down. If Discover can't reach LibriVox, the **Add from device** path always works offline.
> To swap proxies, edit the `PROXIES` array near the top of the script in `index.html`.

---

## Sources & license

Streamed content comes from [LibriVox](https://librivox.org) — free public-domain
audiobooks read by volunteers, explicitly placed in the public domain.

App code: **MIT** — free to use, modify, and distribute.
