# VPN Kill Switch Rollback Plan

## Purpose

This document defines the rollback strategy that must exist before testing VPN kill switch rules.

No kill switch rules are applied in this document.

## Why Rollback Is Required

A VPN kill switch can break connectivity if firewall rules are incorrect.

Before applying any restrictive outbound firewall policy, the workstation must have a clear recovery path.

## Current Baseline Firewall State

The current baseline is:

| Direction | Policy |
|---|---|
| Incoming | Deny |
| Outgoing | Allow |
| Routed | Deny |

This allows normal outbound traffic and blocks unsolicited inbound traffic.

## Emergency Rollback Concept

If a kill switch test breaks connectivity, the emergency goal is to restore the baseline firewall state.

The conceptual rollback is:

1. Reset or clean restrictive test rules.
2. Restore deny incoming.
3. Restore allow outgoing.
4. Restore deny routed.
5. Re-enable UFW.
6. Confirm internet connectivity.
7. Confirm VPN can be started manually.

## Emergency Rollback Commands

The basic UFW rollback model is:

    sudo ufw --force reset
    sudo ufw default deny incoming
    sudo ufw default allow outgoing
    sudo ufw default deny routed
    sudo ufw --force enable

## Local Rollback Script

A local helper script was created for emergency firewall rollback.

Local path:

    ~/.local/bin/ufw-baseline-restore

The script requires an explicit apply flag:

    ufw-baseline-restore --apply

Without the apply flag, the script prints usage information and makes no changes.

## Script Behavior

When executed with `--apply`, the script restores the baseline UFW policy:

| Step | Action |
|---|---|
| 1 | Reset UFW rules |
| 2 | Set incoming policy to deny |
| 3 | Set outgoing policy to allow |
| 4 | Set routed policy to deny |
| 5 | Enable UFW |
| 6 | Print final UFW status |

## Safety Notes

This rollback restores basic outbound connectivity.

It removes custom firewall rules.

Before testing a kill switch, the current firewall status should be saved under private/raw/.

The script is intended for emergency recovery during VPN kill switch testing.

It should be available before any restrictive outbound firewall rules are tested.

## Test Requirements Before Kill Switch

Before applying any kill switch rule, confirm:

| Requirement | Status |
|---|---|
| UFW baseline saved | Required |
| VPN endpoint identified privately | Required |
| VPN interface identified | Required |
| Wi-Fi interface identified | Required |
| Ethernet interface identified | Required |
| Rollback commands prepared | Required |
| Manual VPN start works | Required |
| Manual VPN stop works | Required |

## Current Status

Rollback plan drafted.

Local rollback helper created.

No kill switch rules applied yet.
