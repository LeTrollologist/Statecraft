# Suzerain: Statecraft

A work-in-progress trainer for *Suzerain*, built with BepInEx and Lua inspection.

Statecraft is a small modding project focused on making the game's internal variables easier to inspect and edit. The idea is to provide a straightforward interface for changing game values without having to dig through the game's internals or rely entirely on Cheat Engine.

**Status:** Unreleased, under development.

## Features

- **Variable scanning**  
  Scan Lua variables exposed by the game and inspect their current values.

- **Curated variable list**  
  Access commonly used variables without having to search through thousands of entries manually.

- **Value editing**  
  Change supported values, including government budget, personal wealth, and economic stats.

- **Rizia DLC support**  
  Includes variables for budget, authority, energy, military resources, and diplomatic relations.

- **Set and Lock controls**  
  Set supported variables to a chosen value and lock them where the current implementation permits.

## Current state

Variable scanning and direct reads and writes are working. Some changes may not appear in the game's interface immediately, even when the underlying variable has been updated. Depending on the value, the game may need to process a relevant event, update a situation, or advance to the next turn before the change becomes visible.

The trainer is still being developed. The interface, variable coverage, and handling of game state updates are ongoing work.

## Requirements

- Suzerain
- BepInEx
- A compatible build of the trainer plugin

Exact version compatibility and installation instructions will be provided alongside a public release.

## Installation

Statecraft is not publicly released yet, so installation instructions are not available at this stage.

Once a release is available, the plugin will be distributed with instructions for placing it in the appropriate BepInEx plugin directory.

## Compatibility

The trainer is being developed for Suzerain, including support for the Rizia DLC.

## Contributing

If you have experience with Suzerain's scripting, Lua integration, or BepInEx modding, I'd be interested in hearing from you. There's still plenty to investigate, particularly how the game updates its internal state and reflects variable changes in the UI.

## Disclaimer

Statecraft is an unofficial, community-made project. It is not affiliated with or endorsed by the developers or publishers of Suzerain.

Use it at your own discretion. Back up your saves before experimenting with modified game values.
