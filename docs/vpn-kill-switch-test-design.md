# VPN Kill Switch Test Design

## Purpose

This document defines the first test-only VPN kill switch design.

No firewall rules are applied by this document.

## Design Goal

Prevent ordinary outbound traffic from leaving through Wi-Fi or Ethernet when the VPN is inactive.

Allow normal traffic only when it goes through the VPN interface.

## Planned Test Model

The first test should be manual and reversible.

The intended behavior is:

| State | Expected Result |
|---|---|
| VPN active | outbound traffic allowed through vpn0 |
| VPN inactive | ordinary outbound traffic blocked |
| VPN inactive | traffic to VPN endpoint allowed |
| Rollback executed | normal outbound traffic restored |

## Required Private Values

The implementation requires private values that must not be published:

| Value | Purpose |
|---|---|
| VPN endpoint IP or hostname | allow tunnel establishment |
| VPN endpoint port | allow WireGuard handshake |
| Wi-Fi interface | identify non-VPN route |
| Ethernet interface | identify wired non-VPN route |
| VPN interface | allow traffic through tunnel |

## UFW-Based Concept

The rough UFW idea is:

1. Change default outgoing policy to deny.
2. Allow outbound traffic through the VPN interface.
3. Allow outbound traffic to the VPN endpoint on the required WireGuard port.
4. Keep incoming denied.
5. Keep routed denied unless explicitly required.
6. Test VPN-on and VPN-off behavior.
7. Roll back immediately if connectivity breaks.

## Important Limitation

UFW may not be the most precise tool for advanced policy routing and VPN leak protection.

If UFW becomes too limited, nftables should be evaluated later.

## Safety Rules

Do not apply a persistent kill switch before:

1. Saving the current UFW state.
2. Testing rollback.
3. Testing VPN activation.
4. Testing VPN deactivation.
5. Testing DNS behavior.
6. Testing after reboot.
7. Confirming the endpoint rule works.

## Rollback First Principle

Before applying any restrictive rule, rollback commands must be ready in the terminal or saved locally.

The emergency rollback should restore:

| Setting | Value |
|---|---|
| Incoming | deny |
| Outgoing | allow |
| Routed | deny |
| UFW | enabled |

## Current Status

Test design drafted.

Rules not applied yet.

## Local Dry-Run Helper

A local dry-run helper was created to preview the intended VPN kill switch rule model before applying any firewall changes.

Local helper:

    ~/.local/bin/vpn-killswitch-dryrun

The helper detects the local NetworkManager WireGuard profile and endpoint information, but redacts the endpoint from its output.

It does not apply firewall rules.

The helper previews the following model:

| Step | Planned Action |
|---|---|
| 1 | Keep incoming denied |
| 2 | Change outgoing default to deny |
| 3 | Keep routed denied |
| 4 | Allow outbound traffic through the VPN interface |
| 5 | Allow outbound traffic to the VPN endpoint port |
| 6 | Keep emergency rollback available |

Emergency rollback remains:

    ufw-baseline-restore --apply

No kill switch rules have been applied yet.
