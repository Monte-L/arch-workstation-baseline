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

The exact rollback commands should be reviewed before use.

A basic UFW rollback model is:

sudo ufw --force reset
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw default deny routed
sudo ufw --force enable

## Safety Notes

This rollback restores basic outbound connectivity.

It removes custom firewall rules.

Before testing a kill switch, the current firewall status should be saved under private/raw/.

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

No kill switch rules applied yet.
