# Pexesooo

A self-contained, browser-based **memory matching game** (known as *pexeso* in Czech and Slovak, *Concentration* / *Pairs* in English) plus a companion **set generator**. Build your own card sets from two folders of images, get a single portable JSON file, and play — no server, no build step, no install.

> **Status:** v1.4.0 · two static HTML files · runs offline from `file://`

---

## What's in here

| File | What it is |
|---|---|
| `Pexesooo.html` | The game. Loads a set file and plays it for 1–6 players. |
| `PexesoooGenerator.html` | The set builder. Turns two folders of images into one `.json` set. |
| `DESIGN.md` | Shared visual language (design tokens, typography, components). |
| `README.md` | This file. |

Both apps are single, dependency-free HTML files. Just open them in a browser.

---

## Features

- **Make your own sets** from your own images — flags, animals, vocabulary, family photos, anything.
- **Image-only card pairs:** PNG, JPG, and JPEG files are supported.
- **Self-contained sets:** images are embedded in the JSON, so a set is one file you can share, e-mail, or commit to a repo.
- **1–6 players** with custom names and a clear turn indicator.
- **Fair starting order:** each *Play again* rotates which player starts; *Start game* resets the rotation.
- **Optional turn timer** with a countdown bar; on timeout the turn passes automatically.
- **Live or hidden scoring** — show scores during play, or (the default) reveal them only in the results.
- **Completion time** and **per-player turn counts** are shown in the results, for both solo and multiplayer games.
- **Custom card back** (optional) — or the set name is used as the back by default.
- **Czech / English** UI (Czech by default), switchable at any time.
- **Private by design:** everything runs locally in your browser. No uploads, no tracking, no network calls (other than loading web fonts).

---

## Quick start

### 1. Build a set (`PexesoooGenerator.html`)

1. Prepare **two folders** of image files. A pair is formed when a file in folder A and a file in folder B share the **same name** (case-insensitive, extension ignored). For example `france.png` in A pairs with `france.jpg` in B.
2. Open `PexesoooGenerator.html`.
3. Enter a **set name** and **description** (up to 256 characters).
4. Choose **Folder A** and **Folder B** (a folder picker dialog opens — you select the folder, not individual files).
5. *(Optional)* choose a **card back** image.
6. Click **Submit**. Review the detected pairs — incomplete or unreadable pairs are flagged and excluded; untick any you don't want.
7. Click **Generate JSON**. You get a file named `Pexesooo__<Set_Name>.json`.

**Accepted files:** `PNG`, `JPG`, `JPEG`.
**Processing:** card images are center-cropped to 200×200; the optional card back to 400×400.

### 2. Play (`Pexesooo.html`)

1. Open `Pexesooo.html`.
2. Under **Settings**, load a set using one of two methods:
   - **File** — choose a single `.json` set file directly. A preview shows the name, description, pair count, creation date, sample pairs, and card back.
   - **Folder** — choose a folder; the game scans it (top level only, no recursion) for valid `.json` set files and lists all it finds. Click a row to select a set.
3. Pick the number of players, enter names, and set the timer / scoring options.
4. Click **Start game**.

**Turn rules**

- Flip up to two cards.
- **Match** → you keep the pair and **go again**.
- **No match** → the cards stay face-up so everyone can see them; **tap your own name** to end your turn (the cards flip back and play passes on). If the timer is on and runs out, the turn ends automatically.
- Won cards leave a **fixed empty gap** — positions never shuffle mid-game, so memory still matters.
- The game ends when the board is cleared. The **Results** section shows each player's score, their turn count, the pairs they won, and the total time.

In the results you can choose **Play again** (immediately restarts with the same settings, rotating the starting player) or **Different game** (returns to settings).

---

## Set file format

A set is a single JSON file. Images are stored inline as base64 data URIs, so the file is fully portable.

```json
{
  "_source": "https://github.com/KarlM0/Pexesooo/",
  "format": "pexesooo",
  "version": 1,
  "generator": "PexesoooGenerator/1.4.0",
  "createdAt": "2026-06-06T12:00:00Z",
  "name": "World Flags",
  "description": "Match each flag to its country name",
  "thumb": { "size": 200, "fit": "cover", "type": "image/jpeg" },
  "pairCount": 24,
  "back": { "kind": "image", "src": "data:image/jpeg;base64,…" },
  "pairs": [
    {
      "id": "france",
      "a": { "kind": "image", "src": "data:image/jpeg;base64,…" },
      "b": { "kind": "image", "src": "data:image/jpeg;base64,…" }
    }
  ]
}
```

Notes:

- Each pair produces **two cards**; a match is two cards with the same pair `id`.
- Both card sides are `{ "kind": "image", "src": "data:image/…" }`.
- `back` is **optional**. If absent (or unreadable), the game renders the **set name** as the card back.
- `_source` identifies the project URL and is informational only; the game ignores it.
- `format` must be `"pexesooo"`. Sets produced by earlier versions of the generator (which used `"pexeso"`) are not compatible.

---

## Design

Both apps follow a shared dark-theme design language defined in `DESIGN.md` (CSS custom properties, three typefaces — Fraunces / Manrope / JetBrains Mono — and a small component library). A few elements are deliberate, documented extensions of that system:

- **Card flip** — a restrained 0.3s eased `rotateY`, a stated exception to the base "single entrance animation" motion rule.
- **File / folder picker pair, set list, player turn-strip, repo link, and the loading indicator** — derived from the base tokens where the component library has no existing pattern.

These are flagged in the source as candidates for inclusion in `DESIGN.md`.

---

## Browser support & limitations

- Tested as standards-based HTML/CSS/JS; works in current Chromium, Firefox, and Safari.
- The folder selection in both apps uses the `webkitdirectory` file input; support is broad but the exact behavior of the folder dialog varies by browser/OS.
- **HEIC** images (common on iPhones) are **not** supported by browsers — convert to JPG/PNG first.
- Image orientation: the generator applies EXIF orientation when re-encoding, but verify with a rotated phone photo on your browser.
- **No persistence** — there's no save/resume; settings and progress live only in the current tab.
- **Offline:** the apps work from `file://`; without internet the web fonts fall back to system fonts (layout and behavior are unaffected).
- The folder picker in the game scans only the **top level** of the chosen folder; JSON files in subfolders are ignored.

---

## Privacy

Everything happens in your browser. Your images never leave your device; the apps make no API calls and store nothing remotely.

---

## Versioning

- App version is shown in each app's header (`Pexesooo v1.4.0`, `PexesoooGenerator v1.4.0`).
- The set file records the generator version in its `generator` field and the schema version in `version`.

---
