# RiffDeck Light Theme

A print-optimized, light-themed version of [RiffDeck](https://github.com/macchant/Riffdeck) — the stage-ready setlist app for working musicians.

## What's Different

| Feature | Dark Theme | Light Theme |
|---------|------------|-------------|
| Background | Deep slate (#0a0a12) | Clean white (#fff) |
| Chords | Gold with glow | Bold black (UG style) |
| Section headers | Purple pill badge | Underline style |
| Best for | Stage use (dark rooms) | Printing, daylight, reading |

## Files

- `index-light.html` — Main app with light theme
- `manifest-light.webmanifest` — PWA manifest
- `sw.js` — Service worker for offline support
- `icons/` — PWA icons

## Usage

1. Serve the folder with any static server
2. Open `index-light.html` in browser
3. Print (Ctrl+P) for Ultimate Guitar-style chord sheets

## Print Tips

- Uncheck "Headers and footers" in print dialog for cleaner output
- The print stylesheet shows only the chord chart (no UI elements)
- Song title, artist, key, BPM, and duration appear at the top

## Credits

Original: [macchant/Riffdeck](https://github.com/macchant/Riffdeck)  
License: MIT
