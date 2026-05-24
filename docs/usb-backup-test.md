# USB Backup Test

## Purpose

This document records the first USB backup and restore test for the Arch Linux workstation.

The goal was to validate a simple non-sensitive backup workflow before enabling more persistent hardening changes.

## Backup Medium

A USB flash drive was used for the first test.

The device was treated as non-encrypted removable storage.

Because of that, only non-sensitive material was included.

## Included Data

The test backup included:

| Item | Included |
|---|---|
| explicit package list | yes |
| foreign/AUR package list | yes |
| enabled services list | yes |
| running services list | yes |
| public tracked project snapshot | yes |
| Git log/status snapshot | yes |
| checksums | yes |

## Excluded Data

The backup intentionally excluded:

- WireGuard private keys
- NetworkManager private connection files
- SSH private keys
- browser profiles
- tokens
- passwords
- API keys
- VPN endpoints
- raw private command outputs
- personal sensitive documents

## Backup Method

The public project backup was created using `git archive`.

This means only tracked repository files were included.

Ignored files such as `private/` were not included.

## Restore Test

A small restore test was performed by extracting the archived project into a temporary directory.

The restore test checked for:

| Item | Result |
|---|---|
| README.md restored | pass |
| docs directory restored | pass |
| dotfiles directory restored | pass |

## Checksums

SHA256 checksums were generated for the backup files.

This allows later integrity verification.

## Unmount

The USB drive was synchronized and unmounted after the test.

## Result

The first USB backup and small restore test passed.

This validates the basic backup workflow for non-sensitive project and system inventory data.

## Remaining Work

1. Choose whether to use encrypted backup for sensitive material.
2. Test restoring package lists.
3. Test restoring selected dotfiles.
4. Consider restic or borg for encrypted backups.
5. Repeat backup test with a larger dataset.
