# Arch Linux Secure Workstation Baseline

## Overview

This project documents the configuration, security baseline, service review, VPN/DNS behavior, and sanitized dotfiles of a personal Arch Linux workstation.

The goal is to build a clean, reproducible, and well-documented Linux workstation environment focused on:

- system transparency
- secure defaults
- firewall validation
- VPN and DNS review
- service minimization
- dotfile organization
- private data separation
- practical infrastructure documentation

This is not intended to be a fully hardened enterprise workstation.

It is a personal learning and portfolio project focused on understanding, documenting, and improving a real Linux environment step by step.

## Environment

| Component | Value |
|---|---|
| Operating System | Arch Linux |
| Device | Lenovo ThinkPad T480 |
| Window Manager | Hyprland |
| Terminal | Kitty |
| Application Launcher | Rofi |
| Status Bar | Waybar |
| Shell | Zsh |
| Firewall | UFW |
| Intrusion Prevention | Fail2ban |
| VPN | WireGuard-based ProtonVPN setup |
| DNS | NetworkManager / resolvconf reviewed, Quad9 observed during VPN-active state |

## Current Baseline Status

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
| Repository safety review | In progress |

## Security Baseline Highlights

### Firewall

UFW is active and enabled.

Current policy:

| Direction | Policy |
|---|---|
| Incoming | Deny |
| Outgoing | Allow |
| Routed | Deny |

No explicit inbound allow rules remain.

Previously open inbound rules for SSH, WireGuard-style traffic, and a development port were removed.

### SSH

The SSH daemon is disabled and inactive.

SSH is not exposed through the firewall.

If SSH is enabled in the future, it should use:

- key-based authentication
- disabled root login
- disabled password login where possible
- restricted firewall source rules
- active Fail2ban protection

### Fail2ban

Fail2ban is enabled and active.

The sshd jail is configured and ready for future SSH hardening, although SSH is not currently exposed.

### Docker

Docker remains installed for infrastructure labs and development work.

However, Docker is no longer started automatically at boot.

Current Docker/container runtime state:

| Service | State |
|---|---|
| docker.service | disabled / inactive |
| docker.socket | disabled / inactive |
| containerd.service | disabled / inactive |

Docker can be started manually when needed for lab work.

### VPN and DNS

VPN routing was validated through policy routing.

The public route decision selected the VPN interface placeholder used in this public repository:

| Item | Result |
|---|---|
| VPN interface placeholder | vpn0 |
| Policy routing table | 51820 |
| VPN-active route | via vpn0 |
| VPN-off route | via Wi-Fi |
| Quad9 with VPN active | detected |
| Quad9 with VPN off | detected after DNS policy update |

DNS behavior differs between VPN-on and VPN-off states.

A temporary VPN startup workaround is documented. The long-term preferred direction is to evaluate a NetworkManager-managed WireGuard profile with clear DNS policy and VPN leak protection.

## Documentation

| Document | Purpose |
|---|---|
| docs/system-inventory.md | Sanitized system and hardware inventory |
| docs/security-baseline.md | Overall workstation security posture |
| docs/network-baseline.md | Network interface and routing baseline |
| docs/vpn-dns-baseline.md | VPN routing and DNS baseline |
| docs/dns-behavior-review.md | VPN-on and VPN-off DNS comparison |
| docs/dns-policy.md | Defined DNS behavior for VPN-on and VPN-off states |
| docs/external-dns-leak-test.md | Sanitized external DNS leak test results |
| docs/firewall-fail2ban.md | Firewall, listening services, SSH, and Fail2ban review |
| docs/package-service-inventory.md | Package, AUR, service, and Docker review |
| docs/enabled-services-review.md | Full enabled systemd services review |
| docs/post-reboot-validation.md | Post-reboot validation results |
| docs/dotfiles-guide.md | Dotfiles review and sanitization notes |
| docs/risk-register.md | Security and operational risk register |
| docs/project-log.md | Project progress log |
| docs/repository-review.md | Pre-publication repository safety review |
| docs/next-phase-plan.md | Future hardening roadmap and next phase plan |
| docs/networkmanager-wireguard-evaluation.md | NetworkManager WireGuard migration evaluation |
| docs/vpn-kill-switch-plan.md | VPN kill switch and leak protection planning |
| docs/vpn-kill-switch-rollback.md | Rollback strategy before kill switch testing |
| docs/vpn-kill-switch-test-design.md | Test-only VPN kill switch design |
| docs/vpn-kill-switch-test-results.md | First manual VPN kill switch test results |
| docs/persistent-kill-switch-validation.md | Persistent VPN kill switch post-reboot validation |
| docs/vpn-autoconnect-validation.md | NetworkManager VPN autoconnect validation |
| docs/local-helper-scripts.md | Local helper scripts used during VPN and firewall testing |
| docs/restore-checklist.md | Workstation restore checklist |
| docs/usb-backup-test.md | First USB backup and small restore test |
| docs/restic-encrypted-backup-test.md | Encrypted USB backup and restore test using Restic |
| docs/restic-backup-policy.md | Restic include/exclude backup policy |
| docs/restic-selected-directory-backup-test.md | Selected-directory encrypted backup test using Restic excludes |
| docs/restic-password-strategy.md | Restic password handling and recovery strategy |
| backup/restic-private-include-template.txt | Public template for private backup include planning |
| docs/private-backup-planning.md | Planning for future private encrypted backups |
| backup/restic-excludes.txt | Restic exclude policy template |
| docs/backup-restore-strategy.md | Backup and restore strategy |
| docs/vpn-kill-switch-facts-summary.md | Sanitized facts for VPN kill switch design |

## Dotfiles

The dotfiles directory contains a sanitized subset of the workstation configuration.

Included components:

| Component | Purpose |
|---|---|
| Hyprland | Wayland compositor configuration |
| Hyprpaper | Wallpaper configuration with placeholder paths |
| Waybar | Status bar configuration |
| Kitty | Terminal configuration |
| Rofi | Application launcher and theme |
| Starship | Shell prompt configuration |
| Neovim | Editor configuration |
| Zsh | Sanitized shell configuration |

The public dotfiles do not include:

- VPN configuration files
- WireGuard private keys
- SSH keys
- browser data
- application caches
- tokens
- passwords
- raw command outputs
- full home directory or full ~/.config dump

## Repository Safety

This repository intentionally excludes local raw evidence and sensitive configuration.

The private/ directory is used only for local collection and must not be uploaded.

The .gitignore file excludes:

- private/
- key files
- token files
- secret files
- environment files
- WireGuard configuration files
- backup and temporary files

Before public upload, the repository should be reviewed with:

- git status
- git ls-files
- critical sensitive pattern scan
- private directory ignore check

## Future Hardening Work

Planned future work includes:

1. Replace the temporary VPN startup workaround with a cleaner architecture.
2. Evaluate a NetworkManager-managed WireGuard profile.
3. Define a clear DNS policy.
4. Evaluate VPN kill switch and leak protection.
5. Perform external DNS leak testing.
6. Build a backup and restore strategy.
7. Review SSH hardening if remote access is needed later.
8. Prepare the repository for public GitHub publication.

## Project Status

Initial workstation baseline completed and published.

The repository has passed local safety review and is available as a public portfolio project.

Current phase: Phase 2B - DNS Policy Definition.

Completed recent work:

- NetworkManager-managed WireGuard evaluation
- Post-reboot NetworkManager WireGuard validation
- Quad9 DNS configured for the active Wi-Fi profile
- Quad9 DNS confirmed with VPN inactive
- Quad9 DNS confirmed with VPN active
- wg-quick / resolvconf workflow retained as fallback

Upcoming work:

1. Validate DNS policy after another reboot.
2. Perform external DNS leak testing.
3. Plan VPN kill switch / leak protection.
4. Decide whether new Wi-Fi/Ethernet profiles should receive automatic DNS policy.
5. Build backup and restore strategy.
6. Review SSH hardening only if remote access becomes necessary.
