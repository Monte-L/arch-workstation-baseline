# Enabled Services Review

## Purpose

This document records the review of enabled systemd units, sockets, timers, and selected running services.

Raw command outputs are stored locally under private/raw/ and must not be published.

## Enabled Unit Files

| Unit | State | Assessment |
|---|---|---|
| fail2ban.service | enabled | Keep. Required for intrusion prevention baseline. |
| getty@.service | enabled | Keep. Standard Linux virtual console service. |
| NetworkManager-dispatcher.service | enabled | Keep. Supports NetworkManager dispatcher scripts. |
| NetworkManager.service | enabled | Keep. Required for network management. |
| sddm.service | enabled | Keep. Graphical login manager. |
| ufw.service | enabled | Keep. Required for firewall baseline. |
| systemd-userdbd.socket | enabled | Keep. Standard systemd user database socket. |
| remote-fs.target | enabled | Documented. No issue by itself. |

## Enabled Sockets

| Socket | State | Assessment |
|---|---|---|
| systemd-userdbd.socket | enabled | Keep. Standard systemd socket. |

## Enabled Timers

No enabled timer unit files were listed during this review.

## Selected Running Services

| Service | Status | Assessment |
|---|---|---|
| fail2ban.service | running | Expected |
| NetworkManager.service | running | Expected |
| polkit.service | running | Expected for desktop authorization |
| sddm.service | running | Expected graphical display manager |
| systemd-udevd.service | running | Expected device event manager |
| systemd-userdbd.service | running | Expected systemd user database manager |
| user@1000.service | running | Expected user session service |

## Active Timers Observed

| Timer | Assessment |
|---|---|
| shadow.timer | Normal system maintenance |
| systemd-tmpfiles-clean.timer | Normal temporary file cleanup |
| archlinux-keyring-wkd-sync.timer | Normal Arch Linux keyring maintenance |

## Not Observed as Enabled

The following services were not observed as enabled during the selected review:

- docker.service
- docker.socket
- containerd.service
- sshd.service
- bluetooth.service
- cups.service
- avahi-daemon.service
- smb.service
- nmb.service
- systemd-resolved.service

## SSH Socket Note

An sshd-related local Unix socket was observed in the active socket list.

This is not the same as exposing SSH over TCP port 22.

At this stage:

- sshd.service is disabled and inactive
- UFW has no inbound SSH allow rule
- no SSH network exposure was documented

## Current Assessment

The enabled service set is minimal and appropriate for a personal Arch Linux workstation.

No unnecessary network-facing services were found enabled during this review.

Docker and containerd were previously disabled from autostart and are no longer part of the normal boot service footprint.

## Follow-Up

Recommended future checks:

1. Re-check services after reboot.
2. Confirm Docker/containerd remain inactive after reboot.
3. Confirm NetworkManager-wait-online remains disabled after reboot.
4. Re-run socket review after VPN on and VPN off states.
5. Continue with package cleanup review.
