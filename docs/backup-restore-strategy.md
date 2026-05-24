# Backup and Restore Strategy

## Purpose

This document defines the backup and restore strategy for the Arch Linux workstation.

The goal is to make the workstation recoverable before applying more advanced or persistent hardening changes.

## Current Context

The workstation has already completed:

| Area | Status |
|---|---|
| Initial system baseline | completed |
| Firewall baseline | completed |
| Docker/containerd cleanup | completed |
| NetworkManager WireGuard validation | completed |
| DNS policy | completed |
| External DNS leak test | completed |
| Manual VPN kill switch test | passed |
| Persistent kill switch | not enabled yet |

Before enabling persistent kill switch behavior, a backup and restore strategy should be prepared.

## Backup Goals

The backup strategy should allow recovery of:

- important user documents
- project repositories
- selected dotfiles
- package list
- service state
- NetworkManager configuration notes
- UFW baseline notes
- restore procedure
- workstation rebuild checklist

## What Should Be Backed Up

| Item | Priority | Notes |
|---|---|---|
| project repositories | High | Includes workstation baseline and other technical projects |
| selected dotfiles | High | Only reviewed and intentional configuration |
| documents | High | Personal documents should be backed up privately |
| package list | High | Useful for rebuild |
| enabled services list | Medium | Useful for restoring system state |
| UFW baseline notes | Medium | Already documented, but useful in restore |
| NetworkManager notes | Medium | Do not publish private connection files |
| shell helper scripts | Medium | Useful for VPN/firewall workflow |

## What Should Not Be Published

The following items may be backed up privately if needed, but must not be published:

- WireGuard private keys
- NetworkManager private connection files
- SSH private keys
- browser profiles
- tokens
- passwords
- API keys
- raw command outputs
- VPN endpoints
- public IP records
- exact local network details

## Backup Tool Options

| Tool | Purpose | Notes |
|---|---|---|
| git | version control for projects and sanitized dotfiles | already in use |
| rsync | file-level backup | simple and transparent |
| tar | archive creation | useful for snapshots |
| restic | encrypted backup | good future option |
| borg | encrypted deduplicated backup | good future option |
| pacman -Qqe | package inventory | useful for rebuild |
| systemctl list-unit-files | service inventory | useful for rebuild |

## Recommended Initial Model

The first practical backup model should be simple:

1. Keep public projects in GitHub.
2. Keep private raw evidence only locally or in encrypted backup.
3. Back up selected home directories using rsync.
4. Export package list.
5. Export enabled services list.
6. Store restore instructions in this repository.
7. Test restoring a small sample directory.

## Suggested Private Backup Structure

Suggested backup destination structure:

| Directory | Purpose |
|---|---|
| workstation-backup/packages/ | package lists |
| workstation-backup/services/ | service state |
| workstation-backup/dotfiles/ | selected private dotfiles if needed |
| workstation-backup/projects/ | project repositories |
| workstation-backup/documents/ | user documents |
| workstation-backup/restore/ | restore notes |

## Restore Priorities

If the system needs to be rebuilt, restore in this order:

1. Install base Arch system.
2. Install network tools and NetworkManager.
3. Restore package list.
4. Restore essential dotfiles.
5. Restore firewall baseline.
6. Restore VPN configuration privately.
7. Restore project repositories.
8. Validate DNS policy.
9. Validate UFW and Fail2ban.
10. Validate VPN routing.
11. Validate backup integrity.

## Current Status

Backup strategy drafted.

No full backup has been executed yet.

Next step: create a private backup inventory snapshot and choose a backup destination.
