# Final Workstation Status Review

## Purpose

This document records the final status of the Arch Linux workstation baseline after VPN, DNS, firewall, backup, and restore validation.

Sensitive values such as real VPN endpoints, public IPs, exact network names, private keys, and raw private outputs are not published.

## Final Baseline Summary

| Area | Status |
|---|---|
| Operating system | Arch Linux |
| Desktop environment | Hyprland |
| VPN management | NetworkManager-managed WireGuard |
| VPN autoconnect | enabled |
| DNS policy | Quad9 |
| Firewall | UFW |
| Kill switch | persistent and validated |
| Fail2ban | active |
| SSH service | inactive unless intentionally enabled |
| Docker/containerd | disabled/inactive unless needed |
| Backup strategy | documented |
| Restic encrypted backup test | passed |
| Restore testing | performed |

## Privacy-Oriented Network Model

The current model is:

1. Wi-Fi connects automatically.
2. UFW blocks ordinary outbound traffic outside the VPN.
3. NetworkManager starts the WireGuard VPN profile automatically.
4. Public traffic routes through the VPN interface.
5. DNS uses Quad9.
6. If VPN is inactive, ordinary outbound traffic remains blocked.

## Firewall State

Expected UFW model:

| Direction | Policy |
|---|---|
| Incoming | deny |
| Outgoing | deny |
| Routed | deny / disabled |

Allowed exceptions:

| Exception | Purpose |
|---|---|
| DHCP bootstrap | allow network connection setup |
| VPN endpoint | allow WireGuard tunnel establishment |
| VPN interface outbound | allow traffic through VPN |

## VPN State

| Item | Status |
|---|---|
| NetworkManager WireGuard profile | validated |
| Manual VPN start/stop | validated |
| VPN autoconnect | validated |
| Post-reboot VPN behavior | validated |
| Public route through VPN | validated |

## DNS State

| State | DNS Result |
|---|---|
| VPN active | Quad9 |
| VPN inactive | Quad9 on Wi-Fi profile |
| External DNS leak test | passed in sanitized form |

## Backup and Restore State

| Area | Status |
|---|---|
| USB non-sensitive backup | passed |
| Restic encrypted backup test | passed |
| Restic check | passed |
| Restore test | passed |
| Exclude policy | created |
| Private backup planning | started |

## Rollback

Emergency firewall rollback remains available:

    ufw-baseline-restore --apply

This restores the baseline non-kill-switch firewall state:

| Direction | Policy |
|---|---|
| Incoming | deny |
| Outgoing | allow |
| Routed | deny |

## Final Assessment

The workstation baseline is complete for the current phase.

The system now has:

- sanitized public documentation
- NetworkManager-managed VPN
- VPN autoconnect
- persistent firewall kill switch
- Quad9 DNS policy
- external DNS leak validation
- backup and restore strategy
- Restic encrypted backup validation
- emergency rollback path

## Future Optional Work

Potential future improvements:

1. Review browser DNS-over-HTTPS settings.
2. Add periodic backup schedule.
3. Evaluate AppArmor or auditd.
4. Evaluate full disk encryption on reinstall.
5. Improve Restic private backup strategy.
6. Add a second backup destination.
7. Create a polished portfolio/LinkedIn summary.
