# Security Baseline

## Purpose

This document summarizes the current security posture of the Arch Linux workstation.

Detailed evidence and raw command outputs are stored locally under private/raw/ and must not be published.

## Baseline Summary

| Area | Status |
|---|---|
| Firewall | Active |
| Firewall frontend | UFW |
| Default incoming policy | Deny |
| Default outgoing policy | Allow |
| Explicit inbound allow rules | None |
| SSH service | Disabled and inactive |
| Fail2ban | Enabled and active |
| Active Fail2ban jail | sshd |
| VPN | ProtonVPN WireGuard active during test |
| VPN routing | Confirmed through policy routing |
| DNS | Managed by NetworkManager |
| Quad9 direct detection | Not detected directly |
| Dotfiles | Selected and sanitized |
| Private raw data | Excluded through .gitignore |
| Shell PATH | Corrected in zshrc |

## Firewall Posture

UFW is active and enabled.

The current firewall baseline uses:

- deny incoming
- allow outgoing
- deny routed

No explicit inbound allow rules remain after cleanup.

Previously open rules for SSH, WireGuard-style inbound traffic, and a development port were removed.

## SSH Posture

The SSH daemon is currently disabled and inactive.

SSH is not exposed through UFW.

Future SSH configuration should require:

1. key-based authentication
2. disabled root login
3. disabled password login if possible
4. restricted UFW source rules
5. active Fail2ban sshd jail

## Fail2ban Posture

Fail2ban is enabled and active.

The sshd jail is enabled with the following behavior:

| Parameter | Value |
|---|---|
| maxretry | 5 |
| bantime | 3600 |
| findtime | 600 |

Since sshd is not currently active, Fail2ban is prepared for future SSH use but is not protecting an exposed SSH service at this stage.

## VPN Posture

ProtonVPN through WireGuard was tested.

The active interface was detected as vpn0.

Although the main routing table showed Wi-Fi as the default route, policy routing selected the VPN interface for public IPv4 traffic.

This confirms that VPN routing was active during the test.

## DNS Posture

DNS is managed by NetworkManager through /etc/resolv.conf.

Quad9 public resolver addresses were not detected directly in /etc/resolv.conf during the initial check.

DNS behavior still requires further validation, especially while the VPN is active.

## Dotfiles Security

Dotfiles were not copied as a full ~/.config dump.

Only selected configuration files were copied into the public dotfiles directory.

The public dotfiles were reviewed and cleaned.

Actions performed:

- removed backup files
- sanitized wallpaper paths
- sanitized VPN aliases
- avoided browser data
- avoided VPN configuration files
- avoided SSH keys
- avoided tokens and secrets
- ran a critical sensitive scan

## Shell PATH Issue

During review, the shell PATH was found to be misconfigured.

The PATH pointed to the bash profile file instead of standard executable directories.

This caused basic commands such as cat, sed, and date to fail.

A safe PATH baseline was added to zshrc.

## Current Security Assessment

The workstation now has a reasonable personal security baseline.

The most important improvements completed were:

1. firewall activation and cleanup
2. removal of unnecessary inbound rules
3. confirmation that SSH is not exposed
4. confirmation that Fail2ban is active
5. VPN routing validation
6. dotfiles sanitization
7. shell PATH correction
8. private raw data separation

## Remaining Work

| Item | Priority | Status |
|---|---|---|
| DNS leak testing | High | Open |
| Quad9 enforcement decision | Medium | Open |
| Package inventory | Medium | Open |
| Enabled systemd services review | Medium | Open |
| Backup strategy | High | Open |
| SSH hardening plan | Medium | Future |
| Public GitHub review | Medium | Future |
