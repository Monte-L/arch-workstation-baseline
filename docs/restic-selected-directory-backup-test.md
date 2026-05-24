# Restic Selected Directory Backup Test

## Purpose

This document records a selected-directory encrypted backup test using Restic.

The goal was to validate backing up a real project directory while excluding private data and Git internals.

## Test Scope

The selected test directory was the workstation baseline project.

The backup used the Restic exclude policy file:

    backup/restic-excludes.txt

## Backup Tool

| Tool | Purpose |
|---|---|
| Restic | encrypted backup |
| restic-excludes.txt | exclude unsafe or unnecessary paths |

## Included Data

The selected backup included reviewed project content such as:

| Item | Included |
|---|---|
| README.md | yes |
| docs/ | yes |
| backup/ policy files | yes |
| selected public dotfiles | yes |

## Excluded Data

The test confirmed that the following were excluded:

| Item | Result |
|---|---|
| private/ directory | excluded |
| .git directory | excluded |

## Restore Validation

The backup was restored into a temporary directory under `/tmp`.

Validation results:

| Check | Result |
|---|---|
| README.md restored | pass |
| docs directory restored | pass |
| backup policy directory restored | pass |
| private directory excluded | pass |
| .git directory excluded | pass |

## Result

The selected-directory Restic backup test passed.

This confirms that Restic can back up a real project directory while respecting the exclude policy.

## Current Assessment

The encrypted backup workflow is now validated at three levels:

1. Small non-sensitive test backup.
2. USB backup of non-sensitive project/system inventory.
3. Selected real project directory backup with excludes.

## Remaining Work

1. Define password storage strategy.
2. Decide whether to back up selected private configuration.
3. Create a reviewed include list for real user data.
4. Avoid backing up browser profiles, VPN keys, SSH private keys, and tokens accidentally.
5. Repeat restore tests periodically.
