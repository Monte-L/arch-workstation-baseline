# VPN Kill Switch Facts Summary

## Purpose

This document summarizes the sanitized facts collected before designing a VPN kill switch.

Raw outputs and sensitive values are stored locally under private/raw/ and must not be published.

## Current Network State

| Item | Sanitized Result |
|---|---|
| Wi-Fi interface | wifi0 |
| Ethernet interface | eth0 |
| VPN interface | vpn0 |
| VPN manager | NetworkManager-managed WireGuard |
| VPN state during snapshot | active |
| VPN route | public route through vpn0 |
| VPN routing table | 52254 |
| DNS policy | Quad9 |
| IPv6 path | disabled/ignored in tested profiles |
| Firewall | UFW active |

## Firewall Baseline

| Direction | Current Policy |
|---|---|
| Incoming | deny |
| Outgoing | allow |
| Routed | disabled / deny routed |

## WireGuard Facts

The private WireGuard configuration contains the facts needed for a future kill switch.

| Item | Status |
|---|---|
| VPN address | collected privately |
| DNS entries | collected privately |
| AllowedIPs | full-tunnel route |
| Endpoint | collected privately |
| Endpoint port | 51820 |

The real VPN endpoint must remain private and must not be committed to the public repository.

## Important Finding

The current firewall allows outgoing traffic by default.

This means that if the VPN is inactive, ordinary outbound traffic can still leave through the regular Wi-Fi route.

The DNS policy improves resolver consistency, but it does not prevent non-DNS traffic from leaving outside the VPN.

## Kill Switch Design Requirement

A future kill switch should enforce:

1. Allow traffic required to establish the VPN tunnel.
2. Allow normal outbound traffic through the VPN interface.
3. Block ordinary outbound traffic through Wi-Fi or Ethernet when VPN is inactive.
4. Preserve inbound firewall protection.
5. Include a tested rollback path.
6. Avoid persistence until manual testing succeeds.

## Current Decision

No kill switch rules have been applied yet.

The next step is to design a test-only ruleset and rollback procedure.
