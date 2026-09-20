# Changelog

## 0.1.0

Initial Farming Simulator 25 support.

- Added SteamCMD install and update support for App ID `2300320` using a licensed Steam account.
- Added support for the plugin API used by Raziel7893/WindowsGSM v1.25.2.1.
- Added startup for the official `dedicatedServer.exe` manager.
- Added checks for `FarmingSimulator2025Game.exe` and `dedicatedServer.xml`.
- Added WindowsGSM Embedded Console output using Raziel's live `AllowsEmbedConsole` state.
- Added clean CTRL+C shutdown with sensible fallbacks.
- Added a guarded SteamCMD update path.
- Added `steam_appid.txt` creation for unattended server starts.
- Disabled the broad automatic WindowsGSM firewall exception for the server manager.
- Added setup and firewall notes.
