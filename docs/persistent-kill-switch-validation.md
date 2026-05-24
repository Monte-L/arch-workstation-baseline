# Persistent VPN Kill Switch Validation

## Purpose

This document records validation of the persistent VPN kill switch behavior after reboot.

Sensitive endpoint details, public IP addresses, and raw private outputs are not published.

## Current Model

The workstation uses:

| Component | State |
|---|---|
| VPN manager | NetworkManager-managed WireGuard |
| Firewall | UFW |
| Default incoming policy | deny |
| Default outgoing policy | deny |
| Default routed policy | deny / disabled |
| VPN endpoint rule | configured privately |
| VPN interface outbound rule | configured |
| Emergency rollback | available |

## Post-Reboot Test

After reboot, the system was tested before enabling the VPN manually.

Observed behavior:

| State | Expected Result | Observed Result | Status |
|---|---|---|---|
| After boot, before VPN activation | ordinary web traffic blocked | browser access failed | Pass |
| After manual VPN activation | web traffic restored through VPN | browser access worked | Pass |
| After VPN activation | public IP changed to VPN path | VPN public IP observed | Pass |

## Interpretation

The persistent UFW kill switch survived reboot.

Before the VPN was manually activated, ordinary outbound traffic was blocked.

After starting the NetworkManager WireGuard VPN profile, connectivity returned through the VPN path.

## Important Notes

VPN autoconnect is not enabled yet.

The current boot behavior is:

1. Wi-Fi connects.
2. UFW blocks ordinary outbound traffic.
3. User manually starts VPN with `vpn-nm-on`.
4. Traffic works through VPN.

## Rollback

Emergency rollback remains:

    ufw-baseline-restore --apply

Rollback should only be used if normal non-VPN outbound connectivity is intentionally required or if VPN activation fails.

## Result

Persistent kill switch validation passed.

The next recommended step is to evaluate NetworkManager VPN autoconnect.
