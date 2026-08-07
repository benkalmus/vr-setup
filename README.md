# VR Setup

## System
- GPU: AMD Radeon RX 7900 XTX (RADV, Mesa 26.1.5)
- Network: Wired 1Gbps (192.168.100.17)
- Headset: Meta Quest 3 (192.168.100.19)
- Firewall: UFW (ports 9943-9944 tcp/udp allowed)

## ALVR
- v21.0.0-dev13 (nightly) installed at `~/.local/share/ALVR-Launcher/installations/v21.0.0-dev13+nightly.2026.06.06/`
- v20.14.1 (stable) also installed
- Quest 3 client APK sideloaded via adb
- Config: `~/.config/alvr/session.json`
- **SteamVR must be on "previous" branch** (2.16+ causes desync: [ALVR #3297](https://github.com/alvr-org/ALVR/issues/3297), [#3326](https://github.com/alvr-org/ALVR/issues/3326))
- UFW firewall script exit code 2 is false positive (rules already exist)

### SteamVR Linux async compute + setcap (IMPORTANT)
- Error **307 "a key component of SteamVR isn't working"** on AMD/Linux: enabling async reprojection requires creating a high-priority Vulkan queue, which needs the `CAP_SYS_NICE` capability. Without it SteamVR's compositor crash-loops → error 307.
- The async-compute fix for Linux/AMD microstutter/ghosting ([ALVR #2537](https://github.com/alvr-org/ALVR/issues/2537)) needs BOTH:
    - `steamvr.vrsettings`: `"disableAsync": false`, `"enableLinuxVulkanAsync": true`, `"motionSmoothing": false` (SteamVR may rewrite `disableAsync` back to `true` on its own — re-check if async stops working)
    - ALVR `session.json`: `linux_async_compute: true` in both `openvr_config` and `extra.patches`
- Then grant the capability (otherwise SteamVR 307s on next start):
    ```
    sudo setcap cap_sys_nice=eip ~/.local/share/Steam/steamapps/common/SteamVR/bin/linux64/vrcompositor-launcher
    ```
- setcap is a **one-time filesystem capability** (persists across reboots) BUT **SteamVR self-updates overwrite `vrcompositor-launcher` and wipe the cap** — **re-run the command after every SteamVR update**.
- Verify current cap: `getcap .../bin/linux64/vrcompositor-launcher` (expect `cap_sys_nice=eip`).
- Status 2026-08-05: async enabled + setcap in place on SteamVR March release; still investigating controller/translation "staircase" stutter.

## WiVRn
- v26.6.2 (AUR: wivrn-server, wivrn-dashboard)
- xrizer-git for OpenVR→OpenXR translation
- WayVR 26.7.1-2 for desktop overlay
- [docs](https://github.com/WiVRn/WiVRn/blob/master/docs/configuration.md)
- Config read from: 
    - `$XDG_CONFIG_HOME/wivrn/config.json`
    - `/usr/share/wivrn/config.json`
    - https://github.com/WiVRn/WiVRn/blob/master/docs/configuration.md

- No SteamVR needed. Uses OpenXR directly.
- Encoder: vaapi (AMD), codec: auto (negotiates AV1 if available)
- Quest 3 client: install from Meta Store or APK

### Steam Launch Options
```
PRESSURE_VESSEL_IMPORT_OPENXR_1_RUNTIMES=1 %command%
```
Add to each VR game in Steam → Properties → Launch Options.

### Known Issues
- Fallout 4 VR: needs OpenComposite for controller fix, WiVRn 26.6 has ReShade crash ([#977](https://github.com/WiVRn/WiVRn/issues/977))
- 32-bit games (HL2VR): need 32-bit xrizer (not in Flatpak)
- Some games have input mapping quirks via xrizer (Blade & Sorcery)

## GameMode
CPU governor switching for VR games (WiVRn + ALVR). Sets `desiredgov=performance` while a game runs, restores the previous governor on exit. LACTd handles GPU settings.

### Install
```
sudo pacman -S gamemode lib32-gamemode
sudo usermod -aG gamemode $(whoami)
```
**Re-login** — the gamemode group must be present in the session for the polkit rule to allow governor changes.

### Config
Repo file: `gamemode/gamemode.ini` (edit here), symlinked into the live config:
```
ln -snf ../projects/vr-setup/gamemode/gamemode.ini ~/.config/gamemode.ini
```

### Usage
Add `gamemoderun` to each VR game's Steam Launch Options:
```
gamemoderun PRESSURE_VESSEL_IMPORT_OPENXR_1_RUNTIMES=1 %command%
```
Non-Steam launchers (e.g. BSManager): see `games/beat-saber.md`.

### Verify
```
systemctl --user restart gamemoded && gamemoded -t
```
Then `gamemoded -s` while a game runs (expect `active`). Governor during play: `cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor` → `performance`.

### Useful Links
- [WiVRn GitHub](https://github.com/WiVRn/WiVRn)
- [VR on Linux DB](https://db.vronlinux.org/)
- [ProtonDB](https://www.protondb.com/)
- [WayVR](https://github.com/wayvr-org/wayvr)

### Global OpenXR env var
Added to shell configs so all Proton games use WiVRn/OpenXR:
- `~/.config/fish/config.fish`: `set -gx PRESSURE_VESSEL_IMPORT_OPENXR_1_RUNTIMES 1`
- `~/.zshrc`: `export PRESSURE_VESSEL_IMPORT_OPENXR_1_RUNTIMES=1`

Launch Steam from terminal so it inherits the env var.
Alternatively, `~/.local/share/applications/steam.desktop` has `Exec=` prefixed with `env PRESSURE_VESSEL_IMPORT_OPENXR_1_RUNTIMES=1` so KDE desktop shortcut also works.
### WayVR capture fix
Set `capture_method: "Screen"` in `~/.config/wayvr/conf.d/zz-saved-config.json5` (was `"Auto"`). Auto mode on KDE picks the active window instead of full desktop. KDE screen selection dialog may be invisible in VR — accept it on the physical monitor.
### Audio mirroring
PipeWire `module-combine-stream` creates virtual sink `vr_mirror` that forwards to both `wivrn.sink` and analog stereo speakers. Config at `~/.config/pipewire/pipewire.conf.d/10-vr-mirror.conf`. Handles headset connect/disconnect automatically via `combine.on-demand-streams = true`.
### VR spectator view
WiVRn has no built-in spectator view (issue #404). Enable `debug-gui: true` in server.json for Monado debug window showing one-eye readback. May require rebuilding with `-DWIVRN_FEATURE_DEBUG_GUI=ON` if AUR package doesn't include it.
