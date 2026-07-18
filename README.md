> 🇨🇿 [Česká verze tohoto dokumentu](README.cs.md)

# Pexesooo

A self-contained, browser-based **memory matching game** (known as *pexeso* in Czech and Slovak, *Concentration* / *Pairs* in English), a companion **set generator**, and a **printer**. Build your own card sets from two folders of images, get a single portable JSON file, and play on screen — or print the cards, cut them out, and play on paper. No server, no build step, no install.

> **Status:** v1.7.0 · three static HTML files · runs offline from `file://`

---

## What's in here

| File | What it is |
|---|---|
| `Pexesooo.html` | The game. Loads a set file and plays it for 1–6 players. |
| `PexesoooGenerator.html` | The set builder. Turns two folders of images into one `.json` set. |
| `PexesoooPrinter.html` | The printer. Turns a set into a print-ready PDF for cutting out. |
| `DESIGN.md` | Shared visual language (design tokens, typography, components). |
| `README.md` | This file. |

All three apps are single, dependency-free HTML files. Just open them in a browser.

---

## Features

- **Make your own sets** from your own images — flags, animals, vocabulary, family photos, anything.
- **1:1 (identical) pairs:** point both folder pickers at the **same folder** to make a classic pexeso where both cards of a pair show the *same* picture.
- **Image-only card pairs:** PNG, JPG, and JPEG files are supported.
- **Two output resolutions:** build **online** sets (200×200, smaller files) or **printer-friendly** sets (600×600, higher-quality images for printing). The choice applies to the card images and the card back.
- **Self-contained sets:** images are embedded in the JSON, so a set is one file you can share, e-mail, or commit to a repo.
- **Print to paper:** turn any set into a PDF and cut out physical cards — **single-sided**, **fold-over**, or **true double-sided (duplex)**, at your chosen card size.
- **Preview the pairs** before playing — see every pair of a set laid out at a glance.
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

   **Tip — identical pairs:** to make a classic pexeso where both sides of a pair are the *same* picture, choose the **same folder** for both Folder A and Folder B. Each file then pairs with itself (1:1).
2. Open `PexesoooGenerator.html`.
3. Enter a **set name** and a **description** (up to 256 characters; the description is a two-line field).
4. Choose **Folder A** and **Folder B** (a folder picker dialog opens — you select the folder, not individual files). After you pick a folder, the picker shows the **folder name and how many images it found** in brackets.
5. *(Optional for online sets, required for printer-friendly sets)* choose a **card back** image.
6. Choose a **resolution**:
   - **200×200 — online only** — smaller files, ideal for on-screen play.
   - **600×600 — printer-friendly** — larger, higher-quality images for printing; a **card back is required**.

   The resolution applies to both the card images and the card back.
7. Click **Submit**. Review the detected pairs — incomplete or unreadable pairs are flagged and excluded; untick any you don't want.
8. Click **Generate JSON**. You get a file named `Pexesooo__<Set_Name>__200px.json` or `Pexesooo__<Set_Name>__600px.json` (the suffix records the resolution).

**Accepted files:** `PNG`, `JPG`, `JPEG`.
**Processing:** card images and the card back are center-cropped to the chosen resolution — **200×200** (online) or **600×600** (printer-friendly). JPEG quality is higher for printer-friendly sets.

### 2. Play (`Pexesooo.html`)

1. Open `Pexesooo.html`.
2. Under **Settings**, load a set using one of two methods:
   - **File** — choose a single `.json` set file directly. A preview shows the name, description, a single line of **pair count, creation date, and resolution**, sample pairs, and card back.
   - **Folder** — choose a folder; the game scans it (top level only, no recursion) for valid `.json` set files and lists all it finds. Click a row to select a set.
3. *(Optional)* click **Preview** to see all of the set's pairs laid out — each pair's two cards side by side, with the pair's file name below — before you start.
4. Pick the number of players, enter names, and set the timer / scoring options.
5. Click **Start game**.

**Turn rules**

- Flip up to two cards.
- **Match** → you keep the pair and **go again**.
- **No match** → the cards stay face-up so everyone can see them; **tap your own name** to end your turn (the cards flip back and play passes on). If the timer is on and runs out, the turn ends automatically.
- Won cards leave a **fixed empty gap** — positions never shuffle mid-game, so memory still matters.
- The game ends when the board is cleared. The **Results** section shows each player's score, their turn count, the pairs they won, and the total time.

In the results you can choose **Play again** (immediately restarts with the same settings, rotating the starting player) or **Different game** (returns to settings).

### 3. Print a set (`PexesoooPrinter.html`)

1. Open `PexesoooPrinter.html`.
2. Load a set exactly as in the game — by **File** (a single `.json`) or by **Folder** (the printer lists every valid set found at the top level). A full preview of each set is shown; click one to select it.
3. Under **Print options**, choose:
   - **What to print** — one of the four scenarios below.
   - **Card size** — **40 / 45 / 50 / 60 mm** (square; default **50 mm**).
   - **Page size** — **A4** or **US Letter**.
   - *(duplex only)* **Back offset** — an optional **X / Y** nudge in millimetres to correct your printer's front/back alignment (see the duplex note below).
4. Click **Generate PDF**. The file downloads locally, named `Pexesooo__<Set_Name>__<resolution>px__<scenario>__<size>mm__<PageSize>.pdf` (a non-zero back offset adds an `__x<X>_y<Y>mm` segment).

**Print scenarios**

- **Card images only** — every card face is printed at the chosen size, with corner cut marks. Card backs are ignored.
- **Card faces + backs** — each card prints as a **fold-over strip**: the face plus the set's shared back (rotated so it reads upright once folded). Print, cut out each strip, fold along the dashed line, and glue the halves back-to-back.
- **Duplex — long edge** — faces and backs on alternating pages for **double-sided printing with long-edge binding**; the backs are laid out so they register behind the faces after the flip.
- **Duplex — short edge** — the same, arranged for **short-edge binding**.

**Notes**

- The card back is the whole point of the faces+backs and duplex scenarios, so a set **without a card back** can only be printed with **Card images only** — the other three are disabled until the set has a back.
- Images below **600 × 600 px** print fine but may look soft; the app shows an advisory. Build **printer-friendly (600×600)** sets in the generator for the best print quality.
- For duplex, **run a single test page first** and check that the backs line up behind the faces. Make sure your printer's duplex setting matches the scenario (**long-edge** vs **short-edge** binding). If the backs are consistently off, dial the misalignment into **Back offset** (**X** = right, **Y** = up, in mm) and reprint.

The PDF is built entirely in your browser — no library, no upload — with the card images embedded directly.

---

## Set file format

A set is a single JSON file. Images are stored inline as base64 data URIs, so the file is fully portable. All three apps read and write this same format.

```json
{
  "_source": "https://github.com/KarlM0/Pexesooo/",
  "format": "pexesooo",
  "version": 1,
  "generator": "PexesoooGenerator/1.7.0",
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
- `thumb.size` records the resolution the images were produced at — **200** (online) or **600** (printer-friendly).
- `back` is **optional** in the format. If absent (or unreadable), the game renders the **set name** as the card back. (Note: the generator *requires* a back when producing a printer-friendly set, and the printer *requires* a back for its faces+backs and duplex scenarios.)
- `_source` identifies the project URL and is informational only; the apps ignore it.
- `format` must be `"pexesooo"`. Sets produced by earlier versions of the generator (which used `"pexeso"`) are not compatible.

---

## Design

All three apps follow a shared dark-theme design language defined in `DESIGN.md` (CSS custom properties, three typefaces — Fraunces / Manrope / JetBrains Mono — and a small component library). A few elements are deliberate, documented extensions of that system:

- **Card flip** — a restrained 0.3s eased `rotateY`, a stated exception to the base "single entrance animation" motion rule.
- **File / folder picker pair, set list, resolution / option selectors, player turn-strip, pair-preview grid, back-offset inputs, repo link, and the loading indicator** — derived from the base tokens where the component library has no existing pattern.
- **Printed PDF output** (card layout, corner cut marks, fold lines) is paper, not app chrome, so it is intentionally outside the token system.

These are flagged in the source as candidates for inclusion in `DESIGN.md`.

---

## Security

All three apps are designed to run entirely in the browser with no server involvement. A few measures are in place to limit the impact of a malicious set file:

- **Content Security Policy** — a `<meta>` CSP tag blocks all outbound network connections from the page, restricts images to `data:` and `blob:` URIs, and limits fonts to Google Fonts.
- **Safe image rendering** — card images are always set via DOM property assignment (`img.src = …`), never interpolated into `innerHTML`, preventing attribute injection attacks from crafted set files.
- **Input limits** — set files with more than 500 pairs or individual image sources larger than 3 MB are rejected during validation.

Only load set files from sources you trust.

---

## Browser support & limitations

- Tested as standards-based HTML/CSS/JS; works in current Chromium, Firefox, and Safari.
- The folder selection in the apps uses the `webkitdirectory` file input; support is broad but the exact behavior of the folder dialog varies by browser/OS.
- **HEIC** images (common on iPhones) are **not** supported by browsers — convert to JPG/PNG first before building a set.
- Image orientation: the generator applies EXIF orientation when re-encoding, but verify with a rotated phone photo on your browser.
- **Printing:** the exact behavior of duplex depends on your printer/driver; use the printer's **test page** and **Back offset** to get front/back registration right, and match the duplex binding (long vs short edge) to the chosen scenario.
- **No persistence** — there's no save/resume; settings, game progress, and the printer's back-offset value live only in the current tab.
- **Offline:** the apps work from `file://`; without internet the web fonts fall back to system fonts (layout and behavior are unaffected).
- The folder picker in the game and the printer scans only the **top level** of the chosen folder; JSON files in subfolders are ignored.

---

## Privacy

Everything happens in your browser. Your images never leave your device; the apps make no API calls and store nothing remotely. The printer builds its PDF locally, too — nothing is uploaded to print.

---

## Versioning

- App version is shown in each app's header (`Pexesooo v1.7.0`, `PexesoooGenerator v1.7.0`, `PexesoooPrinter v1.7.0`).
- The set file records the generator version in its `generator` field and the schema version in `version`.
