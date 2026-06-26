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
- 🎵 **Full player** — seekable waveform, chapter list, speed (0.75×–2×), skip ±30s, sleep timer, bookmarks, repeat, lock-screen controls
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

## Host on GitHub Pages

```bash
git init fana && cd fana
# copy index.html, favicon.ico, apple-touch-icon.png here
git add . && git commit -m "FANA audiobook app"
git remote add origin https://github.com/YOUR_USERNAME/fana.git
git push -u origin main
```

Then **Settings → Pages → Deploy from a branch → `main` / `/ (root)` → Save**.
Live at `https://YOUR_USERNAME.github.io/fana/` within a minute.

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
