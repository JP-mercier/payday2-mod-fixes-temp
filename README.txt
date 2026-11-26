PAYDAY 2 mod fixes - 2026-11-25
================================

Patched files for mods that crashed or logged errors on every load.
Only the changed files are included.

INSTALL
-------
1. Close the game.
2. Copy the "mods" folder from this zip into your PAYDAY 2 folder
   (the one that contains PAYDAY2.exe). Say yes to "replace files".
3. Delete these three folders from PAYDAY 2\mods (see "Not fixed"):
     unlock_all_dlcs
     unlock_only_heists
     unlock_only_characters

Mods that auto-update (BeardLib, Better Assault Indicator, Extra Heist Info,
HoloUI, Hotline Miami Hud, Menu Backgrounds, Perfect View Model, VanillaHUD Plus,
Mental Breakdown) will overwrite these patches on their next update. Re-copy
the files if the same crash comes back after an update.

CRASH FIXES
-----------
glowslop (mods/glowslop/mod.lua)
  Crash: access violation in contourext.lua _upd_color (2026-11-25 17:13).
  The mod swaps enemy materials asynchronously and passed the contour callback
  straight to the engine, skipping CopBase:on_material_applied's alive() check.
  When a cop died or was cleaned up before the new material loaded, the contour
  code ran on a deleted unit. The callback now goes through the vanilla check.

Better Assault Indicator (lua/compatibility/hud/hotline_miami_hud.lua)
  Crash: hotline_miami_hud.lua:292 "attempt to index a nil value" at assault
  start (2026-11-24). The Hotline Miami HUD compatibility expects the "text"
  label that HMH only creates when its "assault" option is on at game start.
  Both uses now check that the label exists.

VanillaHUD Plus (lua/TabStats.lua)
  Crash: TabStats.lua:1013 "_tabstats_settings (a nil value)" when changing a
  Tab Stats option (2026-11-24). Returns early if the stats screen is not built.

Joining Screen Selector (menumanagerdialogs.lua)
  Crash: menumanagerdialogs.lua:275 "attempt to index field 'hud'" on a drop-in
  pause request outside a heist (2026-11-21). managers.hud is now checked.
  Also: the mod-list panels were kept across heists and the cleanup loop used
  ipairs on a table keyed by peer id 2-4, so it never ran. Later
  set_visible/remove calls on those dead panels are a native crash. Panels are
  now checked with alive() and the cleanup visits every entry.

Perfect View Model (PVM.lua)
  Crash: playerstandard.lua "attempt to compare nil with number" from the PVM
  menu (2026-11-22). Toggle called the steelsight/ducking actions without the
  time argument. It now passes the player timer and checks the player exists.

set_color on a removed GUI object (2026-11-25 17:21 and 17:45)
  The minidump shows a native crash inside the engine's set_color binding for
  GUI objects: some Lua code recolored a HUD element that had already been
  removed. Both crashes were mid-heist (Shacklethorn Auction, Mountain Master).
  The mod responsible could not be pinned down, so alive() checks were added to
  every place found that recolors objects it does not own:
    Extra Heist Info  lua/waypoints/EHILootWaypoint.lua (secure-loot waypoints:
                      SetColor, ReplaceWaypoint, SetText, timed update; dead
                      entries are dropped from the cache)
    Extra Heist Info  lua/hud/waypoints.lua (EHIWaypoint:SetColor, warning
                      animation on icon/arrow/world bitmap)
    VanillaHUD Plus   lua/Interaction.lua (interaction lock circle)
    VanillaHUD Plus   lua/WaypointsManager.lua (removed components are forgotten)
    Hotline Miami Hud lua/PacifiedCivs.lua (civilian suspicion waypoint colors)
  If this crash happens again, send crash.txt, crash.dmp and the BLT log
  (mods/logs/<date>_log.txt) right away, before restarting the game.

ERROR-LOG FIXES (no crash, logged on every load)
------------------------------------------------
Mental Breakdown (src/lootdrop-tweak.lua)
  Hooked DLCTweakData under the same hook id as dlc-tweak.lua, so BLT refused it
  ("Could not post hook function 'init'") and the "Animated" skin category was
  never added. Now hooks LootDropTweakData with its own id.

BeardLib (Modules/Utils/ModAssetsModule.lua)
  ModAssetsModule.lua:156 "attempt to compare number with nil" during update
  checks when a local version is a number but the server version is not.

HoloUI (Base.lua)
  Hooks on update_tab_position, set_tab_position and stage_spin_up failed
  because those functions no longer exist in the game. They are now skipped.

Menu Backgrounds (Hooks/StageEndScreen.lua)
  HUDStageEndCrimeSpreeScreen has no "show" of its own (it inherits the hooked
  HUDStageEndScreen one). The duplicate hook is skipped.

Day & Night Full (add.xml)
  38 material_config entries pointed to files the mod does not ship ("File does
  not exist!" x38 per load). The entries are removed; BeardLib was already
  skipping them, so nothing changes in game.

NOT FIXED
---------
- unlock_all_dlcs, unlock_only_heists, unlock_only_characters: FATAL ERROR on
  every load ('WinEpicDLCManager' no longer exists after the 2026-11-25 game
  update). These unlock paid DLC and are not patched here; delete them.
- Radio Outkill (assets/mod_overrides): about 45 .ogg files are missing.
  Reinstall it.
- Maps/Proving Grounds: hooks/ArcadeVehicleBase.lua and ArcadeVehicleCamera.lua
  cannot be read. Reinstall the map.
- "Missing meta in provider 'meta_file'" for Low Violence Mode, Difficulty Screen
  Animation, Simple Mod Updater, Celer, Drag and Drop Inventory, Less Inaccurate
  Weapon Laser: their update settings are incomplete. Harmless.
- Chat Translator: the crash from 01:09-01:20 is already fixed in the copy
  installed at 17:43.

None of these patches has been tested in game yet.
