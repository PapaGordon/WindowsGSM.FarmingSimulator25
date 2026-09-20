<p align="center">
  <img src="FarmingSimulator25.cs/FarmingSimulator25.png" alt="Farming Simulator 25" width="128">
</p>

<h1 align="center">WindowsGSM.FarmingSimulator25</h1>

<p align="center">
  MeFriendos build for running a Farming Simulator 25 dedicated server with WindowsGSM.
</p>

<p align="center">
  <a href="https://github.com/Raziel7893/WindowsGSM/releases/tag/v1.25.2.1"><img src="https://img.shields.io/badge/WindowsGSM-Raziel%20v1.25.2.1-38CDD4" alt="Raziel WindowsGSM v1.25.2.1"></a>
  <a href="CHANGELOG.md"><img src="https://img.shields.io/badge/version-0.1.0-80B918" alt="Version 0.1.0"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="MIT License"></a>
</p>

This plugin adds Farming Simulator 25 dedicated-server support to WindowsGSM. It handles installation, updates, startup and shutdown, while the actual server settings stay in the official GIANTS web panel.

FS25 uses the normal licensed Steam game files, so installation needs a Steam account that owns Farming Simulator 25.


## Features

- Installs and updates Farming Simulator 25 through SteamCMD.
- Uses Steam App ID `2300320`.
- Uses a normal Steam account instead of anonymous SteamCMD login.
- Starts the official `dedicatedServer.exe` server manager.
- Checks that `x64\FarmingSimulator2025Game.exe` and `dedicatedServer.xml` exist before startup.
- Creates `steam_appid.txt` with the correct App ID when needed.
- Supports the WindowsGSM Embedded Console using the live state Raziel passes through `AllowsEmbedConsole`.
- Handles a missing Steam account or failed SteamCMD update cleanly.
- Sends CTRL+C first for a clean shutdown before using fallback methods.
- Removes WindowsGSM's broad automatic firewall application exception for the exact `dedicatedServer.exe` path.
- Leaves targeted manual firewall rules untouched.
- Leaves the GIANTS web-panel configuration alone.

## Quick overview

| Setting | Value |
| --- | --- |
| Steam App ID | `2300320` |
| SteamCMD login | Licensed Steam account required |
| Start executable | `dedicatedServer.exe` |
| Game executable | `x64\FarmingSimulator2025Game.exe` |
| Default game port | `10823` |
| Port increment | `3` |
| GIANTS multiplayer ports | `10823-10825/UDP` |
| Web panel | Read from `dedicatedServer.xml` |
| Default max players | `16` |
| Query method | None in WindowsGSM |
| Embedded Console | Supported |
| Server configuration | GIANTS web panel |
| Firewall | Manual port rules only |

## Raziel WindowsGSM compatibility

This plugin works with **Raziel7893/WindowsGSM v1.25.2.1** and uses the fork's current plugin API, including **Set Account**, Steam Guard handling and the live **Embed Console** state.

It also removes the broad WindowsGSM application firewall exception for `dedicatedServer.exe` before startup, while leaving your own port-specific rules alone.

## Requirements

- [Raziel7893/WindowsGSM v1.25.2.1](https://github.com/Raziel7893/WindowsGSM/releases/tag/v1.25.2.1) or a compatible WindowsGSM build
- 64-bit Windows
- Administrator rights for WindowsGSM when the firewall safety check needs to remove a broad application exception
- A Steam account with its own Farming Simulator 25 license

Raziel's current WindowsGSM builds require the **.NET 8 Desktop Runtime** when moving from the original WindowsGSM build.

The dedicated server uses the normal FS25 game installation. If another machine should play at the same time, use a separate Steam account/license for the server.

## Plugin installation

1. Download the latest release archive.
2. Extract the complete `FarmingSimulator25.cs` folder into `<WindowsGSM>\plugins\`.
3. Click **Reload Plugins** or restart WindowsGSM.
4. Add **Farming Simulator 25 Dedicated Server**.
5. In the install window use **Set Account** and sign in with a Steam account that owns Farming Simulator 25.
6. Run **Install**.
7. Start the server manager.
8. Open the web panel address shown by `dedicatedServer.exe`.
9. Finish the actual game-server setup in the GIANTS web panel.
10. Create only the firewall rules the server really needs.

Steam Guard can ask for a code during installation or updates because FS25 cannot be installed anonymously through SteamCMD.

## Steam account and installation

Farming Simulator 25 is Steam App ID:

```text
2300320
```

The Windows build contains both the game and the GIANTS dedicated-server manager. The plugin therefore uses the Steam account configured through **Set Account** instead of anonymous SteamCMD login.

It also makes sure this file exists beside `dedicatedServer.exe`:

```text
steam_appid.txt
```

with:

```text
2300320
```

This keeps Steam's launch confirmation from getting in the way of unattended server starts. It does not bypass game ownership.

## Server configuration

FS25 keeps the actual dedicated-server settings in the GIANTS web panel.

The plugin leaves `dedicatedServer.xml` alone, so changes made in the official panel stay there. WindowsGSM handles installation, updates and the server process; use the GIANTS web panel for the live server settings.


## Ports and firewall

GIANTS currently lists these UDP multiplayer ports:

```text
10823
10824
10825
```

The plugin uses `10823` as the WindowsGSM base port and a port increment of `3` so multiple instances do not start on top of the same default range.

The GIANTS web-admin port is configured in `dedicatedServer.xml`. Do not assume it is always the same on every installation.

The plugin removes the unrestricted WindowsGSM application exception for the exact `dedicatedServer.exe` path before startup. It does **not** create port rules automatically.

For the MeFriendos setup:

- Allow the FS25 multiplayer ports required by the current GIANTS configuration.
- Keep the web panel on localhost, LAN or WireGuard/VPN access.
- Do not expose the admin web panel publicly unless there is a specific reason to do so.
- Adjust manual firewall rules if the GIANTS configuration changes.

Existing manual firewall rules are left untouched.

## Embedded Console and shutdown

When **Embed Console** is enabled, stdout and stderr from `dedicatedServer.exe` are redirected into WindowsGSM and the native console window stays hidden.

When stopping the server, the plugin uses this order:

1. Send CTRL+C and wait up to 30 seconds.
2. Try to close the native server-manager window.
3. Use `Process.Kill()` only if the normal shutdown methods failed.

The forced kill is intentionally the last fallback.

## Updating Farming Simulator 25

1. Stop the server.
2. Back up the savegame, mods and server configuration.
3. Click **Update** in WindowsGSM.
4. Complete Steam Guard authentication if Steam asks for it.
5. Start the server manager again.
6. Check the GIANTS web panel and server log before opening the server to players.

The plugin does not intentionally change savegames, mods or GIANTS web-panel settings during an update.

## Troubleshooting

### WindowsGSM asks for a Steam account

That is expected. FS25 uses the normal licensed game app. Use **Set Account** with a Steam account that owns Farming Simulator 25.

### Steam Guard asks for a code

That is normal for the licensed Steam account used by SteamCMD. Complete the authentication and let WindowsGSM continue.

### Startup says dedicatedServer.exe is missing

Run **Update** with validation or reinstall the server files. The plugin will not start an incomplete FS25 installation.

### Startup says FarmingSimulator2025Game.exe is missing

The normal game installation is incomplete. Validate the installation through WindowsGSM/SteamCMD.

### dedicatedServer.xml is missing

Validate the FS25 installation. The plugin expects the normal GIANTS dedicated-server files to be present.

### Players cannot connect

Check the game port configured in the GIANTS web panel and confirm the required UDP ports are allowed by narrow firewall rules and forwarded where needed.

### The web panel is not reachable remotely

This can be intentional. For the MeFriendos setup, administer it through LAN or WireGuard/VPN instead of publishing the web panel to the internet.

### WindowsGSM reports that automatic firewall access could not be disabled

Run WindowsGSM as administrator. If an unrestricted `dedicatedServer.exe` application rule already exists, remove it manually and keep only the intended port-specific rules.

## Project links

- Source: [PapaGordon/WindowsGSM.FarmingSimulator25](https://github.com/PapaGordon/WindowsGSM.FarmingSimulator25)
- WindowsGSM: [Raziel7893/WindowsGSM](https://github.com/Raziel7893/WindowsGSM)
- Current WindowsGSM release: [v1.25.2.1](https://github.com/Raziel7893/WindowsGSM/releases/tag/v1.25.2.1)
- Farming Simulator: [farming-simulator.com](https://www.farming-simulator.com/)
- Community: [mefriendos.de](https://mefriendos.de)

This is an independent community plugin. It is not affiliated with or endorsed by GIANTS Software, Valve or WindowsGSM.

## License

This MeFriendos plugin is released under the [MIT License](LICENSE).
