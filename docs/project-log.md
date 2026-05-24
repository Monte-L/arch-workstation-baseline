# Project Log

## Day 1 - Foundation and Initial Baseline

### Completed

- Created project structure.
- Created initial README.
- Created private raw collection directory.
- Collected sanitized system inventory.
- Documented hardware, storage, graphics, and operating system baseline.
- Reviewed network configuration.
- Verified NetworkManager DNS handling.
- Tested ProtonVPN WireGuard routing.
- Confirmed VPN policy routing through interface protonch.
- Enabled and validated UFW.
- Removed unnecessary inbound firewall rules.
- Confirmed no explicit inbound allow rules remain.
- Reviewed listening services.
- Identified containerd as localhost-only listener.
- Reviewed Fail2ban status.
- Confirmed Fail2ban is enabled and active.
- Confirmed sshd jail is configured.
- Confirmed sshd service is disabled and inactive.
- Reviewed dotfiles candidates.
- Fixed broken shell PATH.
- Copied selected public dotfiles.
- Removed backup files from public dotfiles.
- Sanitized wallpaper paths.
- Sanitized VPN aliases.
- Ran critical sensitive scan on public dotfiles.

### Main Findings

| Area | Finding | Result |
|---|---|---|
| Shell | PATH was misconfigured | Fixed in zshrc |
| Firewall | UFW had open inbound rules | Rules removed |
| VPN | ProtonVPN uses policy routing | Confirmed |
| DNS | Quad9 not directly detected | Needs follow-up |
| SSH | sshd disabled and inactive | Safe current state |
| Fail2ban | sshd jail active | Ready for future SSH use |
| Dotfiles | Public copy required cleanup | Sanitized |

### Current Status

The workstation now has a documented initial baseline with sanitized public dotfiles and no explicit inbound firewall allow rules.

### Next Steps

1. Review DNS behavior while VPN is active.
2. Decide whether Quad9 should be enforced outside VPN.
3. Review package list.
4. Review enabled systemd services.
5. Prepare local Git commit.
6. Review repository before any public upload.
