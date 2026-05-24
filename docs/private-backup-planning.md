# Private Backup Planning

## Purpose

This document defines the planning stage for future private encrypted backups.

No sensitive backup is executed at this stage.

The goal is to decide what private data may be backed up later using Restic, and what must remain excluded or require special handling.

## Current Status

The workstation backup workflow has already validated:

| Area | Status |
|---|---|
| USB non-sensitive backup | passed |
| Small restore test | passed |
| Restic encrypted test backup | passed |
| Restic check | passed |
| Selected project directory backup | passed |
| private/ exclusion test | passed |
| .git exclusion test | passed |
| Restic exclude policy | created |
| Restic password strategy | documented |

Sensitive backup execution is still blocked until the actual password storage method is confirmed.

## Private Backup Categories

| Category | Backup Decision | Notes |
|---|---|---|
| selected personal documents | allowed later | only after password storage is confirmed |
| selected private notes | allowed later | review before backup |
| selected private project files | allowed later | avoid secrets and raw outputs unless intentional |
| selected dotfiles | allowed later | only reviewed files |
| SSH public keys | allowed | public material only |
| SSH private keys | special handling | do not include by default |
| VPN configuration | special handling | do not include by default |
| NetworkManager private connections | special handling | may contain secrets |
| browser profiles | exclude by default | contains sessions, cookies, tokens |
| password databases | special handling | only if intentionally included |
| API keys/tokens | exclude by default | high risk |
| raw command outputs | exclude by default | may contain IPs, endpoints, usernames |

## Default Policy

The default private backup policy is conservative:

1. Do not back up full home directory.
2. Do not back up browser profiles.
3. Do not back up SSH private keys by default.
4. Do not back up VPN private keys by default.
5. Do not back up NetworkManager private connection files by default.
6. Do not back up token or environment files by default.
7. Include only reviewed directories and files.
8. Restore-test every new backup category before relying on it.

## Recommended First Private Backup Scope

The first real private backup should be small.

Recommended initial scope:

| Item | Include? |
|---|---|
| selected documents folder | yes, if reviewed |
| selected notes folder | yes, if reviewed |
| selected project folder without private/raw | yes |
| package and service inventories | yes |
| sanitized dotfiles | yes |
| full home directory | no |
| browser profile | no |
| SSH private keys | no |
| VPN configs | no |

## Special Handling Required

Some files may be important but dangerous to back up carelessly.

| Item | Recommended Handling |
|---|---|
| SSH private keys | separate encrypted backup or password manager note |
| WireGuard private keys | separate encrypted backup only if necessary |
| NetworkManager connection files | backup only after manual review |
| password database | backup separately and intentionally |
| recovery codes | store offline, not only digitally |

## Restore Testing Requirement

Every sensitive backup category should have a restore test.

Minimum restore test:

1. Restore to `/tmp`.
2. Confirm expected files exist.
3. Confirm excluded files are absent.
4. Confirm no secrets are unintentionally included.
5. Delete the temporary restore directory after review.

## Current Decision

Private backup planning is in progress.

No sensitive backup has been executed yet.

The next step is to create a local-only private include candidate list under `private/`.
