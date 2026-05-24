# Restic Password Strategy

## Purpose

This document defines the password handling strategy for Restic encrypted backups.

Restic encryption is only useful if the repository password is protected.

If the password is lost, the backup cannot be restored.

If the password is exposed, the backup should be considered compromised.

## Current Status

Restic encrypted backup testing has passed.

Completed backup tests:

| Test | Result |
|---|---|
| Non-sensitive encrypted backup | pass |
| Restic check | pass |
| Restore test | pass |
| Selected-directory backup with excludes | pass |
| private/ exclusion validation | pass |
| .git exclusion validation | pass |

Sensitive backups should not begin until password handling is defined.

## Password Requirements

The Restic password should be:

- long
- unique
- not reused from other accounts
- not stored in the repository
- not pasted into chat
- not included in screenshots
- not saved inside the backup destination
- recoverable by the owner

## Storage Options

| Option | Security | Recovery | Notes |
|---|---|---|---|
| memorize only | medium | risky | easy to lose |
| paper copy | good | good | store physically and privately |
| password manager | good | good | depends on password manager security |
| plaintext file | poor | good | not recommended |
| saved in Git repo | unacceptable | unacceptable | never do this |
| saved on same USB as backup | poor | medium | defeats part of the protection |

## Recommended Strategy

The recommended starting strategy is:

1. Use a strong unique Restic password.
2. Store it in a trusted password manager or secure offline note.
3. Keep one offline emergency recovery copy.
4. Do not store it in Git.
5. Do not store it inside the Restic repository.
6. Do not include it in documentation.
7. Test restore while the password is still known.
8. Review recovery access before backing up sensitive data.

## Operational Rule

Before running real sensitive backups, confirm:

| Requirement | Status |
|---|---|
| Password created | required |
| Password stored securely | required |
| Emergency recovery copy prepared | recommended |
| Restore test completed | required |
| Exclude policy reviewed | required |
| Backup destination trusted | required |

## Sensitive Backup Readiness

Sensitive backups may include:

- selected private dotfiles
- selected documents
- selected configuration notes
- carefully reviewed private project data

Sensitive backups should not include by default:

- browser profiles
- raw SSH private keys without separate decision
- raw VPN private keys without separate decision
- unreviewed full home directory
- tokens
- password files
- cache directories

## Current Decision

Restic is approved as the preferred encrypted backup tool.

However, sensitive backup execution is blocked until password storage strategy is confirmed.

## Remaining Work

1. Decide where the Restic password will be stored.
2. Create a reviewed include list for private data.
3. Test restoring selected private configuration.
4. Consider periodic Restic check.
5. Consider a second backup destination later.

## Selected Password Storage Direction

The selected password storage direction is:

| Layer | Decision |
|---|---|
| Primary storage | trusted password manager |
| Emergency recovery | offline written recovery copy |
| Git repository storage | forbidden |
| Same USB backup drive storage | forbidden |
| Plaintext local file | forbidden |

The actual Restic password is not recorded in this repository.

The password must never be included in screenshots, command logs, public documentation, or copied into chat.

## Sensitive Backup Readiness Update

Sensitive backups may only begin after:

1. the Restic password is saved in the selected password manager;
2. an offline recovery copy exists;
3. the password has been tested with `restic snapshots`;
4. the include/exclude policy has been reviewed again.
