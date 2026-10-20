# Changelog

All notable changes to Suzerain Trainer. Versions follow [Semantic Versioning](https://semver.org/).

## [1.0.0] - First public release

Tested on Suzerain (Unity 6000.3.9f1, IL2CPP) with BepInEx 6.0.0-be.789.

### Added
- In-game overlay (F1) with **Sordland**, **Rizia**, **Favorites**, **All vars**, **Presets**, **Types** and **Lua console** tabs.
- Live edits through the Dialogue System's Lua `Variable` table, with a read-back (`before -> after`) after every Set.
- **Locks** re-applied in one batched Lua call; saved to disk and restored on start.
- **Only raise / only lower** protection for bulk actions, so values that are already better are never reduced.
- **Presets** (save locks or changed edit boxes) and six built-in **macros**.
- **Undo last** and **Undo all** for the current session.
- **Auto-backup** of the newest save before the first edit of a session, plus a manual button.
- **Favorites** and typed editing (toggles, steppers) for any of the ~12,800 game variables.
- Live value refresh while the overlay is open.
- Soft warnings for values above caps quoted by community guides.
- BepInEx config file: toggle key, UI scale, lock and refresh intervals, backup settings.
- UI scaling (A- / A+), draggable window, optional click blocking (beta).
- **Auto-scan** report, **Lua console** and **Types** explorer for troubleshooting and modding.
- `install.ps1` to install BepInEx, build the plugin and uninstall.
- `README.md`, `SECURITY.md` and `CONTRIBUTING.md`.

### Known issues
- The game's HUD refreshes on its own schedule, so a changed value may not show until a turn ends or a screen reopens.
- `Albin_Convinced` and `Gloria_Convinced` may show `n/a` depending on the save.
- UI font scaling and click blocking are lightly tested.
