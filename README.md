# 🎸 RiffDeck | Real-Time Setlist & Tab Vault

![License: MIT](https://img.shields.io/badge/License-MIT-d4af37.svg)
![Build: Single File](https://img.shields.io/badge/Build-Single_File-8c3b0c.svg)
![Database: Firebase](https://img.shields.io/badge/Database-Firebase_Realtime-FFCA28.svg)

**RiffDeck** is a zero-build, single-file web application designed for musicians and bands to manage, sync, and practice their setlists in real-time. 

Wrapped in a custom "Les Paul Darkburst" aesthetic, RiffDeck acts as a centralized vault for guitar tabs, lyrics, YouTube reference tracks, and practice tools. By utilizing Firebase's Realtime Database via a CDN, any track added, edited, or transposed by one band member instantly updates on the screens of everyone else.

Perfect for locking down heavy setlists (from X-Japan to Aimer) in a short timeframe before hitting the studio.

## ✨ Core Features

* **🔥 Real-Time Band Sync:** Powered by Firebase. Add or edit a track, and your bandmates see the updates instantly without refreshing.
* **🎛️ Intelligent Chord Transposer:** Shift the key of any song up or down with a click. The app mathematically calculates and updates all inline chords and slash chords (e.g., changing `[Am]` to `[Bm]`) and updates the master Key display.
* **⏱️ Visual Metronome:** A built-in, pulsing visual beat indicator mapped to the specific BPM of the selected track.
* **📜 Hands-Free Auto-Scroll:** A customizable scrolling engine allows you to keep your hands on the fretboard while the tab sheet slowly advances.
* **📺 Theater Mode:** Toggle the embedded YouTube reference track on or off to expand your tab sheet to full width.
* **📝 In-App Editor:** Add new songs, artist details, keys, BPMs, and YouTube IDs directly through the UI. Chords placed in brackets `[like this]` are automatically parsed, highlighted, and made ready for transposition.

## 🛠️ Tech Stack

* **Frontend:** HTML5, CSS3, Vanilla JavaScript (ES6+)
* **Backend/Database:** Firebase Realtime Database (Modular V9 SDK via CDN)
* **Hosting:** GitHub Pages (Recommended)
* **Architecture:** Zero-dependency Single Page Application (SPA)

## 🚀 Setup & Installation

Because RiffDeck is a single-file application, you do not need Node.js, Webpack, or any local servers to run it. However, to enable the real-time syncing feature, you must connect it to a free Firebase project.

### Step 1: Create a Firebase Project
1. Go to the [Firebase Console](https://console.firebase.google.com/).
2. Click **Create a Project** (Name it "RiffDeck" or your band's name).
3. Disable Google Analytics (optional) and create the project.
4. Click the **Web icon (`</>`)** to register a web app.
5. Copy the `firebaseConfig` object provided by Firebase.

### Step 2: Configure the App
1. Clone or download this repository.
2. Open `riffdeck.html` (or `index.html`) in a text editor.
3. Scroll to the bottom of the file to the `<script type="module">` section.
4. Locate the `firebaseConfig` object and replace it with the API keys you copied in Step 1.

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abcde12345"
};