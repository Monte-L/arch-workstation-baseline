# Local Helper Scripts

## Purpose

This document records local helper scripts used during the Arch Linux workstation baseline and VPN hardening workflow.

These scripts are local workstation helpers.

Some scripts are operational, some are dry-run only, and some are emergency rollback helpers.

Sensitive values such as VPN endpoint details, private keys, and local profile names must not be published.

## Helper Script Summary

| Helper | Purpose | Applies Changes |
|---|---|---|
| vpn-nm-on | Start the NetworkManager-managed WireGuard VPN profile | Yes |
| vpn-nm-off | Stop the active NetworkManager WireGuard VPN connection | Yes |
| vpn-on-safe | Legacy wg-quick/resolvconf VPN startup fallback | Yes |
| vpn-off-safe | Legacy wg-quick/resolvconf VPN shutdown fallback | Yes |
| ufw-baseline-restore | Restore UFW to the baseline firewall policy | Yes, only with --apply |
| vpn-killswitch-dryrun | Preview planned VPN kill switch rules | No |
| vpn-endpoint-resolve-dryrun | Resolve VPN endpoint information for planning | No |
| vpn-killswitch-test | Apply temporary UFW kill switch test rules | Yes, only with --apply |

## NetworkManager VPN Helpers

### vpn-nm-on

Purpose:

Start the NetworkManager-managed WireGuard VPN profile.

Expected result:

- VPN interface becomes active.
- Public route uses the VPN interface.
- DNS policy remains active.

This is now the preferred VPN control method.

### vpn-nm-off

Purpose:

Stop the active NetworkManager WireGuard connection.

Expected result:

- VPN interface is removed.
- Public route returns to the normal Wi-Fi route.
- With kill switch test rules active, ordinary outbound traffic should be blocked.

## Legacy VPN Fallback Helpers

### vpn-on-safe

Purpose:

Start the older wg-quick based VPN workflow after refreshing resolvconf.

This was created after a post-reboot resolvconf issue.

Current role:

Fallback only.

### vpn-off-safe

Purpose:

Stop the older wg-quick based VPN workflow and refresh resolvconf afterward.

Current role:

Fallback only.

## Firewall Rollback Helper

### ufw-baseline-restore

Purpose:

Restore the baseline UFW policy.

Usage:

    ufw-baseline-restore --apply

Baseline restored:

| Direction | Policy |
|---|---|
| Incoming | deny |
| Outgoing | allow |
| Routed | deny |

Safety behavior:

Without `--apply`, the script prints usage information and makes no changes.

This helper should remain available before testing any restrictive kill switch rule.

## Kill Switch Planning Helpers

### vpn-killswitch-dryrun

Purpose:

Preview the planned UFW kill switch rule model.

Behavior:

- Detects local VPN profile information.
- Redacts endpoint information from output.
- Prints planned firewall model.
- Applies no firewall changes.

### vpn-endpoint-resolve-dryrun

Purpose:

Resolve endpoint information for kill switch planning.

Behavior:

- Detects the VPN endpoint locally.
- Redacts endpoint information from output.
- Confirms endpoint type and port.
- Applies no firewall changes.

## Kill Switch Test Helper

### vpn-killswitch-test

Purpose:

Apply a temporary UFW-based kill switch test.

Usage:

    vpn-killswitch-test --apply

Safety behavior:

The script requires:

1. `--apply` argument.
2. Manual confirmation by typing APPLY.
3. Rollback helper available.

Test behavior:

| State | Expected Result |
|---|---|
| VPN active | traffic works through VPN |
| VPN inactive | ordinary outbound traffic is blocked |
| VPN reactivated | traffic works again |
| rollback executed | baseline UFW policy is restored |

Current status:

The manual kill switch test passed.

The test is not persistent.

## Current Operating Decision

Current preferred VPN method:

    NetworkManager-managed WireGuard

Current fallback:

    wg-quick plus resolvconf helper

Current firewall state after testing:

    baseline restored

Persistent kill switch:

    not enabled yet

## Safety Notes

Do not publish:

- real VPN endpoint
- private keys
- NetworkManager private connection files
- raw UFW outputs containing endpoint IPs
- raw command outputs from private/raw/

Public documentation should use placeholders such as:

- vpn0
- wifi0
- eth0
- [VPN-ENDPOINT-REDACTED]
