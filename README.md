# Suzerain Trainer

A live in-game editor for **Suzerain** (Sordland base game and the Rizia DLC). Press **F1**, change a value, and it applies immediately, with no save editing and no reload. Built as a BepInEx plugin.

> Single-player only. Back up your saves first. Not affiliated with Torpor Games. Use at your own risk; achievement behaviour while cheating is untested.

## What it does

- In-game overlay (F1 to show or hide, drag the title bar to move it).
- **Sordland** and **Rizia** tabs with the commonly edited values: budget, wealth, unrest, opinions, relations, military, diplomacy, houses, religions, story flags and more.
- Per-row **Set**, **-/+** steppers and **Lock**. Lock re-applies a value 4 times a second, so the game can't push it back.
- Bulk buttons: **Fill recommended**, **Set all shown**, **Set + lock shown**, **Unlock shown**, **Unlock all**.
- Filter box on every tab.
- **All vars** tab: browse and edit any of the roughly 12,800 game variables.
- **Lua console** and **Auto-scan** (writes `suzerain_diagnostic.txt`) for digging around.

## How it works

Suzerain keeps its state in the Pixel Crushers Dialogue System's Lua `Variable` table (keys like `BaseGame.GovernmentBudget` and `RiziaDLC.Resources_Authority`). The plugin reads and writes that table directly. After a Set it reads the value back and shows `before -> after` in the status bar.

The HUD only redraws on its own events, so a value can be changed while the top bar still shows the old number. End the turn or reopen a screen to see it update.

## Requirements

- Suzerain on Steam, tested on Unity **6000.3.9f1** (IL2CPP, metadata v39).
- **BepInEx 6 IL2CPP win-x64**, build **be.789** or newer: <https://builds.bepinex.dev/projects/bepinex_be>
- **.NET SDK 6.0 or newer** to build the plugin.
- A few GB of free RAM and page file (see Troubleshooting).

## Install

1. Download `BepInEx-Unity.IL2CPP-win-x64-6.0.0-be.<build>.zip` and extract it **next to `Suzerain.exe`**, so `winhttp.dll` and the `BepInEx` folder sit beside it.
2. Launch the game once and wait. The first run generates interop files and can take a few minutes. Close the game after it reaches the menu.
3. Open `BepInEx\config\BepInEx.cfg` and set:
   ```
   [Logging]
   UnityLogListening = false
   ```
   This avoids a known Unity 6 crash.
4. Check that `BepInEx\interop\Assembly-CSharp.dll` exists.
5. Build the plugin (edit `GameDir` in `SuzerainTrainer.csproj` first if your game isn't in the default Steam path):
   ```
   dotnet build -c Release
   ```
6. Copy `bin\Release\net6.0\SuzerainTrainer.dll` into `BepInEx\plugins\`.
7. Launch, load a save, press **F1**, then click **Refresh**.

## Usage

| Control | What it does |
|---|---|
| **F1** | Show or hide the overlay (works while the game window is focused) |
| Drag title bar | Move the window |
| **Refresh** | Re-read current values from the game (do this after loading a save) |
| **Set** | Write the value in the box. Highlighted when it differs from the current value |
| **- / +** | Step the box by 1 (10 for large values) |
| **Lock** | Hold the value; re-applied 4 times a second |
| **Fill recommended** | Fill the boxes with suggested values (not applied until you press Set) |
| **Set all shown / Set + lock shown** | Apply every visible row, optionally locking it |
| **Unlock shown / Unlock all** | Release locks |
| **Dump file** | Write all variables to `suzerain_variables.txt` in the game folder |
| **Auto-scan** | Probe the Lua environment and write `suzerain_diagnostic.txt` |

Tips:
- Load a save first. Variables don't exist at the main menu.
- Rows that show `n/a in save` aren't in your current save yet, usually because you haven't reached that part of the story.
- Some values have in-game caps (the notes column lists the ones I know). Going past them can cause odd behaviour.
- Rows can be recalculated by the game every turn. Use Lock for anything that keeps reverting.

## Troubleshooting

- **Game crashes while loading a save, with "Could not allocate memory" in `Player.log`:** the machine ran out of memory. Close other programs, make sure Windows manages the page file, and try again. BepInEx adds some overhead.
- **Crash on first launch with BepInEx:** make sure `UnityLogListening = false` is set.
- **F1 does nothing:** check `BepInEx\LogOutput.log` for the `[stage N]` lines. If it stops at stage 2 or 3, the plugin loaded but Unity isn't calling it; paste the log in an issue.
- **Refresh shows 0 loaded or a bulk dump error:** the curated rows still load. Run **Auto-scan** and check the report.
- **Value changes but the HUD doesn't:** the HUD refreshes on its own events. End the turn or open and close a panel.
- **`Albin_Convinced` / `Gloria_Convinced` rows show n/a:** the real variable names may carry a prefix. Use the All vars tab and filter by `convinc`, `albin` or `gloria`.

## Files

```
SuzerainTrainer/
  SuzerainTrainer.csproj   build file (set GameDir)
  Plugin.cs                BepInEx entry point
  TrainerUI.cs             overlay and Lua access
  Rows.cs                  the curated variable list (generated)
```

To add your own rows, add an entry to `Rows.cs`: group (`Sordland` or `Rizia`), section, exact variable key, label, whether it's a bool, recommended value and a note.

## Uninstall

Delete `SuzerainTrainer.dll` from `BepInEx\plugins\`. To remove BepInEx entirely, delete `winhttp.dll`, `doorstop_config.ini` and the `BepInEx` folder from the game folder.

## Credits

BepInEx, Il2CppInterop and Cpp2IL by their respective authors. Suzerain by Torpor Games. The Dialogue System is by Pixel Crushers.
