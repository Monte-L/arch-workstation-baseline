# Restic Backup Policy

## Purpose

This document defines the initial include and exclude policy for Restic-based workstation backups.

The goal is to prevent accidental backup of unnecessary, unsafe, or highly sensitive files before creating larger real backups.

## Current Status

Restic encrypted backup testing has passed with non-sensitive data.

Before backing up real workstation data, an include/exclude policy is required.

## Backup Categories

| Category | Status | Notes |
|---|---|---|
| Non-sensitive project archives | Allowed | Safe for initial backups |
| Package lists | Allowed | Useful for rebuild |
| Service lists | Allowed | Useful for restore validation |
| Sanitized dotfiles | Allowed | Already reviewed for public repository |
| Private dotfiles | Caution | Only with encryption and review |
| VPN configs | Sensitive | Only encrypted, never public |
| SSH private keys | Sensitive | Prefer separate secure handling |
| Browser profiles | Sensitive/heavy | Exclude by default |
| Raw command outputs | Sensitive | Exclude from public backups unless encrypted and intentional |
| Personal documents | Sensitive | Only after encryption/password strategy |

## Initial Include Policy

The first real backup should include only reviewed data:

| Path Type | Include? | Notes |
|---|---|---|
| selected project repositories | yes | Prefer tracked/sanitized repositories first |
| package inventory exports | yes | Generated text files |
| service inventory exports | yes | Generated text files |
| restore notes | yes | Documentation |
| selected dotfiles | yes | Only reviewed files |
| full home directory | no | Too broad for initial backup |

## Initial Exclude Policy

The Restic exclude file is stored at:

    backup/restic-excludes.txt

It excludes common unsafe or unnecessary paths such as:

- caches
- browser profiles
- trash
- build artifacts
- temporary files
- VM/disk images
- key files
- token files
- environment files
- WireGuard private configs
- raw private evidence

## Sensitive Data Policy

Sensitive data should only be backed up after the following are true:

1. Restic password storage strategy is defined.
2. Backup destination is trusted.
3. Restore test has already passed.
4. Include/exclude file has been reviewed.
5. Sensitive paths are backed up intentionally, not accidentally.

## Password Policy

The Restic repository password must not be:

- committed to Git
- stored in plaintext in the repository
- pasted into chat
- stored inside the backup repository
- included in documentation screenshots

If the password is lost, the encrypted backup cannot be restored.

## Recommended Next Backup Test

The next test should use Restic with:

- the exclude file
- a small selected real directory
- no private keys
- no browser data
- no VPN config
- restore into `/tmp`
- integrity check after backup

## Current Decision

Restic remains the preferred encrypted backup tool.

The next step is to perform a small selected-directory backup using the exclude file.
