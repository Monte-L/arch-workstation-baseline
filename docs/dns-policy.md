# DNS Policy

## Purpose

This document defines the intended DNS behavior for the Arch Linux workstation.

Raw DNS outputs are stored locally under private/raw/ and must not be published.

## Current DNS Policy

The current policy is:

| State | DNS Policy |
|---|---|
| VPN active | Use Quad9 DNS through the NetworkManager WireGuard profile |
| VPN inactive | Use Quad9 DNS through the active Wi-Fi NetworkManager profile |
| IPv6 | Ignored/disabled in the currently tested path |
| External DNS leak test | Still required |
| VPN kill switch | Not implemented yet |

## Reasoning

Earlier testing showed different DNS behavior between VPN-on and VPN-off states.

Before applying a DNS policy:

| State | DNS Result |
|---|---|
| VPN active | Quad9 detected |
| VPN inactive | Quad9 not detected directly |

To make DNS behavior more predictable before implementing a VPN kill switch, Quad9 DNS was configured on the active Wi-Fi profile.

## Current Validation

After applying the DNS policy:

| State | Public Route | DNS Result |
|---|---|---|
| VPN inactive | via Wi-Fi | Quad9 detected |
| VPN active | via vpn0 | Quad9 detected |

For public documentation, the local VPN interface/profile name is represented as vpn0.

## NetworkManager Direction

NetworkManager is now the preferred control layer for Wi-Fi, WireGuard, and DNS behavior.

The current direction is:

1. Manage Wi-Fi through NetworkManager.
2. Manage WireGuard through NetworkManager.
3. Define DNS per NetworkManager profile.
4. Keep wg-quick as fallback.
5. Implement VPN leak protection later.

## Scope Limitation

The DNS policy was applied to the current active Wi-Fi profile.

New Wi-Fi or Ethernet profiles may not automatically inherit this DNS policy unless they are configured separately or a NetworkManager dispatcher rule is created.

## Privacy Notes

This policy reduces DNS exposure to the local network or ISP when VPN is inactive.

However, it does not replace a VPN kill switch.

Without a kill switch, ordinary outbound traffic can still leave through Wi-Fi when VPN is inactive.

## Remaining Work

1. Apply DNS policy to other existing trusted profiles if needed.
2. Create a NetworkManager dispatcher policy for new connections, if desired.
3. Perform external DNS leak testing with VPN active.
4. Perform external DNS leak testing with VPN inactive.
5. Validate behavior after another reboot.
6. Evaluate firewall-based VPN kill switch.
7. Decide whether VPN-off traffic should be blocked entirely.
