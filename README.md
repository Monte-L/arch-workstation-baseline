# Arch Linux Secure Workstation Baseline

This repository documents a privacy-oriented Arch Linux workstation baseline focused on defensive configuration, service review, firewall validation, VPN/DNS behavior, sanitized dotfiles, backup planning, and operational documentation.

The project is based on a real personal workstation and was organized as a public portfolio project. Sensitive files, private keys, VPN configuration files, tokens, passwords, local caches, and raw private evidence are intentionally excluded.

## Project Goals

- Document a clean Arch Linux workstation baseline
- Review running services and reduce unnecessary exposure
- Validate firewall behavior with UFW
- Review SSH exposure and Fail2ban readiness
- Validate VPN routing and DNS behavior
- Plan and test VPN leak protection / kill switch behavior
- Separate public dotfiles from private application data
- Document backup and restore strategy
- Preserve a reproducible technical reference for future rebuilds
- Present the project as a defensive Linux/security portfolio artifact

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
| DNS | Quad9 policy reviewed and validated |

## Current Operating Model

The current validated model is:

```text
Wi-Fi connects automatically
   ↓
VPN starts automatically through NetworkManager
   ↓
UFW blocks ordinary outbound traffic outside the VPN
   ↓
DNS uses Quad9
   ↓
Rollback remains available through local helper scripts
```

## Security Baseline Highlights

### Firewall

UFW is active with a default-deny incoming posture. No explicit inbound allow rules remain in the current baseline.

### SSH

The SSH daemon is disabled and inactive. SSH is not exposed through the firewall.

If SSH is enabled in the future, it should use key-based authentication, disabled root login, restricted firewall source rules, and Fail2ban protection.

### Fail2ban

Fail2ban is enabled and active. The SSH jail is prepared for future SSH hardening, although SSH is not currently exposed.

### Docker

Docker remains installed for lab and development work, but it is disabled from automatic startup. Docker and containerd can be started manually only when needed.

### VPN and DNS

VPN routing and DNS behavior were reviewed across VPN-on and VPN-off states. The project documents a NetworkManager-managed WireGuard direction, Quad9 DNS behavior, and VPN leak-protection planning.

## Completed Work

- Sanitized public dotfiles
- System and service inventory
- Firewall baseline with UFW
- Fail2ban validation
- SSH exposure review
- Docker autostart review
- Network and routing baseline
- VPN/DNS behavior review
- NetworkManager VPN autoconnect validation
- Persistent VPN kill switch testing
- External DNS leak testing
- Post-reboot validation
- USB backup test
- Restic encrypted backup test
- Selected-directory restore testing
- Emergency firewall rollback helper planning

## Dotfiles

The repository includes a sanitized subset of workstation configuration.

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
- full home directory dumps
- full `~/.config` dumps

## Documentation Map

Key documents:

| Document | Purpose |
|---|---|
| `docs/system-inventory.md` | Sanitized system and hardware inventory |
| `docs/security-baseline.md` | Workstation security posture |
| `docs/network-baseline.md` | Network interface and routing baseline |
| `docs/vpn-dns-baseline.md` | VPN routing and DNS baseline |
| `docs/firewall-fail2ban.md` | Firewall, listening services, SSH, and Fail2ban review |
| `docs/package-service-inventory.md` | Package, AUR, service, and Docker review |
| `docs/enabled-services-review.md` | Enabled systemd services review |
| `docs/dotfiles-guide.md` | Dotfiles review and sanitization notes |
| `docs/risk-register.md` | Security and operational risk register |
| `docs/vpn-kill-switch-plan.md` | VPN kill switch and leak protection planning |
| `docs/persistent-kill-switch-validation.md` | Persistent VPN kill switch validation |
| `docs/vpn-autoconnect-validation.md` | NetworkManager VPN autoconnect validation |
| `docs/backup-restore-strategy.md` | Backup and restore strategy |
| `docs/restic-encrypted-backup-test.md` | Encrypted backup and restore test |
| `docs/restore-checklist.md` | Workstation restore checklist |

## Repository Safety

This repository intentionally excludes local raw evidence and sensitive configuration.

The `.gitignore` is designed to exclude:

```text
private/
*.key
*.pem
*.secret
*.token
.env
*.conf with sensitive VPN data
backup artifacts
temporary files
```

Before public updates, the repository should be checked with:

```bash
git status
git ls-files
```

and reviewed for sensitive terms such as private keys, tokens, passwords, VPN configuration files, and raw personal command outputs.

## Skills Demonstrated

- Arch Linux workstation administration
- Linux service inventory and review
- UFW firewall validation
- Fail2ban validation
- SSH exposure reduction
- VPN routing and DNS behavior review
- NetworkManager VPN autoconnect planning
- VPN kill switch testing and rollback planning
- Dotfile sanitization
- Backup and restore testing
- Restic encrypted backup planning
- Technical documentation
- Defensive security mindset
- Operational risk tracking

## Project Status

The current baseline phase is complete.

The workstation has a documented defensive operating model with automatic Wi-Fi connection, NetworkManager VPN autoconnect, UFW-based outbound protection outside the VPN, Quad9 DNS policy, rollback helpers, sanitized dotfiles, and backup/restore validation.

## Future Work

Future work is optional and should be treated as advanced hardening or backup expansion.

Possible next steps:

- Expand backup automation
- Improve restore drills
- Review AppArmor or other mandatory access control options
- Add auditd-style logging experiments
- Further separate public documentation from private operational notes
- Revisit disk encryption strategy during a future reinstall
- Continue testing firewall-based VPN leak protection under more network scenarios

## Portfolio Summary

This project demonstrates the ability to review, document, and improve a real Linux workstation from a defensive infrastructure perspective. It shows practical skills in Linux administration, firewall validation, VPN/DNS review, service minimization, dotfile hygiene, backup planning, and public technical documentation.
