# RiffDeck — The stage-ready setlist for working musicians

![License: MIT](https://img.shields.io/badge/License-MIT-f97316.svg)
![Build: Single File](https://img.shields.io/badge/Build-Single_HTML_File-27272a.svg)
![Database: Firebase](https://img.shields.io/badge/Sync-Firebase_Realtime-FFCA28.svg)
![PWA: Installable](https://img.shields.io/badge/PWA-Installable-10b981.svg)

> Charts, chords, metronome, and band sync in one place. Built for musicians who play, not for designers who don't.

---

## What's new in v1.0 — Premium edition

A from-scratch SaaS-style redesign of the original RiffDeck, with the core feature set every gigging musician actually needs.

### Visual

- Cleaner SaaS aesthetic — zinc grayscale + signature **orange** accent
- Inter (UI) + JetBrains Mono (chords/code) — no decorative fonts
- Fresh logo: pick + strings mark, recognizable at favicon size
- Subtle dotted grid background, no gold gradients, more whitespace
- Full dark mode with WCAG-AA contrast

### Features

- **Setlists** — name and order multiple sets (Friday Gig, Acoustic, Rehearsal). Drag-reorder songs within a setlist. Share by link.
- **Smart auto-scroll** — `requestAnimationFrame`-driven, never drifts. If you set the song duration, the chart scrolls to finish exactly when the song ends. Otherwise falls back to per-song speed memory (1–10).
- **Tap tempo** — tap the button (or press `T`) 4× to set BPM. Auto-saves to the song.
- **Per-song memory** — transpose offset and scroll speed remember themselves per song, per device.
- **Keyboard shortcuts** — full one-hand operation. Press `?` to see them all.
- **PWA** — install to home screen / desktop, works offline at gigs (app shell cached, songs from localStorage if Firebase down).
- **Setlist share-links** — `?s=<setlistId>` URLs let bandmates open the same set.
- **Pause-on-touch** — auto-scroll pauses when you scroll/tap, so you don't fight it.

### Carried over from the original

- ChordPro `[bracket]` parsing — `[Am]Hello darkness [F]my old friend`
- Chromatic transpose with slash-chord support — `[G/B]` becomes `[Ab/C]` etc.
- Web Audio metronome (no drift, accent on beat 1)
- YouTube reference video panel
- Real-time Firebase sync — edit on one device, instantly visible on another

---

## Keyboard shortcuts

| Action                 | Key       |
| ---------------------- | --------- |
| Auto-scroll play/pause | `Space`   |
| Scroll faster / slower | `↑` / `↓` |
| Previous / next song   | `J` / `K` |
| Transpose down / up    | `−` / `=` |
| Reset transpose        | `0`       |
| Toggle metronome       | `M`       |
| Tap tempo              | `T`       |
| Toggle video           | `V`       |
| Toggle sidebar         | `S`       |
| New song               | `N`       |
| Edit current song      | `E`       |
| Focus search           | `/`       |
| Fullscreen             | `F`       |
| Show shortcuts         | `?`       |
| Close any overlay      | `Esc`     |

---

## Tech stack

| Layer    | Choice                                              |
| -------- | --------------------------------------------------- |
| Frontend | Single HTML file (HTML5 + CSS3 + vanilla ES6)       |
| Database | Firebase Realtime Database (compat SDK 10.7.1)      |
| Storage  | localStorage (prefs + offline fallback)             |
| Audio    | Web Audio API (synthesized clicks, no audio file)   |
| PWA      | Manifest + service worker (network-first app shell) |
| Hosting  | GitHub Pages                                        |

Zero build step. No npm. No Webpack. Just open `index.html`.

---

## Setup

### 1. Clone

```bash
git clone https://github.com/macchant/Riffdeck.git
cd Riffdeck
```

### 2. (Optional) point at your own Firebase

The repo ships with a working Firebase config so the demo works out of the box. To use your own database for private band data:

1. Create a project at [console.firebase.google.com](https://console.firebase.google.com)
2. Create a Realtime Database (test mode is fine while you experiment)
3. Replace the `firebaseConfig` block near the bottom of `index.html` with your keys
4. Set your security rules — at minimum, restrict writes:

   ```json
   {
     "rules": {
       ".read": true,
       ".write": "auth != null"
     }
   }
   ```

### 3. Run

For PWA / service worker to register, you must serve over HTTP (not `file://`):

```bash
# Option A: Python
python -m http.server 8080

# Option B: Node
npx http-server -p 8080
```

Then open `http://localhost:8080`.

### 4. Deploy

Push to GitHub. In your repo settings, enable **GitHub Pages** from the `main` branch. Done.

---

## Data model

### Song (`setlist/{songId}`)

```js
{
  id:         "s_a1b2c3d4",
  title:      "Starlight",
  artist:     "Muse",
  key:        "B",
  bpm:        122,
  duration:   "3:42",       // mm:ss — used by smart auto-scroll
  youtube_id: "Pgum6OT_VH8",
  content:    "[Verse 1]\n[B]Far away...",
  updatedAt:  1714291200000
}
```

### Setlist (`setlists/{listId}`)

```js
{
  id:        "sl_friday1",
  name:      "Friday Night Acoustic",
  songIds:   ["s_a1b2", "s_c3d4", "s_e5f6"],   // ordered
  updatedAt: 1714291200000
}
```

The virtual `"all"` setlist is computed client-side and represents every song in the library, sorted alphabetically.

---

## Roadmap

- **v1.1** — chord diagrams on tap, role views (singer/guitarist/drummer), section minimap
- **v1.2** — Svelte rebuild, Supabase backend option, real-time band cursor
- **v1.3** — print/PDF export, Spotify/Apple Music refs, count-in before auto-scroll

---

## License

MIT. Use it, fork it, gig with it.
