# VPN Kill Switch Test Results

## Purpose

This document records the first manual VPN kill switch test.

Sensitive endpoint details and raw firewall outputs are stored locally under private/raw/ and must not be published.

## Test Summary

A temporary UFW-based kill switch test was applied manually.

The goal was to verify whether ordinary outbound traffic would be blocked when the VPN was disconnected.

## Test Model

The temporary test model used the following logic:

| Rule Area | Behavior |
|---|---|
| Incoming traffic | denied |
| Outgoing traffic | denied by default during test |
| Routed traffic | denied |
| VPN interface | outbound traffic allowed |
| VPN endpoint | outbound UDP traffic allowed to endpoint port |
| Rollback | available through local helper |

## Test Results

| Test State | Expected Result | Observed Result | Status |
|---|---|---|---|
| VPN active after applying test rules | Traffic works through VPN | Ping test succeeded | Pass |
| VPN inactive after disconnecting VPN | Ordinary traffic blocked | Ping test failed with packet loss | Pass |
| VPN reactivated | Traffic works again through VPN | Ping test succeeded | Pass |
| Rollback executed | Baseline firewall restored | Outgoing policy restored to allow | Pass |

## Final Firewall State After Test

After testing, the emergency rollback helper was executed.

Final baseline:

| Direction | Policy |
|---|---|
| Incoming | deny |
| Outgoing | allow |
| Routed | deny |

## Interpretation

The first manual kill switch test was successful.

When the VPN was inactive, normal outbound traffic through Wi-Fi was blocked.

When the VPN was active, traffic through the VPN interface worked.

This confirms that the UFW-based model is viable for a controlled test scenario.

## Important Limitation

The kill switch is not currently persistent.

The test rules were rolled back after validation.

Further testing is required before enabling any persistent kill switch behavior.

## Remaining Work

1. Document the local test script behavior.
2. Test DNS behavior while kill switch rules are active.
3. Test web access while VPN is active and kill switch rules are active.
4. Test VPN reconnect behavior after disconnect.
5. Test behavior after reboot.
6. Decide whether to keep UFW or evaluate nftables for a cleaner persistent model.
7. Avoid publishing real VPN endpoint information.
