# Contributing to Suzerain Trainer

Thanks for helping. This is a small hobby project, so the process is light, but a few rules keep it safe and maintainable.

## Ground Rules

- **Single-player only.** No features aimed at multiplayer, online or competitive play. (I mean, why?)
- **Never commit game content.** That means no Suzerain code, assets or text, no `BepInEx\interop` DLLs, no dumped metadata, and no decompiled or Il2Cpp-dump output. Variable names and short snippets needed to explain a bug are fine.
- **Never commit binaries.** The repository is source-only; see the README. Don't add `.dll`, `.exe` or `bin/obj` folders.
- **No networking, telemetry or obfuscated code.** Anything that phones home or hides what it does won't be merged. See [SECURITY.md](SECURITY.md).
- Be kind. Assume good faith and keep discussion about the code.

## Ways to Help

- Report bugs and unclear docs
- Confirm or correct variable names, caps and the recommended values
- Find the real names for the two flags that show `n/a` (`Albin_Convinced`, `Gloria_Convinced`)
- Help find the game's HUD refresh method (the **Types** tab is there for this)
- Test on other game builds and report what changes

## Reporting a Bug

Open an issue with:

1. **What happened** and what you expected
2. **Versions:** plugin, Suzerain build, Unity version (shown in `LogOutput.log`), BepInEx build
3. **Steps to reproduce**, including whether you were in Sordland or Rizia and which save point
4. **`BepInEx\LogOutput.log`**, or at least the lines from the first `[stage N]` onward and any exception
5. **Auto-scan output** (`suzerain_diagnostic.txt`) if the problem is a missing or wrong variable

For crashes while loading a save, also include the end of `Player.log` and say how much RAM you have. "Could not allocate memory" crashes are usually the machine running out of memory, not the plugin.

**Security problems:** don't file a public issue. Follow [SECURITY.md](SECURITY.md).

## Requesting a Feature

Describe the problem you're trying to solve, not only the button you'd like. Say whether it needs a new variable, a new tab or a new setting.

## Development Setup

1. Install the **.NET SDK 6.0+** and **BepInEx 6 IL2CPP (be.789 or newer)** into your Suzerain folder.
2. Launch the game once so BepInEx generates `BepInEx\interop`.
3. Build:
   ```
   dotnet build -c Release
   ```
   Set `GameDir` in `SuzerainTrainer.csproj` (or pass `-p:GameDir=...`) if your game isn't in the default Steam folder.
4. Copy `bin\Release\net6.0\SuzerainTrainer.dll` into `BepInEx\plugins\` and launch.

No game handy? You can still syntax-check by compiling the sources against small stub types for the Unity, BepInEx and Dialogue System calls the code uses. Say in your pull request what you could and couldn't test.

## Project Layout

| File | Purpose |
|---|---|
| `Plugin.cs` | BepInEx entry point and config bindings |
| `Behaviour.cs` | Thin injected `MonoBehaviour` that forwards `Update` and `OnGUI` |
| `TrainerUI.cs` | All overlay logic (`Trainer` class) |
| `LuaApi.cs` | Lua read, write and batch helpers, including all escaping |
| `Store.cs` | Saved state, presets and save backups |
| `Macros.cs` | Built-in one-click macros |
| `Explorer.cs` | Type explorer and optional click blocking |
| `Rows.cs` | Curated variable list |
| `install.ps1` | Installer, builder and uninstaller |

## Coding Guidelines

- **Keep the injected class thin.** Il2CppInterop can't wrap methods that use `Dictionary`, `List` or custom types, and logs warnings when it tries. Put logic in `Trainer` (a plain class), not in `TrainerUI`.
- **Wrap Unity and game calls in `try/catch`.** A failed call should set the status line or log a warning, not throw every frame.
- **Escape everything that goes into Lua.** Use `LuaApi.Esc`, `LuaApi.ToLiteral` and the helpers in `LuaApi.cs`. Never build Lua by concatenating raw preset, variable or user text.
- **Lua in this game runs on a trimmed interpreter.** Avoid `string.*` functions in snippets (they failed in testing). Plain `pairs`, `tostring`, `table.concat` and `pcall` are known to work.
- **Batch Lua calls.** One call that handles many variables beats many small calls (see `ApplyLocks` and `ReadMany`).
- **Draw through `R(x, y, w, h)`** so the UI scale setting keeps working, and avoid allocating in `OnGUI` where you can.
- **Only write config values when they change.** Setting a BepInEx config entry saves the file.
- Match the existing style: 4-space indent, braces on their own line for types and methods, short comments that explain *why*.

## Adding or Changing Rows and Macros

A row in `Rows.cs` is: group (`Sordland` or `Rizia`), section, exact variable key, label, whether it's a bool, recommended value, note, direction and cap.

- **Direction:** `+1` means bigger is better (bulk actions only raise), `-1` means smaller is better (only lower), `0` means set exactly (used for flags).
- **Cap:** a known limit from community guides, or `double.NaN`. It only produces a warning.
- **Verify the key** in a real save with **Auto-scan** (it lists FOUND and MISSING keys) before you submit.
- **Macros:** every key in `Macros.cs` must exist in `Rows.cs`. A quick check that loads both and asserts this is easy to add and worth running.

## Before You Open a Pull Request

Run the checks that apply and tick them off in the PR description:

- [ ] Builds with no errors (`dotnet build -c Release`)
- [ ] Game starts and `LogOutput.log` shows stages 1 to 3 with no exceptions
- [ ] **Refresh** loads variables and the curated rows show values
- [ ] **Set** shows a sensible `before -> after` and, if the HUD didn't move, you checked after ending a turn
- [ ] **Lock** holds a value across a turn and survives a game restart
- [ ] Presets save, apply and delete; **Undo last** and **Undo all** work
- [ ] A backup appears in `TrainerBackups` after the first edit
- [ ] UI looks right at the default scale (1.0) and one other (for example 1.4)
- [ ] No game files, binaries or personal data are included
- [ ] README or settings docs updated if behaviour changed

## Pull Request Guidelines

- Keep PRs focused: one fix or feature per PR.
- Describe what changed, why, and how you tested it (game build, Sordland or Rizia).
- Use clear commit messages, for example `Fix lock restore when the save has no Variable table`.
- Don't reformat unrelated code.
- Be ready for review feedback. Small follow-up commits are fine.

## Licensing

By contributing, you agree that your contribution is released under the same license as the project (see the `LICENSE` file once one is present). Don't contribute code you don't have the right to share.
