# System Inventory

## Purpose

This document records a sanitized overview of the current Arch Linux workstation.

Raw command outputs are stored locally under `private/raw/` and must not be published.

## Collection Date

- Initial collection: 2026-05-24

## System Summary

| Item | Value |
|---|---|
| Operating System | Arch Linux |
| Kernel | Linux 7.0.9-arch2-1 |
| Architecture | x86_64 |
| Device Type | Laptop |
| Vendor | Lenovo |
| Model | ThinkPad T480 |
| Window Manager | Hyprland |
| Terminal | Kitty |
| Application Launcher | Rofi |
| Status Bar | Waybar |

## Hardware Summary

| Component | Details |
|---|---|
| CPU | Intel Core i5-8250U @ 1.60GHz |
| CPU Cores | 4 |
| CPU Threads | 8 |
| Virtualization | VT-x |
| Memory | 15 GiB |
| Swap | 15 GiB |
| Firmware | UEFI |
| Firmware Vendor | Lenovo |

## Storage

| Device / Partition | Size | Filesystem | Mount Point | Purpose |
|---|---:|---|---|---|
| nvme0n1 | 238.5 GiB | - | - | Primary NVMe disk |
| nvme0n1p1 | 1 GiB | vfat | /boot/efi | EFI System Partition |
| nvme0n1p2 | 16 GiB | swap | [SWAP] | Swap partition |
| nvme0n1p3 | 221.5 GiB | ext4 | / | Root filesystem |

### Disk Usage at Initial Collection

| Filesystem | Type | Size | Used | Available | Use% | Mount Point |
|---|---|---:|---:|---:|---:|---|
| /dev/nvme0n1p3 | ext4 | 217G | 16G | 191G | 8% | / |
| /dev/nvme0n1p1 | vfat | 1022M | 168K | 1022M | 1% | /boot/efi |

A secondary disk entry appeared as `sda` with 0B and no active mount point during collection. It is not documented as active storage.

## Graphics

| Component | Details |
|---|---|
| Integrated GPU | Intel Corporation Kaby Lake-R GT2 [UHD Graphics 620] |

Note: an NVMe storage controller appeared in the raw graphics command output because the PCI address started with `3d`. This was a grep match artifact, not a graphics device.

## Network Interfaces

To be documented in the network baseline.

Public documentation should avoid exposing sensitive interface details, public IP addresses, VPN endpoints, or private network identifiers.

## Running Services

To be reviewed before publishing.

Only relevant services should be documented publicly. Full raw service output must remain private.

## Sensitive Information Excluded

The following information must not be published:

- Machine ID
- Boot ID
- VPN private keys
- WireGuard configuration files
- Public IP addresses
- VPN endpoints
- Personal usernames
- Tokens, passwords, or secrets
- Full raw service dumps
- Full raw network dumps

## Notes

This inventory represents the initial state of the workstation before deeper security validation.

Further documents will cover:

- firewall configuration
- VPN and DNS behavior
- Fail2ban configuration
- dotfile organization
- risk register
