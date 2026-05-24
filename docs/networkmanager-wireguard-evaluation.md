# NetworkManager WireGuard Evaluation

## Purpose

This document records the evaluation of moving from a wg-quick based WireGuard workflow to a NetworkManager-managed WireGuard workflow.

The goal is to improve long-term reliability, desktop integration, Wi-Fi behavior, DNS control, and future VPN leak protection.

Raw command outputs are stored locally under private/raw/ and must not be published.

## Current Context

The initial baseline used:

- wg-quick
- resolvconf
- local vpn-on-safe and vpn-off-safe helper scripts

This setup worked, but a post-reboot DNS/resolvconf issue required a temporary workaround before starting WireGuard.

The long-term preferred direction is to evaluate NetworkManager as the main WireGuard manager.

## Preflight Result

NetworkManager is installed, enabled, and active.

WireGuard tools remain available:

| Tool | Status |
|---|---|
| wg | available |
| wg-quick | available |

The existing wg-quick setup remains available as a fallback.

## NetworkManager WireGuard Test

A WireGuard profile was imported into NetworkManager and tested manually.

For public documentation, the local VPN profile and interface names are represented with the generic placeholder vpn0.

| Test | Result |
|---|---|
| NetworkManager profile import | successful |
| VPN activation through NetworkManager | successful |
| VPN deactivation through NetworkManager | successful |
| Public route through VPN | successful |
| Quad9 DNS in profile | configured |
| IPv6 in VPN profile | disabled |
| Autoconnect | disabled during testing |

## Routing Result

When activated through NetworkManager, public route decision selected the VPN interface.

The routing table used by NetworkManager differed from the earlier wg-quick table.

This is expected and not considered a problem.

The important result is that public traffic was routed through the VPN interface.

## DNS Result

The NetworkManager WireGuard profile includes Quad9 DNS entries.

This is better than relying on an external resolvconf workaround.

DNS behavior still requires external DNS leak testing before being considered fully validated.

## Current Decision

NetworkManager-managed WireGuard is viable.

However, the migration should not be considered fully complete until post-reboot testing confirms the same behavior.

## Current Operating Model

| Method | Role |
|---|---|
| NetworkManager WireGuard | primary candidate for future use |
| wg-quick + vpn-on-safe | fallback |
| VPN autoconnect | disabled for now |
| kill switch | not implemented yet |

## Next Validation Required

The next test should confirm behavior after reboot:

1. Boot normally.
2. Confirm VPN is not automatically active.
3. Start VPN using NetworkManager helper.
4. Confirm route through VPN.
5. Confirm DNS behavior.
6. Confirm fallback still works if NetworkManager activation fails.

## Future Work

After post-reboot validation, evaluate:

1. Making NetworkManager the primary VPN workflow.
2. Replacing vpn-on-safe with NetworkManager-based helpers.
3. Defining final DNS policy.
4. Planning VPN kill switch / leak protection.
5. Performing external DNS leak testing.

## Post-Reboot Validation

A post-reboot validation was performed after importing and testing the NetworkManager WireGuard profile.

For public documentation, the local VPN interface/profile name is represented as vpn0.

| Check | Result |
|---|---|
| VPN inactive after boot | confirmed |
| NetworkManager VPN helper | worked after reboot |
| VPN route after activation | via vpn0 |
| NetworkManager routing table | 52254 |
| Quad9 DNS after activation | detected |
| UFW after reboot | active |
| Fail2ban after reboot | active |
| Docker after reboot | inactive |
| containerd after reboot | inactive |
| SSH after reboot | inactive |
| NetworkManager-wait-online | disabled |

## Updated Decision

NetworkManager-managed WireGuard passed manual activation, deactivation, and post-reboot validation.

The NetworkManager workflow is now considered the preferred VPN control method.

The previous wg-quick plus resolvconf helper remains available as fallback until the final VPN/DNS policy and kill switch are implemented.
