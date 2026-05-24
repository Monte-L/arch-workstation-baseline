# Arch Linux Secure Workstation Baseline

## Overview

This project documents the configuration, security baseline, dotfiles, and operational setup of a personal Arch Linux workstation.

The goal is to build a clean, reproducible, and well-documented Linux workstation environment focused on:

- system transparency
- secure defaults
- network awareness
- firewall and service validation
- VPN and DNS verification
- public dotfile organization
- private data separation
- practical infrastructure documentation

This is not intended to be a fully hardened enterprise workstation.

It is a personal learning and portfolio project focused on understanding, documenting, and improving a real Linux environment step by step.

## Current Environment

- Operating System: Arch Linux
- Window Manager: Hyprland
- Terminal: Kitty
- Application Launcher: Rofi
- Status Bar: Waybar
- Firewall: UFW
- VPN: WireGuard with ProtonVPN
- DNS: Quad9
- Intrusion Prevention: Fail2ban

## Project Goals

1. Document the current system state.
2. Separate public dotfiles from private application data.
3. Validate firewall rules and exposed services.
4. Verify VPN routing and DNS behavior.
5. Review SSH security and key handling.
6. Document Fail2ban configuration and protected services.
7. Identify remaining risks in a risk register.
8. Build a safe public version of the configuration for portfolio use.

## Scope

### In Scope

- Arch Linux workstation documentation
- Hyprland-related dotfiles
- Shell and terminal configuration
- Firewall baseline
- VPN and DNS validation
- Fail2ban review
- Security checklist
- Risk register
- Backup planning

### Out of Scope

- Publishing private VPN configuration
- Publishing secrets, tokens, keys, or passwords
- Advanced enterprise hardening
- Disk encryption migration
- AppArmor or auditd deployment at this stage

## Documentation Structure

- docs/system-inventory.md
- docs/security-baseline.md
- docs/network-baseline.md
- docs/vpn-dns-baseline.md
- docs/dns-behavior-review.md
- docs/post-reboot-validation.md
- docs/firewall-fail2ban.md
- docs/package-service-inventory.md
- docs/enabled-services-review.md
- docs/dotfiles-guide.md
- docs/risk-register.md
- docs/project-log.md

## Current Status

Project initialized.

Next step: collect the first system inventory and document the current workstation state.

## Current Baseline Status

The current workstation baseline includes:

| Area | Status |
|---|---|
| System inventory | Documented |
| Storage and graphics | Documented |
| Network baseline | Documented |
| VPN routing | Validated through policy routing |
| DNS behavior | Documented for VPN-on and VPN-off states |
| Firewall | Active with default deny incoming |
| Inbound firewall rules | None |
| SSH service | Disabled and inactive |
| Fail2ban | Enabled and active |
| Docker | Installed but disabled from autostart |
| containerd | Disabled and inactive during normal use |
| Dotfiles | Selected and sanitized |
| Post-reboot validation | Completed |
| Public repository review | In progress |

## Security Notes

This repository intentionally excludes private raw command outputs and sensitive configuration files.

The private/ directory is used only for local evidence collection and must not be uploaded.

VPN configuration files, SSH keys, tokens, passwords, endpoints, and raw network data are excluded from the public repository.

## Future Hardening Work

Planned future work includes:

1. Replace temporary VPN startup workaround with a cleaner architecture.
2. Evaluate NetworkManager-managed WireGuard profile.
3. Define a clear DNS policy.
4. Evaluate VPN kill switch / leak protection.
5. Perform external DNS leak testing.
6. Build backup and restore strategy.
7. Review SSH hardening if remote access is needed later.
