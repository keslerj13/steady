# Steady — audiobook + steady background noise

An audiobook player that plays your MP3 chapters back-to-back **and** runs a quiet,
continuous background noise on the *same* play/pause button.

## Why it fixes the problem
- **No more breath glitches.** The background noise is genuinely continuous, so during
  the audiobook's tiny silences the speaker never hears true silence — it never has to
  re-acquire the Bluetooth connection, which is what causes the dropout/glitch.
- **No battery drain when paused.** Pause stops *everything* (both the book and the
  noise), so the headphones are allowed to go to sleep. No separate noise app left
  running in the background.

## Features
- **A library of books.** Add as many as you like — each is its own folder. Tap a
  book to open it; tap another to switch. Each book independently remembers the exact
  chapter and position you left off at, so you can read several at once.
- **Rename** a book with the ✎ button (the folder name is just the default title).
- **Reorder** the shelf by dragging the ⠿ handle on the left of each book.
- Add a book by picking its **folder** (including folders-of-folders / multi-disc) —
  every audio file inside is pulled out and ordered by its path automatically.
- **Cover art**, shown as a large banner in Now Playing (and in the library list +
  lock screen). The default comes from an image in the folder, but tap **Change cover**
  to pick a different one from the folder's images or upload your own.
- **Whole-book progress bar** above the chapter scrub — a visual-only bar showing how
  many minutes into the entire book you are, out of the total.
- Auto-advances chapter to chapter, with **« 60 / « 15** and **15 » / 60 »** buttons to
  jump back/forward 15 or 60 seconds (15s is also mapped to the lock-screen seek).
- **Smart resume:** if it's been more than 5 seconds since you last listened, pressing
  play rewinds 15 seconds so you catch the lead-up to where you stopped.
- **Auto-rewind on device switch:** connecting or disconnecting a Bluetooth
  speaker/headphones automatically rewinds 15 seconds, so nothing is missed while
  the audio hands off.
- Background noise: brown / pink / white, **plus your own noise files** (tap ＋ on the
  Type row to import rain, fan, ocean, etc.). Every option — built-in or custom — is
  loudness-normalized to the same scale, so the level slider behaves identically and a
  loud file won't blast. Custom noises are saved on-device and removable with their ✕.
- Book volume + a **0.5×–3× speed slider** that snaps to set steps.
- Lock-screen / notification controls (play, pause, next, previous), so it keeps
  playing with the screen off.
- Everything (books, covers, your place in each) is stored on-device in the browser.
- Each section has an **(i)** button in its top-right corner that reveals short
  instructions for that section.
- **Installable PWA + offline:** add it to the home screen and it launches fullscreen
  like a real app, with its own icon, and works with no connection.

## Files to host
Upload these to the host (the whole set):
- `index.html` — the app
- `manifest.webmanifest` — makes it installable
- `sw.js` — service worker for offline support
- `icon-192.png`, `icon-512.png`, `icon-maskable-512.png` — app icons

Not needed for hosting: `steady_logo.png` (the source artwork the icons are made from)
and `.claude/launch.json` (local dev-server config).

## How to run it (Android, recommended)
1. Put the files above on any static host (e.g. GitHub Pages — see below).
2. On her phone, open the URL in **Chrome**.
3. Chrome menu → **Install app** / **Add to Home screen**. It opens fullscreen like an app.
4. Tap **Add a book (folder)**, pick a book's folder, and hit play. Repeat for each
   book — they all live in the library and keep their own place.

## Hosting on GitHub Pages
1. Create a public repo (e.g. `steady`) at github.com/new.
2. **Add file → Upload files** and drag in all the files listed above; commit.
3. **Settings → Pages → Source: Deploy from a branch → `main` / `/ (root)`**, Save.
4. After ~1–2 min the site is live at `https://YOUR-USERNAME.github.io/steady/`.

The app icons are derived from `steady_logo.png` (resized with macOS `sips`). To change
the icon, replace that file and regenerate `icon-192.png`, `icon-512.png`, and
`icon-maskable-512.png`.

## Local test
From this folder: `python3 -m http.server 8123` then open `http://localhost:8123`.

## Background playback (screen off / locked)
Both the book **and** the noise play as plain looping `<audio>` media elements — no Web
Audio. That's deliberate: a media element keeps playing when the screen locks, and there's
no AudioContext to get "interrupted" on lock (which on iOS was silently muting the book
after a lock-screen pause/play). Generated brown/pink/white noise is rendered to a seamless
20-second looping WAV (the loop point is crossfaded so there's no click and no gap of
silence — silence is what would let a Bluetooth speaker nod off). Do a real-world check on
her phone: start playback, lock the screen, pocket it for a minute, then pause/play from the
lock screen and confirm sound resumes.
