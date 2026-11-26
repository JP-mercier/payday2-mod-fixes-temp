# PAYDAY 2 mod fixes (2026-11-25)

Patched files for crashing mods. Only changed files are included.

## Install

1. Close the game.
2. Download [PD2_mod_fixes_2026-11-25.zip](https://github.com/JP-mercier/payday2-mod-fixes-temp/releases/download/v2026-11-25/PD2_mod_fixes_2026-11-25.zip).
3. Copy its `mods` folder into the PAYDAY 2 folder (next to `PAYDAY2.exe`) and replace files.
4. Delete `mods/unlock_all_dlcs`, `mods/unlock_only_heists` and `mods/unlock_only_characters`.

Auto-updating mods (BeardLib, Better Assault Indicator, Extra Heist Info, HoloUI, Hotline Miami Hud, Menu Backgrounds, Perfect View Model, VanillaHUD Plus, Mental Breakdown) overwrite these patches when they update.

## Fixed

| Mod | Problem |
|---|---|
| glowslop | Contour crash when a cop died during an async material swap |
| Better Assault Indicator | Hotline Miami HUD compat crash at assault start |
| VanillaHUD Plus | TabStats crash when changing a setting; `alive()` checks in Interaction and WaypointsManager |
| Joining Screen Selector | `managers.hud` nil crash; dead mod-list panels reused |
| Perfect View Model | Missing time argument in stance toggle |
| Extra Heist Info | `alive()` checks on loot waypoints (`set_color` on removed GUI objects) |
| Hotline Miami Hud | `alive()` checks in PacifiedCivs |
| Mental Breakdown | Duplicate hook id; "Animated" category never added |
| BeardLib | Version compare error during update checks |
| HoloUI, Menu Backgrounds | Hooks on functions the game no longer has |
| Day & Night Full | 38 entries for files the mod does not ship |

Details are in `README.txt` inside the zip. Not tested in game.
