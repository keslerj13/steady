# Steady — audiobook + steady background noise

An audiobook player that plays your MP3 chapters back-to-back **and** runs a quiet,
continuous background noise on the *same* play/pause button.

## Why it fixes the problem
- **No more breath glitches.** The background noise is genuinely continuous, so during
  the audiobook's tiny silences the speaker never hears true silence — it never has to
  re-acquire the Bluetooth connection, which is what causes the dropout/glitch.
- **No battery drain when paused.** Pause stops *everything* (it suspends the whole
  audio engine), so the headphones are allowed to go to sleep. No separate noise app
  left running in the background.

## Features
- **A library of books.** Add as many as you like — each is its own folder. Tap a
  book to open it; tap another to switch. Each book independently remembers the exact
  chapter and position you left off at, so you can read several at once.
- **Rename** a book with the ✎ button (the folder name is just the default title).
- **Reorder** the shelf by dragging the ⠿ handle on the left of each book.
- Add a book by picking its **folder** (including folders-of-folders / multi-disc) —
  every audio file inside is pulled out and ordered by its path automatically.
- Cover art: if the folder contains an image, it's shown in Now Playing, the library
  list, and the lock screen.
- Auto-advances chapter to chapter, with ⟲15 / 15⟳ buttons to jump back/forward 15
  seconds (also mapped to the lock-screen seek controls).
- Background noise: brown / pink / white, **plus your own noise files** (tap ＋ on the
  Type row to import rain, fan, ocean, etc.). Every option — built-in or custom — is
  loudness-normalized to the same scale, so the level slider behaves identically and a
  loud file won't blast. Custom noises are saved on-device and removable with their ✕.
- Book volume + playback speed (0.8×–2×).
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

## One thing to confirm on her actual phone
Android Chrome normally keeps audio playing with the screen off. Because this app mixes
the book and the noise through the Web Audio engine, do a real-world check: start
playback, lock the screen, put it in a pocket for a minute. If audio ever stops on lock
(some Android builds are stricter), tell me — there's a fallback design that plays the
noise as a looping audio track instead, which is even more bulletproof for background.
