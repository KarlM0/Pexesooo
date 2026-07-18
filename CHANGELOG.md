# Changelog

All notable changes to **Pexesooo** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
- Add credits field to the generator.
- Display credits field in the set selection as alt text.
- Add hashes to the README files.

## [1.8.0] - 2026-07-18

### Changed
- **`PexesoooPrinter.html`**
  - Baseline constants reset to all zeros (`DUPLEX_BASELINE_CONSTANT_LONG = { x: 0, y: 0 }`, `DUPLEX_BASELINE_CONSTANT_SHORT = { x: 0, y: 0 }`). They are kept in the file to enable for user printer-specific corrections.
  - Pdf file name template changed to `Pexesooo__<Name>__<thumb.size>px__<cardSize>mm__<PageSize>__<scenario>[__x<X>_y<Y>mm].pdf`

- **`Pexesooo.html`**
  - The game file - `Pexesooo.html` renamed to `PexesoooGame.html`.
- **All apps**
  - All apps display **v1.8.0**; the generator identifier in produced sets is now `PexesoooGenerator/1.8.0`.


## [1.7.0] - 2026-07-17

### Added
- **`PexesoooPrinter.html`** — a third app in the suite: a print tool that loads a Pexesooo set (using the **same file/folder loader and preview as the game**) and generates a print-ready **PDF** entirely in the browser. The PDF is produced by a **self-contained inline writer** — no external library — with card JPEGs embedded losslessly via `/DCTDecode`. Fully client-side and offline-capable; Czech / English UI.
  - **Four print scenarios:**
    - **Card images only** — every card face is printed; card backs are ignored.
    - **Card faces + backs** — a **fold-over strip** per card (the face plus the shared back rotated 180°, with a fold line between them), folded and glued into a double-sided card.
    - **Duplex — long edge** — two-sided printing; the backs page is mirrored **horizontally** so each back registers behind its face after a long-edge flip (backs printed upright).
    - **Duplex — short edge** — two-sided printing; the backs page is mirrored **vertically** with the back rotated 180° for a short-edge flip.
  - **Card-size presets:** **40 / 45 / 50 / 60 mm** (square); default **50 mm**.
  - **Page size:** **A4** or **US Letter**.
  - **Duplex back-offset calibration** — a manual **X / Y** nudge (mm) for the duplex back page, applied on top of built-in per-edge baseline constants (`DUPLEX_BASELINE_CONSTANT_LONG = { x: 6, y: 0 }`, `DUPLEX_BASELINE_CONSTANT_SHORT = { x: 0, y: 0 }`) that correct a specific printer's front/back registration.
  - **Warnings / gating:** a resolution advisory when card or back images are below 600 × 600 px; a **missing card back hard-blocks** the scenarios that print a back.
  - **Output:** corner cut marks, an estimate line (cards, and pages or double-sided sheets), and a stamped filename: `Pexesooo__<Name>__<thumb.size>px__<cardSize>mm__<PageSize>__<scenario>[__x<X>_y<Y>mm].pdf`.
  - Folder-loaded sets are shown as **full previews** (matching a single loaded file), each selectable.
- **Game: pair preview.** A new **Preview / Náhled** button (to the left of *Start game*, enabled as soon as a set is selected) displays every pair of the selected set in the Game area — each pair's two cards side by side on a uniting surface (an enlarged version of the end-of-game won-pair display), with the pair's file name below.

### Changed
- Generator: the **Description** field is now a **two-line textarea**.
- Generator: the folder pickers now show the **selected folder name with the image count in brackets** (e.g. `worldflags (24)`) instead of a plain file count.
- Generator: the **Czech** label for *Submit* changed from **"Odeslat"** to **"Připravit"** (English unchanged).
- All three apps now display **v1.7.0**; the generator identifier stamped into produced sets is now `PexesoooGenerator/1.7.0`. The set schema is unchanged (`version` stays `1`) — sets remain backward compatible.

## [1.6.0] - 2026-07-09

### Added
- Generator: **resolution selector** — choose **200×200 (online only)** or **600×600 (printer-friendly)** for the produced images. The choice applies to both the card images and the card back. Default is **200×200**.
- Generator: printer-friendly (600×600) sets **require a card back** — the app will not generate a printer-friendly set unless one is provided; online (200×200) sets keep the card back optional.
- Docs: **README** now explains that pointing **both Folder A and Folder B at the same folder** produces a set of **1:1 (identical-image) pairs** — classic pexeso where both cards of a pair show the same picture.

### Changed
- Generator: JPEG quality is raised to **0.90** for printer-friendly (600×600) images; online (200×200) images stay at **0.82**.
- Generator: generated filenames now carry a resolution suffix — **`Pexesooo__<Set_Name>__200px.json`** or **`Pexesooo__<Set_Name>__600px.json`**.
- Generator: the set schema's **`thumb.size`** now records the actual resolution (**200** or **600**) instead of a fixed `200`; schema `version` stays `1` (backward compatible).
- Generator: the card-back field label now reads **"Card back (optional for online, mandatory for printer-friendly)"**.
- Game: the set preview shows **pair count, creation date, and resolution (`thumb.size`) on a single comma-separated line** (e.g. `26 pairs, 2026-06-15 20:34, 600 px`) in both file and folder modes. Older sets missing a timestamp or `thumb.size` simply omit those segments.
- Both apps display **v1.6.0**; the generator identifier in produced sets is now `PexesoooGenerator/1.6.0`.

### Fixed
- Game: **turn-counter off-by-one** — the turn that clears the board (the winning match) was never counted, leaving the solo player and the multiplayer winner one turn short. That final turn is now counted.

## [1.5.0] - 2026-06-20

### Security
- Both apps: **Content Security Policy** added via `<meta http-equiv="Content-Security-Policy">` restricting scripts to inline-only, images to `data:` and `blob:` URIs, fonts to Google Fonts, and blocking all outbound connections (`connect-src 'none'`).
- Both apps: **XSS via image src injection fixed** — all image elements are now created with `document.createElement('img')` and `.src` assigned as a DOM property, replacing all previous template-literal interpolation of image data URIs into `innerHTML`. A shared `makeSafeImg()` helper enforces this throughout.
- Both apps: all other user-data-derived content (set names, descriptions, pair stems) that previously flowed through `escHtml()` + `innerHTML` is now set via `textContent` directly.
- Game: **memory DoS mitigated** — `validSet()` now rejects set files with more than 500 pairs (`MAX_PAIRS`) or any individual image `src` exceeding 3 MB (`MAX_SRC_BYTES`).

### Changed
- Both apps display **v1.5.0**; the generator identifier in produced sets is now `PexesoooGenerator/1.5.0`.

## [1.4.0] - 2026-06-20

### Added
- Game: **folder picker** alongside the existing file picker — choose a folder and the game scans its top level for valid `.json` set files, lists them all, and lets the player select one by clicking.
- Game: folder set list displays **name, description, pair count, creation date, and card back thumbnail** (at half the standard size, no caption) for each found set; selected row is highlighted with an accent border.
- Game: **creation date** (`createdAt`) shown in set previews for both file and folder modes, formatted as `yyyy-mm-dd HH:mm` (UTC, language-neutral).

### Changed
- Game: set picker area is now a **side-by-side pair** of pickers (file left, folder right); each resets the other when used.

## [1.3.0] - 2026-06-20

### Added
- Both apps: **source attribution** — HTML comment `<!-- Source: https://github.com/KarlM0/Pexesooo/ -->` on line 2 of each file.
- Both apps: **GitHub link** ("View on GitHub" / "Zobrazit na GitHubu") below the version string in each app's header, linking to `https://github.com/KarlM0/Pexesooo/`.
- Generator: generated JSON files include a `"_source": "https://github.com/KarlM0/Pexesooo/"` field.

### Changed
- Both apps: **set format identifier** changed from `"pexeso"` to `"pexesooo"`. This is a breaking change — sets produced by v1.2.0 or earlier cannot be loaded in v1.3.0 or later.
- Both apps: **TXT file support removed** from both the generator and the game. Only image cards (`kind: "image"`) are produced and accepted going forward. Sets containing text cards (produced by earlier versions) are rejected as invalid.
- Generator: **description field** expanded from 128 to 256 characters.
- Both apps display **v1.3.0**; the generator identifier in produced sets is now `PexesoooGenerator/1.3.0`.

## [1.2.0] - 2026-06-20

### Added
- Game: **Play again / Hrát znovu** result button — immediately restarts with the current set, players, and options (reshuffled board).
- Game: **starting-player rotation** — each *Play again* advances the starting player by one position so every player starts equally often; **Start game** resets the rotation to the first player.
- Game: **per-player turn tracking**, shown in the Results. A turn is counted each time a player ends their turn — by tapping their name or by the timer running out. Counts reset on *Start game* and *Play again*.
- `README.md` and `CHANGELOG.md`.

### Changed
- Game: the Results button **New game** is renamed to **Different game / Jiná hra** (unchanged behavior: returns to Settings).
- **Default UI language is now Czech (CS)** in both apps (still switchable to EN at any time).
- Game: **Show score during play** now defaults to **off** — scores are revealed in the Results.
- Both apps display **v1.2.0**; the generator identifier in produced sets is now `PexesoooGenerator/1.2.0`.

## [1.1.0] - 2026-06-20

### Added
- Generator: optional **card-back image** (single PNG/JPG/JPEG, center-cropped to 400×400).
- Generator: **Review backside** section above *Review pairs* (shown only when a back image is chosen) with the same validation and an include toggle.
- Generator: **illegal-filename-character stripping** in the set-name field (`< > : " / \ | ? *` and control characters are removed as you type, with an inline hint); spaces remain allowed in the name.
- Game: the custom card back is rendered on every card face-down and shown **enlarged** in the settings preview alongside name, description, and sample pairs.

### Changed
- Generator: generated files are now named **`Pexesooo__<Set_Name>.json`** (spaces replaced with underscores, lowercase extension).
- Set schema: added the optional **`back`** field. Backward compatible — sets without it fall back to rendering the set name as the card back; schema `version` stays `1`.
- Generator identifier bumped to `PexesoooGenerator/1.1.0`; both apps display `v1.1.0`.

## [1.0.0] - 2026-06-20

### Added
- **`Pexesooo.html`** — the game:
  - 1–6 players with custom names and a turn indicator (active player highlighted).
  - Turn logic: flip up to two cards; a match is won and the player goes again; a non-match stays face-up until the current player taps their own name (or the timer expires), then flips back and play advances.
  - Won cards leave **fixed empty gaps** — board positions never reflow mid-game.
  - Optional **per-turn timer** with a countdown progress bar; on expiry the turn auto-ends.
  - **Live or end-of-game scoring**, toggleable.
  - **Completion time** tracked and shown in results for solo and multiplayer.
  - **English / Czech** UI, switchable at any time.
  - Three collapsible sections: Settings, Game, Results.
- **`PexesoooGenerator.html`** — the set builder:
  - Two-folder input via folder picker; pairs matched by **lowercased filename stem** (extension ignored).
  - Accepts **PNG, JPG, JPEG, and TXT** (text up to 64 characters).
  - Card images center-cropped to **200×200**.
  - Set name (≤32 chars) and description (≤128 chars).
  - **Review grid** with per-row include toggles; incomplete pairs and unreadable files are flagged and excluded.
  - Produces a **self-contained JSON** set with images embedded as base64.
- Shared **`DESIGN.md`** visual language; a restrained card-flip animation as a documented motion exception, plus derived components (file picker, review grid, player turn-strip) flagged for inclusion.
- Fully **client-side and offline-capable**: no server, no build, no network calls beyond loading web fonts.

[Unreleased]: https://github.com/KarlM0/Pexesooo/compare/v1.7.0...HEAD
[1.7.0]: https://github.com/KarlM0/Pexesooo/compare/v1.6.0...v1.7.0
[1.6.0]: https://github.com/KarlM0/Pexesooo/compare/v1.5.0...v1.6.0
[1.5.0]: https://github.com/KarlM0/Pexesooo/compare/v1.4.0...v1.5.0
[1.4.0]: https://github.com/KarlM0/Pexesooo/compare/v1.3.0...v1.4.0
[1.3.0]: https://github.com/KarlM0/Pexesooo/compare/v1.2.0...v1.3.0
[1.2.0]: https://github.com/KarlM0/Pexesooo/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/KarlM0/Pexesooo/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/KarlM0/Pexesooo/releases/tag/v1.0.0
