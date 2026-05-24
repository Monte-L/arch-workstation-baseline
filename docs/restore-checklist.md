# Restore Checklist

## Purpose

This document defines a high-level restore checklist for rebuilding the workstation.

It is not a full automated restore script.

## Restore Order

| Step | Action | Status |
|---|---|---|
| 1 | Install base Arch Linux | planned |
| 2 | Enable NetworkManager | planned |
| 3 | Install essential packages | planned |
| 4 | Restore package list | planned |
| 5 | Restore selected dotfiles | planned |
| 6 | Restore shell helper scripts | planned |
| 7 | Restore UFW baseline | planned |
| 8 | Restore Fail2ban configuration | planned |
| 9 | Restore VPN configuration privately | planned |
| 10 | Validate VPN routing | planned |
| 11 | Validate DNS policy | planned |
| 12 | Validate external DNS leak behavior | planned |
| 13 | Restore project repositories | planned |
| 14 | Validate GitHub remotes | planned |
| 15 | Test backup integrity | planned |

## Essential Packages

The package list should be exported with:

    pacman -Qqe

The foreign package list should be exported with:

    pacman -Qqem

These files should be stored privately in the backup target.

## Services to Validate After Restore

| Service | Expected State |
|---|---|
| NetworkManager | enabled / active |
| ufw | enabled / active |
| fail2ban | enabled / active |
| sshd | disabled / inactive unless intentionally enabled |
| docker | disabled / inactive unless needed |
| containerd | disabled / inactive unless Docker is active |
| NetworkManager-wait-online | disabled |

## Firewall Baseline

Expected UFW baseline:

| Direction | Policy |
|---|---|
| Incoming | deny |
| Outgoing | allow |
| Routed | deny |

## VPN Restore Notes

VPN configuration must be restored privately.

Do not publish:

- private keys
- endpoint
- real profile names
- raw NetworkManager connection files

After restoring VPN, validate:

1. VPN activation.
2. VPN deactivation.
3. route through VPN.
4. DNS policy.
5. external DNS leak test.

## Current Status

Restore checklist drafted.

No full restore test has been performed yet.
