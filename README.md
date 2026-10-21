<div align="center">

# 🎛️ Suzerain Trainer

### *Live In-Game Editor for Suzerain*

**Live Edits • Persistent Locks • Presets • Undo • Auto-Backup**

[![Release](https://img.shields.io/badge/Release-v1.0.0-6366F1?style=for-the-badge&logo=github&logoColor=white&labelColor=0f172a)](https://github.com/LeTrollologist/SuzerainTrainer/releases) [![Loader](https://img.shields.io/badge/Loader-BepInEx_6_IL2CPP-8B5CF6?style=for-the-badge&logo=unity&logoColor=white&labelColor=0f172a)](https://builds.bepinex.dev/projects/bepinex_be) [![Tested](https://img.shields.io/badge/Tested-Unity_6000.3.9f1-22C55E?style=for-the-badge&logo=unity&logoColor=white&labelColor=0f172a)](#known-limitations) [![Mode](https://img.shields.io/badge/Mode-Single--Player_Only-F59E0B?style=for-the-badge&logo=steam&logoColor=white&labelColor=0f172a)](SECURITY.md)

[![Windows x86_64](https://img.shields.io/badge/Target-Windows_x86__64-0078D4?style=flat-square&logo=windows&logoColor=white&labelColor=0f172a)](#quickstart-install-in-minutes) [![.NET 6](https://img.shields.io/badge/Plugin-C%23_%2F_.NET_6-512BD4?style=flat-square&logo=dotnet&logoColor=white&labelColor=0f172a)](#developer-experience--tooling) [![Game](https://img.shields.io/badge/Game-Sordland_%2B_Rizia_DLC-DC2626?style=flat-square&logoColor=white&labelColor=0f172a)](#overlay-at-a-glance) [![Source](https://img.shields.io/badge/Distribution-Source_Only-64748B?style=flat-square&logoColor=white&labelColor=0f172a)](#source-code--whats-not-included)

[![Quickstart](https://img.shields.io/badge/%E2%9A%A1_Quickstart-Install_in_Minutes-38BDF8?style=for-the-badge&logoColor=white&labelColor=0f172a)](#quickstart-install-in-minutes) [![Guide](https://img.shields.io/badge/%F0%9F%93%96_Guide-Using_the_Overlay-818CF8?style=for-the-badge&logoColor=white&labelColor=0f172a)](#using-the-overlay) [![Security](https://img.shields.io/badge/%F0%9F%9B%A1%EF%B8%8F_Security-Policy_%26_Safety-34D399?style=for-the-badge&logoColor=white&labelColor=0f172a)](SECURITY.md) [![Contributing](https://img.shields.io/badge/%F0%9F%A4%9D_Contributing-Guidelines-F472B6?style=for-the-badge&logoColor=white&labelColor=0f172a)](CONTRIBUTING.md)

## Screenshots

<img width="1033" height="698" alt="image" src="https://github.com/user-attachments/assets/f14313b8-c0b6-41f5-b4cb-0847756d78fa" />

</div>

---

> **Single-player only.** The plugin backs up your newest save before your first edit each session, but keep your own copy of anything you care about. Not affiliated with Torpor Games. Use at your own risk. Achievements still work while using this program, if you have issues with cheating, do not use it.

## Core Features

1. **Live Edits, No Reload**
Press **F1** and change budget, wealth, unrest, opinions, relations, military, diplomacy, houses, religions and story flags for Sordland and Rizia. Every Set reads the value back and shows `before -> after` in the status bar.

2. **Persistent Locks**
Tick **Lock** and the value is re-applied every 0.25 s in a single batched Lua call. Locks are saved to disk and restored the next time you start the game. Locking only touches variables that already exist, so it is safe at the main menu.

3. **Only Raise / Only Lower Protection**
Bulk actions never make a value worse. Setting a target of 100 leaves your 6,150 manpower alone, and "unrest 0" never raises unrest.

4. **Presets and One-Click Macros**
Save your locks or your changed edit boxes under a name and re-apply them any time. Six built-in macros ship in the box, such as *Rizia: rich*, *Rizia: war ready* and *Sordland: stable country*.

5. **Undo and Auto-Backup**
**Undo last** and **Undo all** revert everything changed this session. Your newest save is copied to `TrainerBackups` before the first edit, keeping the last 10.

6. **Favorites and the Full Variable Table**
Browse and edit any of the roughly 12,800 game variables with typed controls, and pin the ones you care about to a **Favorites** tab.

7. **Built-In Diagnostics**
A **Lua console**, an **Auto-scan** report and a **Types** explorer (search the game's classes and list their members) are included for modders and troubleshooting.

---

## Overlay at a Glance

| Tab | What it's for |
|---|---|
| **Sordland** | Base-game economy, politics, relations, prologue and military flags |
| **Rizia** | DLC economy, military, welfare, diplomacy and council values |
| **Favorites** | Variables you pinned from All vars |
| **All vars** | Every game variable, filterable, with typed controls |
| **Presets** | Built-in macros, your saved presets, manual save backup |
| **Types** | Search game classes and list methods (writes `suzerain_types.txt`) |
| **Lua console** | Run Lua, dump variables, run **Auto-scan** |

How it works: Suzerain keeps its state in the Pixel Crushers Dialogue System's Lua `Variable` table (keys such as `BaseGame.GovernmentBudget` and `RiziaDLC.Resources_Authority`). The plugin reads and writes that table directly. The HUD only redraws on its own events, so a value can change while the top bar still shows the old number. End the turn or reopen a screen to see it update.

---

## Quickstart (Install in Minutes)

**Requirements:** Suzerain on Steam (tested on Unity **6000.3.9f1**, IL2CPP), **BepInEx 6 IL2CPP win-x64** build be.789 or newer (the script fetches it), and the **.NET SDK 6.0+**. Keep a few GB of free RAM and page file; see [Troubleshooting](#troubleshooting).

### 1. Easy Install (`install.ps1`)

```
powershell -ExecutionPolicy Bypass -File .\install.ps1
```

Add `-GameDir "D:\path\to\Suzerain"` if the game is not in the default Steam folder.

- **Stage 1** installs BepInEx and presets `UnityLogListening = false`. Launch Suzerain once, wait for the main menu, and close it.
- **Stage 2** (run the same command again) builds the plugin and copies it into `BepInEx\plugins`.

```
# Remove the plugin later
powershell -ExecutionPolicy Bypass -File .\install.ps1 -Uninstall
```

### 2. Manual Install

1. Extract `BepInEx-Unity.IL2CPP-win-x64-6.0.0-be.789...zip` from [builds.bepinex.dev](https://builds.bepinex.dev/projects/bepinex_be) next to `Suzerain.exe`.
2. Launch once and close. Set `UnityLogListening = false` under `[Logging]` in `BepInEx\config\BepInEx.cfg` (avoids a known Unity 6 crash).
3. Confirm `BepInEx\interop\Assembly-CSharp.dll` exists.
4. Edit `GameDir` in `SuzerainTrainer.csproj` if needed, then:
   ```
   dotnet build -c Release
   ```
5. Copy `bin\Release\net6.0\SuzerainTrainer.dll` into `BepInEx\plugins\`.

Then launch, load a save, press **F1**, and click **Refresh**.

---

## Using the Overlay

| Control | What it does |
|---|---|
| **F1** | Show or hide (works while the game is focused; rebindable) |
| Title bar | Drag to move the window |
| **A- / A+** | Make the overlay smaller or larger |
| **Refresh** | Re-read every variable. Do this after loading a save |
| **Set** | Write the box value. Highlighted when it differs from the current value |
| **- / +** | Step the box by 1 (10 for large values) |
| **Lock** | Hold the value. Persists across sessions |
| **Fill recommended** | Fill the boxes with suggested values (applied only when you press Set) |
| **Set all shown / Set + lock shown** | Apply every visible row, honouring the filter and *Only raise / only lower* |
| **Undo last / Undo all** | Revert changes made this session |
| **Fav** (All vars) | Pin a variable to the Favorites tab |
| **Back up save now** (Presets) | Copy your newest save to `TrainerBackups` immediately |
| **Auto-scan** (Lua console) | Report on the Lua environment and curated keys (writes `suzerain_diagnostic.txt`) |

Tips:

- Load a save first. Variables don't exist at the main menu.
- Rows showing `n/a in save` aren't in your current save yet.
- An orange input box means the value is above a cap quoted by community guides. It is a warning, not a block, and some caps are out of date.
- The game may recalculate some values each turn. Lock anything that keeps reverting.

### Settings

Edit `BepInEx\config\local.suzerain.trainer.cfg` (created on first run):

| Setting | Default | Meaning |
|---|---|---|
| `ToggleKeyVirtualKeyCode` | 112 (F1) | Windows virtual-key code to show or hide |
| `UiScale` | 1.0 | Overlay size (0.7 to 2.0) |
| `BlockClickThrough` | false | Pause the game's UI input while the mouse is over the overlay (beta) |
| `IntervalSeconds` (Locks) | 0.25 | How often locks are re-applied |
| `RestoreLocksOnStart` | true | Reload your locks when the game starts |
| `LiveRefreshSeconds` | 1.0 | How often visible values refresh |
| `BackupSaveBeforeFirstEdit` | true | Auto-backup before the first edit |
| `BackupsToKeep` | 10 | Backup files to keep |

Presets, locks and favorites live in `BepInEx\config\SuzerainTrainer\`. Backups go to `...\LocalLow\Torpor Games\Suzerain\TrainerBackups\`.

---

## Workspace Architecture

```
SuzerainTrainer/
├── install.ps1              # Installer / builder / uninstaller (BepInEx + plugin)
├── SuzerainTrainer.csproj   # Build file (set GameDir)
├── Plugin.cs                # BepInEx entry point and config bindings
├── Behaviour.cs             # Thin injected component (forwards Update / OnGUI)
├── TrainerUI.cs             # All overlay logic: locks, undo, presets UI, polling
├── LuaApi.cs                # Lua read / write / batch helpers (escaping lives here)
├── Store.cs                 # Saved state, presets, save backups
├── Macros.cs                # Built-in one-click macros
├── Explorer.cs              # Type explorer and optional click blocking
├── Rows.cs                  # Curated variable list (key, label, direction, cap)
├── README.md                # This file
├── CHANGELOG.md             # Release history
├── SECURITY.md              # Security model, scope and how to report issues
└── CONTRIBUTING.md          # How to report bugs, build, test and open a PR
```

---

## Developer Experience & Tooling

### 1. Build From Source

```
dotnet build -c Release
```

The build links against `BepInEx\interop\*.dll`, so launch the game once after installing BepInEx first.

### 2. Add or Change Rows

Append to `Rows.cs`: group, section, exact variable key, label, bool?, recommended value, note, direction (`+1` raise, `-1` lower, `0` exact) and cap (or `double.NaN`). Run **Auto-scan** to confirm the key exists in a real save.

### 3. Read the Log

`BepInEx\LogOutput.log` carries numbered stages: stage 3 means Unity is calling the plugin, stage 6 means the overlay drew a full frame. Every Set is logged with its `before -> after`.

### 4. Uninstall

Delete `SuzerainTrainer.dll` from `BepInEx\plugins\`, or run `install.ps1 -Uninstall`. To remove BepInEx too, delete `winhttp.dll`, `doorstop_config.ini` and the `BepInEx` folder from the game folder.

---

## Source Code & What's Not Included

This repository is **source-only**. There is no prebuilt `SuzerainTrainer.dll` checked in.

- **The plugin source is complete.** Every file needed to build the plugin is here, and `install.ps1` builds it in one step.
- **Why you build it yourself:** the plugin compiles against the `BepInEx\interop` assemblies that BepInEx generates on *your* machine from *your* copy of Suzerain at first launch. Those files are derived from the game, so they are not shipped or redistributed here, and a build has to match your game and BepInEx versions.
- **Why not just ship a prebuilt DLL?** Many consider save editing a form of cheating to begin with, and this project takes that idea a step further. The trainer makes it considerably easier to alter your experience without repeatedly closing the game, manually editing save files, and reopening it. However, that convenience shouldn't mean removing every bit of effort involved. You should understand the installation process and take responsibility for how you use the power to edit the game.
The goal isn't to play Suzerain for you. It's to give you control over your experience without optimizing your every decision. The consequences of your edits remain yours to deal with. Bad edits can produce unexpected results or worse, crashes, which is why backups and undo functionality matter.
There's a certain irony to it, if you want to bypass Suzerain's intended difficulty, you still have to put in a little work to set up the means of doing so.
- **No game content is included:** no Suzerain code, assets, text, dumped metadata or decompiled output. The variable names in `Rows.cs` are plain identifiers that the game itself exposes.
- **BepInEx is not bundled.** `install.ps1` downloads the official build from `builds.bepinex.dev`.
- **Release builds, if any:** a DLL attached to a Release was built against Unity 6000.3.9f1 and the BepInEx build named in the release notes. After a game or BepInEx update, rebuild from source. Building it yourself is the recommended way to verify what you are running (see [SECURITY.md](SECURITY.md)).

---

## Known Limitations

- **HUD refresh:** values are written correctly but the HUD updates on its own schedule. The **Types** tab exists to help find the game's refresh method; it isn't wired up yet.
- **UI scaling** changes IMGUI font sizes at runtime. If it misbehaves, set `UiScale = 1.0`; the plugin disables scaling by itself if the font calls fail.
- **Block clicks** is experimental and off by default.
- `Albin_Convinced` and `Gloria_Convinced` may show `n/a`. Their real names depend on your save: filter **All vars** by `convinc`, `albin` or `gloria`.
- Backups copy the newest save on disk, which may be older than what is currently in memory.
- Tested on one game build (Unity 6000.3.9f1). A game update can change variable names or break the build.

---

## Troubleshooting

- **Crash on first launch with BepInEx:** set `UnityLogListening = false`.
- **Crash while loading a save, "Could not allocate memory" in `Player.log`:** Windows ran out of memory. Close other programs and make sure Windows manages the page file.
- **F1 does nothing:** check `BepInEx\LogOutput.log` for the `[stage N]` lines.
- **Refresh reports a bulk dump error:** the curated rows still work. Run **Auto-scan** and read the report.
- **Value changes but the HUD doesn't:** the HUD refreshes on its own events. End the turn or open and close a panel.
- **Build errors:** make sure `BepInEx\interop` exists (launch the game once after installing BepInEx) and that `GameDir` is correct. Paste the first few error lines in an issue.

---

## Security, Contributing & Credits

- 🛡️ Read the [Security Policy](SECURITY.md) for what the plugin does and doesn't do, which files it writes, and how to report a vulnerability privately.
- 🤝 Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening an issue or pull request.
- BepInEx, Il2CppInterop and Cpp2IL are by their authors. Suzerain is by Torpor Games. The Dialogue System is by Pixel Crushers.
- [SubstantialSky7326](https://www.reddit.com/user/SubstantialSky7326/) for the [guide](https://www.reddit.com/r/suzerain/comments/1ft4tjv/all_suzerain_cheats_20/).
