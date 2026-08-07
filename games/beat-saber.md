# Beat Saber via BSManager (WiVRn + gamemode)

Beat Saber (Steam appid 620980) launched through BSManager instead of Steam.
BSManager uses its own Proton prefix and runs the game directly, so Steam launch
options don't apply — configure everything in BSManager instead.

## BSManager Advanced Launch arguments

Game version → Advanced Launch → custom arguments:

```
gamemoderun PRESSURE_VESSEL_IMPORT_OPENXR_1_RUNTIMES=1 PRESSURE_VESSEL_FILESYSTEMS_RW=$XDG_RUNTIME_DIR/wivrn/comp_ipc %command%
```

- `gamemoderun` activates the GameMode CPU governor tweak (see `gamemode/gamemode.ini`).
- `PRESSURE_VESSEL_IMPORT_OPENXR_1_RUNTIMES=1` passes the WiVRn OpenXR manifest through the pressure-vessel sandbox.
- `PRESSURE_VESSEL_FILESYSTEMS_RW=$XDG_RUNTIME_DIR/wivrn/comp_ipc` mounts WiVRn's compositor socket so the game can connect to wivrn-server. Try without it first (Steam-launched games work with just the import var); add only if the game can't reach the runtime.
- Debug-only extras: `PROTON_LOG=1 WINEDEBUG=vrclient,openxr,steam`.

## Steam detection / overlay

BSManager already injects the Steam appid env vars into the Proton launch, so no
extra setup is needed for the Steam overlay:

```
SteamAppId=620980  SteamOverlayGameId=620980  SteamGameId=620980
STEAM_COMPAT_APP_ID=620980  SteamEnv=1
```

This attaches the Steam overlay and associates the process with appid 620980.
Caveat: the Steam client may not show "currently playing" rich presence for an
externally launched game — that only happens when Steam launches the game itself.

## Flatpak caveat

Older Flatpak builds of BSManager dropped user-added env vars (bug #1015),
fixed in #1016. Native installs are unaffected.

## Useful mods (BSManager built-in)

- **Skip Steam**: don't auto-open Steam; use with the WiVRn/OpenXR runtime.

## Reference

- BSManager: https://github.com/Zagrios/bs-manager
- WiVRn Steam/pressure-vessel notes: https://github.com/WiVRn/WiVRn/blob/master/docs/steamvr.md
