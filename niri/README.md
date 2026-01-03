# niri — CachyOS / Arch-based installation guide 🔧

A short guide to get the `niri` Wayland session/config running on CachyOS (Arch-based). This README lists the programs and services this config references and provides example installation and setup steps for CachyOS.

---

## What this repo contains

- `config.kdl` — main `niri` configuration (keybindings, inputs, outputs, startup apps)
- `noctalia.kdl` — additional theme/layout includes

> This README documents the programs referenced by the config and how to install them on CachyOS.

---

## Supported OS

- CachyOS (Arch-based). Commands in this guide use `pacman` and an AUR helper (e.g., `paru` or `yay`) when needed.

---

## Required and recommended programs (minimal list)

Install the following to fully match the configuration in `config.kdl`:

- `niri` — compositor/session (if packaged or install from source; provide your packaged binary or build instructions)
- `alacritty` — terminal emulator
- `firefox` — web browser
- `swaylock` (or equivalent) — screen locker used by keybindings
- `nautilus` — file manager (optional; used by keybinding)
- `fuzzel` — launcher (may be in AUR)
- `playerctl` — media key support
- `brightnessctl` — brightness control
- `pipewire` + `wireplumber` — audio stack (for `wpctl` usage)
- `xdg-desktop-portal` (and/or `xdg-desktop-portal-wlr`) — desktop portal support
- `swww` — wallpaper daemon (optional; used in commented startup examples)
- `polkit` and a polkit agent (e.g., `polkit-gnome` or `polkit-kde-agent`) — authentication agent
- `xorg-xwayland` — run X11 applications under Wayland

Notes:
- Some packages (e.g., `fuzzel`, `swww`, or `noctalia` related tools) may be available in the AUR. Use `paru` or `yay` to install them.
- If `niri` or `noctalia-shell` is part of this repository or a private build, build and install those first. See [noctalia-shell](https://github.com/noctalia-dev/noctalia-shell) for upstream sources.

---

## Example installation commands (CachyOS / Arch)

System update:

```bash
sudo pacman -Syu
```

Install common packages from official repos:

```bash
sudo pacman -S alacritty firefox nautilus playerctl brightnessctl pipewire wireplumber xdg-desktop-portal xorg-xwayland polkit
```

Install AUR packages with an AUR helper (example: `paru`):

```bash
paru -S fuzzel swww # and any other AUR-only packages like noctalia/noctalia-shell
```

Enable and start PipeWire / WirePlumber for audio:

```bash
systemctl --user enable --now pipewire pipewire-pulse wireplumber
```

(If using a polkit agent that requires a system unit or autostart, enable it per its instructions.)

---

## Configuration and file placement

- Copy `config.kdl` and `noctalia.kdl` into your Wayland config directory for `niri` (commonly `~/.config/niri/`).

```bash
mkdir -p ~/.config/niri
cp config.kdl noctalia.kdl ~/.config/niri/
```

- Ensure the `include` path inside `config.kdl` points to `./noctalia.kdl` (it already does in this repo).

- If `niri` requires a desktop session file, create `/usr/share/wayland-sessions/niri.desktop` so you can select it in your display manager.

---

## Session start / Display manager

- If you use a display manager (SDDM/LightDM/etc.), add a session file so you can pick the `niri` session.
- If you use TTY login, follow the compositor's recommended way to start a Wayland session (check `niri` docs).

---

## Troubleshooting & tips

- If audio control commands like `wpctl` are missing, make sure `pipewire` and `wireplumber` are installed and running.
- For brightness keys to work, ensure `brightnessctl` is installed and you have the right permissions (uaccess or polkit rules).
- If some binaries referenced in `config.kdl` (e.g., `noctalia-shell` or `qs`) are missing, either install those packages or update the `spawn-at-startup` lines to use your preferred tools.
- To debug keybindings and startup apps, enable logs for `niri` (see the compositor's logging docs) and check `journalctl --user -xe`.

---

## Optional tools (recommended)

- `xdg-utils` / `xdg-desktop-portal` / `xdg-desktop-portal-gtk` — for integration with portals
- `waybar` or other status bar if you want a status bar
- `grim` / `slurp` / `swappy` — for screenshots and annotation

---

## Demo 🎬

<p align="center">
  <video src="../walpaper/demo.mp4" controls width="720">
    Your browser does not support HTML5 video. <a href="../walpaper/demo.mp4">Download the demo (MP4)</a>.
  </video>
</p>

> The demo video is included at `walpaper/demo.mp4` in this repository.

---

