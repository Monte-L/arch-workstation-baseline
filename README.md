# Arch Linux Secure Workstation Baseline

## Overview

This project documents the configuration, security baseline, dotfiles, and operational setup of a personal Arch Linux workstation.

The goal is to build a clean, reproducible, and well-documented Linux workstation environment focused on:

- system transparency
- secure defaults
- network awareness
- firewall and service validation
- VPN and DNS verification
- public dotfile organization
- private data separation
- practical infrastructure documentation

This is not intended to be a fully hardened enterprise workstation.

It is a personal learning and portfolio project focused on understanding, documenting, and improving a real Linux environment step by step.

## Current Environment

- Operating System: Arch Linux
- Window Manager: Hyprland
- Terminal: Kitty
- Application Launcher: Rofi
- Status Bar: Waybar
- Firewall: UFW
- VPN: WireGuard with ProtonVPN
- DNS: Quad9
- Intrusion Prevention: Fail2ban

## Project Goals

1. Document the current system state.
2. Separate public dotfiles from private application data.
3. Validate firewall rules and exposed services.
4. Verify VPN routing and DNS behavior.
5. Review SSH security and key handling.
6. Document Fail2ban configuration and protected services.
7. Identify remaining risks in a risk register.
8. Build a safe public version of the configuration for portfolio use.

## Scope

### In Scope

- Arch Linux workstation documentation
- Hyprland-related dotfiles
- Shell and terminal configuration
- Firewall baseline
- VPN and DNS validation
- Fail2ban review
- Security checklist
- Risk register
- Backup planning

### Out of Scope

- Publishing private VPN configuration
- Publishing secrets, tokens, keys, or passwords
- Advanced enterprise hardening
- Disk encryption migration
- AppArmor or auditd deployment at this stage

## Documentation Structure

- docs/system-inventory.md
- docs/security-baseline.md
- docs/network-baseline.md
- docs/vpn-dns-baseline.md
- docs/firewall-fail2ban.md
- docs/package-service-inventory.md
- docs/dotfiles-guide.md
- docs/risk-register.md
- docs/project-log.md

## Current Status

Project initialized.

Next step: collect the first system inventory and document the current workstation state.
