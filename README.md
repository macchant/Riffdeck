<!--
  Drop this file in at: Riffdeck/README.md (replaces the existing one).
  Replace the SCREENSHOT placeholders with real assets in /docs/.
-->

<div align="center">

# 🎸 RiffDeck

### The stage-ready setlist for working musicians.

**Charts, chords, metronome, and real-time band sync — in one PWA that works offline at the gig.**

[![Live Demo](https://img.shields.io/badge/live%20demo-open-7c3aed?style=for-the-badge&logo=googlechrome&logoColor=white)](https://macchant.github.io/Riffdeck/)
[![License: MIT](https://img.shields.io/badge/license-MIT-22c55e?style=for-the-badge)](LICENSE)
[![PWA](https://img.shields.io/badge/PWA-installable-0ea5e9?style=for-the-badge&logo=pwa&logoColor=white)](https://macchant.github.io/Riffdeck/)
[![Version](https://img.shields.io/badge/version-1.7-1f2937?style=for-the-badge)](#changelog)

<img src="docs/hero.gif" alt="RiffDeck auto-scrolling a chord chart with the metronome running" width="800"/>

<sub><em>Replace <code>docs/hero.gif</code> with a real 5–10s GIF of the chart auto-scrolling.</em></sub>

</div>

---

## Why RiffDeck exists

I'm a working musician. Every existing setlist app either (a) costs money to do basic things, (b) needs a constant internet connection on stage, or (c) is built by people who've never had to flip a chord chart at 124 BPM with sweaty hands.

RiffDeck is the app I wished existed: **a fast, offline-first chord chart that auto-scrolls to the beat, transposes on the fly, and stays in sync across devices** for the rest of the band.

> _Built for musicians who play. Not for designers who don't._

---

## ✨ Features

### 🎤 On stage
- **Smart auto-scroll** — `requestAnimationFrame`-driven, **never drifts**. Set the song duration and the chart finishes _exactly_ when the song ends.
- **Web Audio metronome** — drift-free, accent on beat 1, restarts cleanly when BPM changes.
- **BPM nudge** — `−` / `+` buttons (hold to repeat) or `[` / `]` keys.
- **One-handed keyboard control** — full operation from a footswitch / Bluetooth pedal. Press `?` to see all shortcuts.
- **PWA** — install to home screen, works offline. App shell cached, songs from `localStorage` if Firebase is down.

### 🎼 Charting
- **ChordPro `[bracket]` parsing** — `[Am]Hello darkness [F]my old friend`.
- **Chromatic transpose** with slash-chord support — `[G/B]` → `[Ab/C]`.
- **Per-song memory** — transpose offset and scroll speed remember themselves per song, per device.
- **Print mode** — clean black-on-white serif chart, strips all UI chrome. Letter / A4 portrait optimized.
- **YouTube reference panel** — drop in a YT ID and the video sits next to the chart.

### 👥 Band sync
- **Real-time Firebase Realtime Database** — edit on one device, instantly visible on another.
- **Setlist share-links** — `?s=<setlistId>` opens the same set on bandmates' phones.
- **Drag-reorder** songs within a setlist. Multiple named setlists (Friday Gig, Acoustic, Rehearsal).

---

## 🚀 Try it now

1. **Open the demo** → [macchant.github.io/Riffdeck](https://macchant.github.io/Riffdeck/)
2. Click **Install** in your browser to add it as an app.
3. Press `?` to learn the keyboard shortcuts.
4. Read on if you want to run your own copy.

---

## 📸 Screenshots

<div align="center">

| Stage view (dark) | Mobile | Print mode |
|---|---|---|
| ![Stage view](docs/stage-dark.png) | ![Mobile](docs/mobile.png) | ![Print](docs/print.png) |

<sub>Drop real screenshots into <code>/docs/</code>. Three is plenty.</sub>

</div>

---

## 🧠 Engineering highlights

The interesting parts you can't see from the demo:

### Drift-free auto-scroll
The naive approach is `setInterval(scrollBy, 16)` which accumulates timing error and ends the song too early or too late. RiffDeck uses a single `requestAnimationFrame` loop and computes scroll position as a function of `(elapsed / songDurationMs) * totalScrollDistance`, so the chart hits the bottom exactly when the song ends — even if a tab is throttled and frames are skipped.

### Web Audio metronome that doesn't drift
JavaScript timers (`setInterval`) drift by ~10–50ms per minute on busy threads. RiffDeck schedules clicks via the **Web Audio API's sample-accurate clock** (`audioCtx.currentTime + offset`), looking ~100ms ahead and dropping `OscillatorNode`s onto a precise timeline. Result: a metronome that stays locked to the BPM for an entire 90-minute set.

### Transpose engine
Parses ChordPro tokens with a regex aware of slash chords (`G/B`), accidentals (`F#m7`, `Bb6`), and extensions (`Cmaj7add9`). Transposition operates on the **root + bass** independently using a 12-note chromatic table, then re-prints in the user's preferred accidental style.

### Optimistic offline-first
Songs are written to `localStorage` first, then fanned out to Firebase. On boot, the app reads from `localStorage` immediately (zero perceived latency), then reconciles with Firebase in the background. If Firebase is unreachable (a club basement with bad Wi-Fi), the app keeps working — and resyncs when connectivity returns.

### Print stylesheet as a "nuclear reset"
The print CSS strips every screen color, shadow, font, and chrome element to render a pure black-on-white serif chart. Browsers' default headers/footers are the only thing the user has to disable.

---

## 🛠️ Tech stack

| Layer | Choice | Why |
|---|---|---|
| **Build** | Zero build, single `index.html` | Fast iteration, easy GitHub Pages deploy, no `node_modules` to babysit. |
| **Realtime** | Firebase Realtime DB | Sub-100ms sync across devices, free tier covers most bands. |
| **Audio** | Web Audio API | Sample-accurate metronome scheduling. |
| **State** | `localStorage` + Firebase | Offline-first, resync on reconnect. |
| **Offline** | Service Worker + cache-first | App shell cached; works at the gig with no wifi. |
| **UI** | Vanilla JS + Tailwind (CDN) | Tiny bundle, no framework runtime. |
| **Fonts** | Inter (UI) + JetBrains Mono (chords) | Maximum chord-chart legibility. |

---

## 🏃 Run locally

### 1. Clone

```bash
git clone https://github.com/macchant/Riffdeck.git
cd Riffdeck
```

### 2. Serve over HTTP

The PWA / service worker won't register from `file://`, so use a local server:

```bash
# Python
python -m http.server 8080

# or Node
npx http-server -p 8080
```

Open `http://localhost:8080`.

### 3. (Optional) point at your own Firebase

The repo ships with a working Firebase config so the demo runs out of the box. To use your own database for private band data:

1. Create a project at [console.firebase.google.com](https://console.firebase.google.com).
2. Create a Realtime Database (test mode is fine while experimenting).
3. Replace the `firebaseConfig` block near the bottom of `index.html` with your keys.
4. Set security rules:

   ```json
   {
     "rules": {
       ".read": true,
       ".write": "auth != null"
     }
   }
   ```

### 4. Deploy

Push to GitHub. In **Settings → Pages**, set source to the `main` branch. Done.

---

## ⌨️ Keyboard shortcuts

| Key | Action | Key | Action |
|---|---|---|---|
| `Space` | Play / pause auto-scroll | `M` | Toggle metronome |
| `↑` / `↓` | Scroll | `[` / `]` | BPM −1 / +1 |
| `J` / `K` | Prev / next song | `−` / `=` | Transpose −1 / +1 |
| `V` | Toggle YouTube panel | `0` | Reset transpose |
| `S` | Open setlist | `N` | New song |
| `E` | Edit current song | `F` | Toggle fullscreen |
| `/` | Focus search | `Esc` | Close panel |
| `?` | Show this list | | |

---

## 🗄️ Data model

### Song — `setlist/{songId}`
```json
{
  "id": "s_a1b2c3d4",
  "title": "Starlight",
  "artist": "Muse",
  "key": "B",
  "bpm": 122,
  "duration": "3:42",
  "youtube_id": "Pgum6OT_VH8",
  "content": "[Verse 1]\n[B]Far away...",
  "updatedAt": 1714291200000
}
```

### Setlist — `setlists/{listId}`
```json
{
  "id": "sl_friday1",
  "name": "Friday Night Acoustic",
  "songIds": ["s_a1b2", "s_c3d4", "s_e5f6"],
  "updatedAt": 1714291200000
}
```

The virtual `"all"` setlist is computed client-side and contains every song in the library, sorted alphabetically.

---

## 🗺️ Roadmap

- [ ] **v1.8** — backing-track audio (per-song MP3 + lockscreen / Media Session controls)
- [ ] **v1.9** — section quick-jump pills above the chart
- [ ] **v2.0** — band sync ("Stage Sync") — multi-device scroll position via Firebase
- [ ] **v2.1** — chord diagrams on tap, role views (singer / guitarist / drummer)
- [ ] **Future** — AI chord-from-YouTube extraction, count-in metronome, MIDI pedalboard, native iOS / Android wrapper

---

## 📜 Changelog

### v1.7 — BPM nudge + mobile coach mark
- Removed TAP tempo (confused first-time users).
- Added BPM nudge buttons (`−` / `+`, hold to auto-repeat, range 20–300).
- Added `[` / `]` shortcuts.
- First-time mobile coach mark points new users at the hamburger menu.

### v1.6 — Print feature
- New print button → clean black-on-white chord chart for paper or PDF.
- Print-only header (title, artist, key, BPM, duration, capo / transpose).
- Nuclear-reset stylesheet strips every screen color and UI chrome.

### v1.0 — Premium SaaS rebuild
- From-scratch redesign with the core feature set every gigging musician actually needs.
- Slate + electric violet palette, Inter + JetBrains Mono, full dark mode (WCAG AA).

---

## 🤝 Contributing

PRs welcome. Please:
1. Open an issue first if it's a bigger feature.
2. Match the existing vanilla-JS style (no build step).
3. Test on a phone before submitting — half the users are on stage with one.

---

## 📄 License

MIT. Use it, fork it, gig with it. See [LICENSE](LICENSE).

---

<sub>Built by [@macchant](https://github.com/macchant) — because I needed it for my next gig.</sub>
