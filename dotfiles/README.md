# Dotfiles

This directory contains a sanitized subset of the workstation dotfiles.

The goal is to document the public, reproducible parts of the desktop environment without exposing private application data or machine-specific secrets.

## Included Components

| Component | Purpose |
|---|---|
| Hyprland | Wayland compositor configuration |
| Hyprpaper | Wallpaper configuration with sanitized placeholder path |
| Waybar | Status bar configuration |
| Kitty | Terminal configuration |
| Rofi | Application launcher configuration and theme |
| Starship | Shell prompt configuration |
| Neovim | Editor configuration |
| Fastfetch | System information display, if configured |
| Shell | Sanitized Zsh configuration |

## Not Included

The following items are intentionally excluded:

- VPN configuration files
- WireGuard profiles
- SSH keys
- browser data
- application caches
- private tokens
- passwords
- full ~/.config dump
- personal wallpapers
- machine-specific secrets

## Notes

The public shell configuration includes a safe PATH baseline to prevent command lookup issues.

VPN aliases are sanitized and use a generic placeholder profile name.
