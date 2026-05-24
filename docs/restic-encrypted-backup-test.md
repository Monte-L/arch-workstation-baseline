# Restic Encrypted Backup Test

## Purpose

This document records the first encrypted backup and restore test using Restic on removable USB storage.

The goal was to validate encrypted backup behavior before storing any sensitive workstation data.

## Backup Medium

A USB flash drive was used as the backup destination.

The USB filesystem itself was treated as non-encrypted removable storage.

Restic provided encryption at the backup repository level.

## Tool

| Tool | Version |
|---|---|
| restic | 0.18.1 |

## Test Scope

This was a small non-sensitive test.

The test included:

| Item | Included |
|---|---|
| test README file | yes |
| nested test file | yes |
| sensitive data | no |
| VPN keys | no |
| SSH keys | no |
| browser data | no |
| tokens/passwords | no |

## Test Workflow

The test performed the following actions:

1. Created a Restic repository on the USB drive.
2. Created non-sensitive test data under `/tmp`.
3. Created an encrypted Restic backup snapshot.
4. Listed snapshots.
5. Ran repository integrity check.
6. Restored the latest snapshot into `/tmp`.
7. Verified restored files.
8. Checked repository size.

## Validation Results

| Check | Result |
|---|---|
| Restic repository initialized | pass |
| Backup snapshot created | pass |
| Snapshot listed | pass |
| Repository check | pass |
| README test file restored | pass |
| Nested file restored | pass |

## Restore Test

The restore test confirmed:

| Restored Item | Result |
|---|---|
| README-test.txt | pass |
| sample-dir/package-note.txt | pass |

## Repository Integrity

Restic check completed with no errors.

## Repository Size

The test repository size was small, approximately a few megabytes.

## Security Notes

The Restic repository is encrypted.

The backup password must be stored securely.

If the password is lost, the backup cannot be restored.

The password must not be committed to Git, stored in plaintext, or shared in public documentation.

## Current Assessment

The encrypted USB backup workflow is viable.

Restic is a good candidate for future private backups involving sensitive data, provided the password is managed securely.

## Remaining Work

1. Decide password storage method.
2. Define real backup include/exclude paths.
3. Create an exclude file for caches and unsafe directories.
4. Test restoring selected real dotfiles.
5. Consider backing up private configuration only after confirming encryption workflow.
6. Repeat integrity checks periodically.
