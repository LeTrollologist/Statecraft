# Security Policy

Suzerain Trainer is a hobby plugin that edits a **single-player** game. It still runs code inside your game process, so this document explains exactly what it does, what it does not do, and how to report a problem.

## Supported Versions

| Version | Supported |
|---|---|
| 1.0.x | Yes |
| Pre-release builds (before 1.0.0) | No (upgrade) |

Only the latest release gets fixes. The plugin is tested against **Unity 6000.3.9f1** and **BepInEx 6.0.0-be.789**; other combinations are unsupported.

## Reporting a Vulnerability

**Please do not open a public issue for a security problem.**

1. Use GitHub's private reporting: open the repository's **Security** tab and choose **Report a vulnerability**.
2. If that option isn't available, open a public issue that says only *"I'd like to report a security issue privately"*, with no technical details, and wait for contact.

Please include:

- What you found and why it matters
- Steps to reproduce, the plugin version, game build and BepInEx build
- Any relevant lines from `BepInEx\LogOutput.log` (remove anything personal)

**What to expect.** This is maintained by one person in spare time, so there are no guarantees, but the aim is to acknowledge a report within about a week and ship a fix or a mitigation note as soon as practical. Coordinated disclosure is appreciated: please give a fix time before publishing details. Reporters are credited in the release notes unless they ask not to be.

## Security Model

### What the plugin does

- Runs inside the Suzerain process as a BepInEx plugin and reads and writes the game's Lua `Variable` table through the Dialogue System.
- Registers the hotkey by polling the Windows key state of **only the configured toggle key** (default F1), and **only while Suzerain is the foreground window**. It does not record, log or store any other keystrokes.
- Writes the files listed below, on your machine only.
- Optionally (off by default) pauses the game's UI `EventSystem` while the mouse is over the overlay.

### What the plugin does not do

- **No network access.** The plugin makes no connections of any kind. There is no telemetry, analytics, update check or remote logging.
- **No code download or execution.** It never fetches or runs code from outside your machine.
- **No access outside the game and its save and config folders.** It doesn't read other programs, browser data, credentials or personal files.
- **Not for multiplayer.** Suzerain is single-player. The plugin has no anti-cheat bypass features and isn't meant for any online or competitive context.

### Files the plugin reads and writes

| Location | What | When |
|---|---|---|
| `BepInEx\config\local.suzerain.trainer.cfg` | Settings | On first run and when you change settings |
| `BepInEx\config\SuzerainTrainer\state.json` | Locks, favorites, window position | Automatically |
| `BepInEx\config\SuzerainTrainer\presets\*.json` | Your saved presets | When you save or delete a preset |
| `...\LocalLow\Torpor Games\Suzerain\TrainerBackups\` | Copies of your newest save | Before the first edit of a session, and on **Back up save now** |
| `<game folder>\suzerain_variables.txt` | Dump of game variables | Only when you press **Dump file** |
| `<game folder>\suzerain_diagnostic.txt` | Auto-scan report | Only when you press **Auto-scan** |
| `<game folder>\suzerain_types.txt` | Type explorer output | Only when you use the **Types** tab |

The three report files contain game state and class names, not personal data, but review them before sharing publicly.

### Input handling

- Preset files and variable names are treated as **data**. Values are converted to Lua literals and escaped before use; they are never executed as code. A preset can still set any game variable, so only load presets from sources you trust.
- The **Lua console** deliberately runs whatever Lua you type, inside the game's own Lua environment. It exists for debugging and is not exposed to any other source.
- The **Types** explorer uses reflection to list class and method names. It reads metadata only and doesn't invoke game methods.

## Running Third-Party Binaries Safely

- **Build from source** whenever you can (`install.ps1` does it in one step). That is the strongest way to know what you're running.
- Only download this project from its official repository. Treat copies on other sites as untrusted.
- If a Release includes a DLL, check that its notes name the game and BepInEx versions it was built against, and compare it with your own build if you want to be sure.
- `install.ps1` downloads BepInEx from `builds.bepinex.dev`. Read the script before running it; it is short and plain PowerShell. Pass `-BepInExUrl` to use a copy you have already verified.

## Antivirus False Positives

BepInEx loads by placing a proxy `winhttp.dll` in the game folder, a technique that some antivirus products flag. That comes from BepInEx, not from this plugin, and affects only the game folder you installed it into. If you're uncomfortable, don't install it. Removing the plugin is as simple as deleting `SuzerainTrainer.dll` or running `install.ps1 -Uninstall`.

## Out of Scope

- Bugs in the game, BepInEx, Il2CppInterop or Cpp2IL (report those upstream)
- Cheating, balance or "this broke my save" problems that don't involve a security flaw (open a normal issue; restore from `TrainerBackups`)
- Issues requiring an attacker who already has full control of your computer
