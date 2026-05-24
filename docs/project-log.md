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

## Service Hardening Update

### Completed

- Disabled docker.service from starting automatically.
- Disabled docker.socket to prevent automatic socket activation.
- Stopped containerd.service after Docker cleanup.
- Confirmed previous containerd localhost listener was no longer observed.
- Preserved Docker as an installed tool for manual use during infrastructure labs.

### Result

Docker is now installed but not always running.

This reduces the default workstation service footprint while keeping Docker available for lab and development work.

## Post-Hardening Services Snapshot

A post-hardening services snapshot was collected after Docker and containerd cleanup.

### Expected State

- docker.service disabled and inactive
- docker.socket disabled and inactive
- containerd.service disabled and inactive
- WireGuard tools still available
- UFW still enabled
- Fail2ban still enabled
- NetworkManager still enabled
- No unnecessary Docker/containerd listener during normal workstation use

### Notes

Docker remains installed and can be started manually when needed for infrastructure labs.

This keeps the workstation lighter during normal use while preserving lab functionality.

## Enabled Services Full Review

### Completed

- Reviewed enabled systemd unit files.
- Reviewed enabled sockets.
- Reviewed enabled timers.
- Reviewed selected running services.
- Reviewed active timers.
- Confirmed no enabled Docker, containerd, SSH, CUPS, Avahi, Samba, Bluetooth, or systemd-resolved services were observed.
- Documented sshd-related local Unix socket as not equivalent to network-exposed SSH.

### Result

The enabled service set is minimal and appropriate for a personal Arch Linux workstation.

No obvious unnecessary network-facing services were found enabled.

## Package Cleanup Final Review

### Completed

- Confirmed yay-debug is not installed.
- Confirmed wireguard-dkms is not installed.
- Confirmed wg and wg-quick remain available.
- Reviewed orphan packages.
- Removed eww-debug.
- Kept Go as a documented review item.
- Kept tor-browser-alpha-bin as a documented review item.

### Result

The package set is cleaner and unnecessary debug packages were removed or confirmed absent.

Remaining package review items are Go and tor-browser-alpha-bin.

## DNS Behavior Review

### Completed

- Tested DNS behavior with ProtonVPN active.
- Confirmed VPN-active public route through protonch table 51820.
- Confirmed Quad9 direct detection during VPN-active test.
- Tested DNS behavior with ProtonVPN inactive.
- Confirmed VPN-off public route through Wi-Fi.
- Confirmed Quad9 was not directly detected during VPN-off test.
- Documented that DNS behavior differs between VPN-on and VPN-off states.

### Result

VPN-active state is preferred.

VPN-off state requires further review because normal traffic returns to Wi-Fi and Quad9 was not directly detected.

Future hardening should evaluate VPN kill switch or DNS enforcement after the inventory phase is complete.
