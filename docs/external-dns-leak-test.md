# External DNS Leak Test

## Purpose

This document records sanitized external DNS leak testing for the Arch Linux workstation.

Raw screenshots, public IP addresses, resolver IP addresses, exact locations, ISP names, and VPN endpoint details must not be published.

## Test Scope

The test validates DNS behavior in two states:

1. VPN active.
2. VPN inactive.

The goal is to confirm whether DNS behavior matches the intended DNS policy and whether the local ISP DNS appears during testing.

## Current DNS Policy

| State | Intended DNS |
|---|---|
| VPN active | Quad9 through NetworkManager WireGuard profile |
| VPN inactive | Quad9 through Wi-Fi NetworkManager profile |

## Sanitized Test Results

| State | Public Route | Public IP Organization | DNS Leak Test Result | Local ISP DNS Observed | Result |
|---|---|---|---|---|---|
| VPN active | VPN interface | VPN / non-local organization | DNS servers changed from local state | No | Pass |
| VPN inactive | Wi-Fi interface | Local ISP detected privately | DNS provider differed from local ISP | No | Pass |

## VPN Active Interpretation

With VPN active:

- public route used the VPN interface
- public IP organization changed from the local ISP
- external DNS leak test showed different DNS/location behavior from the VPN-off state
- local ISP DNS was not observed

Result: pass.

## VPN Inactive Interpretation

With VPN inactive:

- public route used the Wi-Fi interface
- terminal public IP organization matched the local ISP
- external DNS leak test showed DNS infrastructure different from the local ISP
- local ISP DNS was not observed

This is consistent with the configured Quad9 DNS policy on the Wi-Fi NetworkManager profile.

Result: pass.

## Important Notes

Some DNS leak test websites show the ISP or organization of the DNS resolver, not necessarily the user's local internet provider.

For this reason, the important comparison was:

- local ISP from terminal with VPN inactive
- DNS provider shown by the leak test
- public IP organization with VPN active
- whether the local ISP DNS appeared in DNS results

The local ISP DNS was not observed in the external DNS leak test.

## Sanitization Rules

This public document intentionally does not include:

- public IP addresses
- DNS resolver IP addresses
- real ISP names
- exact locations
- VPN endpoint information
- raw screenshots

## Current Assessment

External DNS leak testing passed in both VPN-active and VPN-inactive states.

The result supports the current DNS policy:

- Quad9 configured on the Wi-Fi profile
- Quad9 configured on the NetworkManager WireGuard profile
- no local ISP DNS observed during external testing

## Remaining Work

1. Repeat DNS leak testing after persistent kill switch changes.
2. Repeat DNS leak testing after future NetworkManager profile changes.
3. Repeat testing when connecting to a new Wi-Fi or Ethernet profile.
4. Consider browser DNS-over-HTTPS settings during future tests.
