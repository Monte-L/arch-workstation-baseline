# VPN Kill Switch and Leak Protection Plan

## Purpose

This document defines the planned approach for VPN leak protection on the Arch Linux workstation.

The goal is to prevent ordinary outbound traffic from leaving through Wi-Fi or Ethernet when the VPN is not active.

No kill switch rules are applied at this stage.

This document is a planning and safety design step.

## Current State

The workstation currently uses:

| Component | State |
|---|---|
| VPN management | NetworkManager-managed WireGuard |
| VPN fallback | wg-quick plus vpn-on-safe |
| DNS policy | Quad9 configured for VPN-on and VPN-off states |
| Firewall | UFW active |
| Incoming traffic | denied by default |
| Outgoing traffic | allowed by default |
| Kill switch | not implemented |

## Important Clarification

The VPN cannot start before the system has basic network connectivity.

A VPN needs an underlying Wi-Fi or Ethernet connection before it can establish a tunnel.

Therefore, the goal is not literally VPN before internet.

The real goal is:

1. allow the base network to connect
2. allow only the traffic needed to establish the VPN
3. block ordinary outbound traffic outside the VPN
4. allow normal traffic only through the VPN interface

## Desired Final Behavior

| State | Expected Behavior |
|---|---|
| VPN inactive | ordinary outbound internet traffic blocked |
| VPN inactive | traffic to VPN endpoint allowed |
| VPN active | ordinary traffic allowed through VPN interface |
| VPN active | DNS follows defined DNS policy |
| VPN down unexpectedly | ordinary traffic does not fall back to Wi-Fi |
| rollback needed | user can restore normal internet access safely |

## Current Risk

At the current stage, UFW allows outgoing traffic by default.

This means that when the VPN is inactive, traffic can leave through the regular Wi-Fi route.

The DNS policy has improved DNS consistency, but it does not prevent non-DNS traffic from leaving outside the VPN.

## Possible Approaches

| Approach | Description | Pros | Cons |
|---|---|---|---|
| ProtonVPN native kill switch | Use ProtonVPN built-in leak protection if available | Simple, provider-integrated | Less transparent, depends on client behavior |
| NetworkManager dispatcher | Use NetworkManager scripts to enforce rules when VPN changes state | Integrates with connection events | Requires careful scripting |
| UFW-based kill switch | Use firewall rules to restrict outbound traffic | Easier to understand, already using UFW | Can be limited for advanced policy routing |
| nftables-based kill switch | Use lower-level firewall rules | Powerful and precise | More complex |
| Hybrid model | NetworkManager manages VPN, firewall enforces leak protection | Strong and flexible | Requires careful testing |

## Preferred Direction

The preferred long-term direction is a hybrid model:

1. NetworkManager manages WireGuard.
2. DNS policy is defined through NetworkManager profiles.
3. Firewall rules prevent ordinary outbound leaks.
4. Rollback commands are prepared before applying rules.
5. External DNS leak testing confirms behavior.

## Safety Requirements Before Applying Rules

Before implementing a kill switch, the following must be prepared:

1. Current firewall rules backup.
2. Rollback commands.
3. Active VPN endpoint identification.
4. Wi-Fi and VPN interface identification.
5. Test plan for VPN-on state.
6. Test plan for VPN-off state.
7. Test plan after reboot.
8. Confirmation that local package management can still work when intended.
9. Confirmation that Git push/pull behavior is understood.
10. Emergency command to restore allow-outgoing behavior.

## Rollback Concept

A rollback must be available before applying any restrictive rules.

The basic emergency rollback model is:

| Action | Purpose |
|---|---|
| reset UFW rules | restore known firewall state |
| allow outgoing | restore normal internet |
| deny incoming | keep inbound firewall protection |
| re-enable UFW | return to baseline firewall |

The exact commands will be documented before any kill switch rule is applied.

## Information Needed Before Implementation

The following information must be collected privately:

| Item | Why it is needed |
|---|---|
| Wi-Fi interface name | to identify non-VPN path |
| Ethernet interface name | to identify wired non-VPN path |
| VPN interface name | to allow traffic through VPN |
| VPN endpoint address | to allow tunnel establishment |
| VPN endpoint port | to allow tunnel establishment |
| DNS behavior | to avoid DNS leaks |
| NetworkManager profile names | to coordinate VPN events |

Sensitive values such as real endpoints must not be published.

## Initial Implementation Strategy

The first implementation should be conservative.

Recommended first test model:

1. Keep current UFW baseline.
2. Do not enable kill switch at boot yet.
3. Create a test-only script.
4. Apply rules manually.
5. Test VPN-on traffic.
6. Test VPN-off blocking.
7. Test rollback.
8. Only then consider persistence.

## Test Matrix

| Test | Expected Result |
|---|---|
| VPN active route check | public route goes through VPN interface |
| VPN inactive route check | ordinary traffic blocked or intentionally allowed only during test |
| DNS with VPN active | expected resolver path |
| DNS with VPN inactive | expected blocked state or Quad9 fallback |
| VPN disconnect test | no ordinary fallback leak |
| reboot test | rules behave as expected |
| rollback test | normal connectivity restored |

## Current Decision

No kill switch rules are applied yet.

The next step is to collect a private kill switch facts snapshot and prepare a rollback plan.

Status: planning phase.
