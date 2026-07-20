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

## WiVRn
- v26.6.2 (AUR: wivrn-server, wivrn-dashboard)
- xrizer-git for OpenVR→OpenXR translation
- WayVR 26.7.1-2 for desktop overlay
- Config: `~/.config/wivrn/server.json`
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
