# Package and Service Inventory

## Purpose

This document records a sanitized overview of installed packages, AUR packages, key software versions, enabled services, and selected running services.

Raw package and service outputs are stored locally under private/raw/ and must not be published.

## Package Summary

| Item | Count |
|---|---:|
| Explicit packages | 78 |
| Official repository explicit packages | 67 |
| Foreign / AUR packages | 11 |

## Foreign / AUR Packages

| Package | Purpose / Notes | Assessment |
|---|---|---|
| eww | Desktop widgets | Optional desktop customization |
| sddm-astronaut-theme | SDDM theme | Cosmetic |
| sddm-sugar-candy-git | SDDM theme | Cosmetic |
| spotify | Music application | User application |
| spotify-adblock | Spotify modification | User application, review if needed |
| tokyonight-gtk-theme-git | GTK theme | Cosmetic |
| tor-browser-alpha-bin | Tor Browser alpha build | Review: alpha software may be less stable |
| visual-studio-code-bin | Code editor | Development tool |
| wireguard-dkms | WireGuard kernel module via DKMS | Review whether required with current kernel |
| yay | AUR helper | Required for AUR workflow |
| yay-debug | Debug package | Review whether needed |

## Key Package Versions

| Package | Version |
|---|---|
| linux | 7.0.9.arch2-1 |
| linux-firmware | 20260519-1 |
| NetworkManager | 1.56.1-1 |
| ufw | 0.36.2-7 |
| fail2ban | 1.1.0-8 |
| wireguard-tools | 1.0.20260223-1 |
| docker | 1:29.5.1-1 |
| containerd | 2.3.1-1 |
| openssh | 10.3p1-1 |
| hyprland | 0.55.2-1 |
| waybar | 0.15.0-2 |
| kitty | 0.46.2-1 |
| rofi | 2.0.0-1 |
| zsh | 5.9-6 |
| starship | 1.25.1-1 |
| neovim | 0.12.2-1 |

## Enabled Services Reviewed

| Service | State | Notes |
|---|---|---|
| NetworkManager.service | enabled | Required for network management |
| NetworkManager-dispatcher.service | enabled | Used for NetworkManager dispatcher scripts |
| NetworkManager-wait-online.service | enabled | May delay boot; review if needed |
| ufw.service | enabled | Required for firewall baseline |
| fail2ban.service | enabled | Required for intrusion prevention baseline |
| docker.service | enabled | Useful for lab/dev work; review whether it should always start at boot |

## Running Services Reviewed

| Service | Status | Assessment |
|---|---|---|
| NetworkManager.service | running | Expected |
| ufw.service | active through firewall status | Expected |
| fail2ban.service | running | Expected |
| docker.service | running | Expected if Docker is actively used |
| containerd.service | running | Expected when Docker is active |
| sddm.service | running | Expected graphical login manager |
| polkit.service | running | Expected desktop authorization service |
| systemd-udevd.service | running | Expected device manager |
| systemd-userdbd.service | running | Expected systemd user database service |
| user@1000.service | running | Expected user session service |

## Docker Assessment

Docker and containerd are installed and running.

This is expected for a workstation used for infrastructure labs, development, and container-based projects.

However, Docker increases the local system attack surface and should be reviewed as an always-on service.

Current recommendation:

- keep Docker installed
- keep Docker documented
- consider disabling Docker autostart later if it is not needed every boot
- start Docker manually when working on container projects, if a stricter workstation profile is desired

## AUR Assessment

The system uses AUR packages.

AUR usage is acceptable for a personal Arch workstation, but AUR packages should be treated as higher-trust-requirement software because they are not official repository packages.

Current recommendations:

1. Keep AUR package count low.
2. Review PKGBUILDs before installing new AUR packages.
3. Avoid unnecessary debug packages.
4. Prefer stable packages over alpha packages when possible.
5. Periodically review foreign packages with pacman -Qqem.

## Current Assessment

The package set is relatively small and appropriate for a Hyprland-based Arch workstation.

The main follow-up items are:

1. Decide whether Docker should remain enabled at boot.
2. Review whether yay-debug is needed.
3. Review whether tor-browser-alpha-bin should be replaced by a stable Tor Browser package.
4. Review whether wireguard-dkms is necessary.
5. Review whether NetworkManager-wait-online.service is needed.
