# Post-Reboot Validation

## Purpose

This document records the workstation state after reboot validation.

Raw post-reboot command outputs are stored locally under private/raw/ and must not be published.

## Validation Summary

| Area | Result |
|---|---|
| UFW | active |
| Fail2ban | active |
| SSH daemon | inactive |
| docker.service | disabled / inactive |
| docker.socket | disabled / inactive |
| containerd.service | disabled / inactive |
| NetworkManager-wait-online.service | disabled / inactive |
| VPN interface | vpn0 active after workaround |
| Public route decision | via vpn0 table 51820 |
| Quad9 DNS | detected directly in /etc/resolv.conf |

## Security Services

| Service | Expected State | Observed State | Result |
|---|---|---|---|
| ufw | active | active | Pass |
| fail2ban | active | active | Pass |
| sshd | inactive | inactive | Pass |

## Docker and Container Runtime

| Service | Expected State | Observed State | Result |
|---|---|---|---|
| docker.service | disabled / inactive | disabled / inactive | Pass |
| docker.socket | disabled / inactive | disabled / inactive | Pass |
| containerd.service | disabled / inactive | disabled / inactive | Pass |

## NetworkManager Wait Online

NetworkManager-wait-online.service remained disabled and inactive after reboot.

This confirms it is no longer part of the normal boot path.

## VPN Validation

After reboot, the first VPN startup attempt exposed a DNS/resolvconf issue.

A local helper was created to run a resolvconf update before starting the WireGuard profile.

After using the helper, the VPN interface became active and public route decision used the ProtonVPN WireGuard interface.

| Item | Result |
|---|---|
| VPN interface | vpn0 |
| Route table | 51820 |
| Public route | via vpn0 |
| DNS | Quad9 detected |

## Current Assessment

The post-reboot state is good.

Core security services are active, unnecessary Docker/container services remain inactive, SSH remains inactive, and VPN routing works after the temporary startup workaround.

## Remaining Work

| Item | Priority | Status |
|---|---|---|
| Replace VPN startup workaround with cleaner architecture | High | Future |
| Evaluate NetworkManager-managed WireGuard profile | High | Future |
| Evaluate VPN kill switch | High | Future |
| Perform external DNS leak test | High | Open |
| Review public repository before GitHub upload | Medium | Open |
