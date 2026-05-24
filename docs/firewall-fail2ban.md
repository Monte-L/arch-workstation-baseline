# Firewall and Fail2ban Baseline

## Purpose

This document records the workstation firewall state, exposed ports, listening services, and Fail2ban status.

Raw firewall and socket outputs are stored locally under private/raw/ and must not be published.

## Firewall Tool

| Item | Status |
|---|---|
| Firewall frontend | UFW |
| UFW enabled at boot | Yes |
| UFW active | Yes |
| Default incoming policy | Deny |
| Default outgoing policy | Allow |
| Default routed policy | Deny |
| Logging | On - low |

## Initial Finding

During the first firewall review, UFW had permissive inbound rules for SSH, WireGuard, and a development port.

The following rules were found:

| Rule | Initial Assessment |
|---|---|
| 22 | SSH open from anywhere |
| 51820/udp | WireGuard-style inbound rule, likely unnecessary for ProtonVPN client mode |
| 8000/tcp | Development server port open from anywhere |

These rules were removed during the baseline cleanup.

## Current UFW Rules

Current status: no explicit inbound allow rules.

The firewall now relies on the default policy:

| Direction | Policy |
|---|---|
| Incoming | Deny |
| Outgoing | Allow |
| Routed | Deny |

## Listening Services After Cleanup

| Port | Protocol | Bind Address | Process | Assessment |
|---:|---|---|---|---|
| 59641 | UDP | IPv4 and IPv6 wildcard | VPN-related | Expected while ProtonVPN WireGuard is active. |
| 38487 | TCP | 127.0.0.1 | containerd | Localhost-only listener. Not directly exposed to the network. |

## VPN Port Clarification

Port 51820/udp is commonly associated with WireGuard.

However, this workstation is currently acting as a ProtonVPN WireGuard client, not as a WireGuard server.

For this use case, an inbound UFW rule for 51820/udp is not required.

The VPN connection continues to work because UFW allows outgoing traffic by default and stateful firewall behavior permits related return traffic.

## SSH Status

SSH access may be configured later if needed.

At the current baseline stage, SSH is not exposed through UFW.

Future SSH configuration should follow these requirements:

1. Key-based authentication only.
2. Root login disabled.
3. Password login disabled if possible.
4. UFW restricted to trusted local networks or a trusted VPN path.
5. Fail2ban enabled for sshd if SSH is exposed.

## Container Runtime

The process initially appeared as contained in the socket output, but systemd identified it as containerd.

| Item | Value |
|---|---|
| Service | containerd.service |
| Description | containerd container runtime |
| Binary | /usr/bin/containerd |
| State | Active |
| Network exposure | Localhost-only listener observed |

## Current Assessment

The firewall is active, enabled at boot, and now has no explicit inbound allow rules.

This is an appropriate baseline for a personal Linux workstation.

Remaining follow-up work:

1. Review Fail2ban status.
2. Re-test listening sockets when ProtonVPN is disabled.
3. Decide whether SSH should be configured later.
4. Keep development ports closed unless actively needed.

## Sensitive Information Excluded

The following information must not be published:

- full socket dumps
- local IP addresses
- public IP addresses
- usernames
- process IDs if not needed
- raw firewall output

## Fail2ban Status

Fail2ban is installed, enabled at boot, and active.

| Item | Status |
|---|---|
| Service enabled | Yes |
| Service active | Yes |
| Active jails | 1 |
| Active jail name | sshd |
| Currently failed attempts | 0 |
| Total failed attempts | 0 |
| Currently banned IPs | 0 |
| Total banned IPs | 0 |

## Active Jail: sshd

The active Fail2ban jail is sshd.

The jail is configured to monitor SSH-related authentication activity through systemd journal matching.

Current jail status shows no failed attempts and no banned IPs.

## Fail2ban Configuration Notes

The local configuration file /etc/fail2ban/jail.local exists.

This indicates that local Fail2ban customization is being used instead of editing the default jail.conf directly.

This is the correct approach because jail.conf may be overwritten by package updates.

## Current Fail2ban Assessment

Fail2ban is working and enabled.

At the current stage, SSH is not exposed through UFW. Therefore, the sshd jail is not protecting an externally reachable SSH service right now.

However, keeping the sshd jail configured is useful if SSH is reintroduced later with stricter controls.

Future SSH exposure should only happen with:

1. Key-based authentication.
2. Root login disabled.
3. Password login disabled if possible.
4. UFW restricted to trusted sources.
5. Fail2ban sshd jail active.

## SSH and Fail2ban Relationship

The Fail2ban sshd jail is enabled, but the SSH server itself is currently disabled and inactive.

| Item | Status |
|---|---|
| sshd service enabled | No |
| sshd service active | No |
| Fail2ban sshd jail enabled | Yes |
| SSH exposed through UFW | No |

## sshd Jail Parameters

| Parameter | Value | Meaning |
|---|---|---|
| enabled | true | The sshd jail is enabled in Fail2ban. |
| port | ssh | Uses the SSH service port, normally 22. |
| maxretry | 5 | Ban after 5 failed attempts. |
| bantime | 3600 | Ban duration is 1 hour. |
| findtime | 600 | Failed attempts are counted within a 10-minute window. |

## Assessment

This is a safe current state.

SSH is not running and is not exposed through the firewall, while Fail2ban is already prepared to protect sshd if SSH is enabled later.

No immediate change is required.

If SSH is introduced in the future, it should be configured securely before opening any firewall rule.
