# Risk Register

## Purpose

This document tracks security, privacy, operational, and configuration risks identified during the workstation baseline review.

## Risk Table

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-001 | VPN routing required validation | Medium | Mitigated | Active VPN test confirmed ProtonVPN WireGuard interface protonch and public IPv4 route decision through VPN policy table 51820. |
| R-002 | Quad9 DNS not detected directly in /etc/resolv.conf | Medium | Open | DNS is managed by NetworkManager. Need to confirm whether DNS is handled by Wi-Fi, VPN, Quad9, or another resolver. |
| R-003 | Raw network and system outputs may contain sensitive information | High | Mitigated | Raw outputs are stored under private/raw/, and private/ is excluded through .gitignore. |
| R-004 | Dotfiles may contain private paths, usernames, tokens, or machine-specific data | High | Open | Dotfiles must be reviewed before being copied into the public project structure. |
| R-005 | WireGuard public keys and VPN endpoints may appear in command output | High | Mitigated | Raw VPN output must remain private. Public documentation only records sanitized behavior and routing conclusions. |
| R-006 | DNS behavior while VPN is active has not been fully validated | Medium | Open | Need DNS leak testing and resolver confirmation while ProtonVPN is active. |

## Severity Guide

| Severity | Meaning |
|---|---|
| Low | Minor issue or documentation improvement |
| Medium | Security or privacy issue requiring validation |
| High | Could expose sensitive data or weaken system security |
| Critical | Immediate risk of credential exposure, compromise, or data loss |

## Current Priority

The next priorities are:

1. Verify DNS behavior while VPN is active.
2. Review firewall status.
3. Review listening services.
4. Review Fail2ban status.
5. Review dotfiles before copying anything into the public folder.

## Firewall Cleanup Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-011 | Inbound SSH rule was open from anywhere | Medium | Mitigated | Removed UFW allow rule for port 22. SSH can be reintroduced later with stricter controls. |
| R-012 | Inbound development port 8000 was open from anywhere | Medium | Mitigated | Removed UFW allow rule for port 8000/tcp. |
| R-013 | Inbound WireGuard port 51820/udp was open from anywhere | Medium | Mitigated | Removed UFW allow rule for 51820/udp. Not required for current ProtonVPN client mode. |
| R-014 | Firewall had permissive inbound rules after initial setup | Medium | Mitigated | UFW now has no explicit inbound allow rules and uses default deny incoming policy. |

## Fail2ban Review Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-015 | Fail2ban service misconfiguration | Medium | Mitigated | Fail2ban is enabled, active, and running one jail for sshd. |
| R-016 | SSH may be reintroduced later without strict controls | Medium | Open | If SSH is enabled in the future, it should require key-based authentication, restricted UFW source rules, and active Fail2ban protection. |

## SSH and Fail2ban Follow-Up

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-017 | SSH service exposure | Medium | Mitigated | sshd is disabled and inactive, and UFW has no inbound SSH allow rule. |
| R-018 | Fail2ban sshd jail active while SSH is inactive | Low | Documented | This is not a current exposure. The jail is ready if SSH is enabled later. |

## Package and Service Review Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-019 | Docker is enabled and running by default | Medium | Open | Docker is useful for lab and development work, but increases local attack surface. Decide whether it should start at boot or be started manually. |
| R-020 | AUR package usage requires manual trust review | Medium | Open | The system has 11 foreign/AUR packages. New AUR packages should be reviewed before installation. |
| R-021 | tor-browser-alpha-bin is alpha software | Low | Open | Alpha software may be less stable than a standard release. Review whether alpha build is necessary. |
| R-022 | yay-debug may be unnecessary | Low | Open | Debug package should be removed if not needed. |
| R-023 | wireguard-dkms may be unnecessary | Low | Open | Review whether DKMS package is required with the current kernel and WireGuard setup. |
| R-024 | NetworkManager-wait-online.service may delay boot | Low | Open | Review whether this service is needed for the workstation workflow. |

## VPN Kill Switch Review

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-025 | Outbound traffic is allowed before VPN activation | Medium | Open | UFW currently allows outgoing traffic by default. This means traffic may leave through Wi-Fi before ProtonVPN is active. A VPN kill switch should be evaluated. |
| R-026 | VPN autostart may fail if Wi-Fi is not ready | Low | Open | WireGuard can be started automatically, but on a laptop using Wi-Fi it may require NetworkManager integration or dispatcher scripts. |

## Service Hardening Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-027 | Docker service running continuously | Medium | Mitigated | docker.service was disabled and stopped. Docker can be started manually when needed. |
| R-028 | Docker socket could trigger Docker automatically | Medium | Mitigated | docker.socket was disabled and stopped to prevent socket activation. |
| R-029 | containerd listener remained active after Docker service stop | Low | Mitigated | containerd.service was stopped and is now inactive during normal workstation use. |
