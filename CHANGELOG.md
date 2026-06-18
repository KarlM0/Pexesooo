# Changelog

All notable changes to **Pexesooo** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.2.0] - 2026-06-07

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

## [1.1.0] - 2026-06-07

### Added
- Generator: optional **card-back image** (single PNG/JPG/JPEG, center-cropped to 400×400).
- Generator: **Review backside** section above *Review pairs* (shown only when a back image is chosen) with the same validation and an include toggle.
- Generator: **illegal-filename-character stripping** in the set-name field (`< > : " / \ | ? *` and control characters are removed as you type, with an inline hint); spaces remain allowed in the name.
- Game: the custom card back is rendered on every card face-down and shown **enlarged** in the settings preview alongside name, description, and sample pairs.

### Changed
- Generator: generated files are now named **`Pexesooo__<Set_Name>.json`** (spaces replaced with underscores, lowercase extension).
- Set schema: added the optional **`back`** field. Backward compatible — sets without it fall back to rendering the set name as the card back; schema `version` stays `1`.
- Generator identifier bumped to `PexesoooGenerator/1.1.0`; both apps display `v1.1.0`.

## [1.0.0] - 2026-06-07

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

[Unreleased]: https://github.com/USERNAME/REPO/compare/v1.2.0...HEAD
[1.2.0]: https://github.com/USERNAME/REPO/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/USERNAME/REPO/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/USERNAME/REPO/releases/tag/v1.0.0
