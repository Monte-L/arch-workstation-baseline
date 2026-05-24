# VPN Autoconnect Validation

## Purpose

This document records validation of NetworkManager WireGuard autoconnect behavior.

The goal was to allow the VPN to start automatically after boot while the persistent firewall kill switch prevents ordinary traffic from leaving outside the VPN.

## Current Model

| Component | State |
|---|---|
| VPN manager | NetworkManager-managed WireGuard |
| VPN autoconnect | enabled |
| Wi-Fi autoconnect | enabled |
| Firewall | UFW |
| UFW incoming policy | deny |
| UFW outgoing policy | deny |
| VPN interface outbound rule | configured |
| VPN endpoint rule | configured privately |
| DNS policy | Quad9 |
| Emergency rollback | available |

## Boot Behavior

Expected boot flow:

1. System boots.
2. Wi-Fi connects.
3. UFW blocks ordinary outbound traffic outside the VPN.
4. NetworkManager starts the WireGuard VPN profile automatically.
5. Public traffic uses the VPN interface.
6. DNS policy remains Quad9-based.

## Validation Result

VPN autoconnect was enabled and tested.

Observed result:

| Check | Result |
|---|---|
| VPN profile autoconnect enabled | pass |
| Wi-Fi profile autoconnect enabled | pass |
| VPN starts automatically after boot | pass |
| Internet works after VPN activation | pass |
| Public IP path uses VPN | pass |
| Kill switch remains active | pass |

## Current Operating State

The workstation now uses the following privacy-oriented operating model:

| State | Expected Behavior |
|---|---|
| VPN active | traffic works through VPN |
| VPN inactive | ordinary outbound traffic blocked |
| VPN fails to connect | ordinary traffic remains blocked |
| rollback executed | normal outbound traffic restored |

## Rollback

Emergency rollback remains available:

    ufw-baseline-restore --apply

Rollback restores the baseline firewall state:

| Direction | Policy |
|---|---|
| Incoming | deny |
| Outgoing | allow |
| Routed | deny |

## Important Notes

VPN autoconnect is useful only because the persistent kill switch is already active.

Without the kill switch, traffic could leave through Wi-Fi before the VPN is ready.

## Result

NetworkManager VPN autoconnect validation passed.

The workstation now has a stronger privacy baseline:

- VPN autoconnect enabled
- persistent kill switch active
- Quad9 DNS policy active
- rollback available
