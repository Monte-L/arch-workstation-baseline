# Dotfiles Guide

## Purpose

This document records the dotfiles selected for public documentation and the review process used to avoid publishing private or machine-specific data.

## Initial Dotfiles Review

A dotfiles inventory was collected from the user's home configuration directory.

The full raw inventory is stored under private/raw/ and must not be published.

## Candidate Dotfiles

| Dotfile | Status | Notes |
|---|---|---|
| Hyprland | Review required | May contain wallpaper paths and local commands. |
| Hyprpaper | Review required | Contains local wallpaper paths. |
| Waybar | Review required | May contain scripts or local paths. |
| Kitty | Candidate for public copy | Usually safe after review. |
| Rofi | Candidate for public copy | Theme variables produced false positives in scan. |
| Starship | Candidate for public copy | Prompt symbols produced false positives in scan. |
| Neovim | Review required | Contains plugin bootstrap URL. |
| Zsh | Review required | Contains aliases and required PATH correction. |
| Bash | Low priority | Minimal configuration observed. |

## PATH Issue Found

During the dotfiles review, the shell PATH was found to be misconfigured.

The observed PATH value pointed to the bash profile file instead of standard binary directories.

This caused basic commands such as cat, sed, and date to fail in the shell.

A safe PATH baseline was added to the top of .zshrc.

## Public Dotfile Rules

The public repository must not include:

- VPN configuration files
- WireGuard private keys
- SSH private keys
- tokens or API keys
- personal email addresses
- machine-specific secrets
- raw application data
- browser configuration
- full .config directory dumps

Dotfiles should be copied selectively and sanitized before publication.

## Public Dotfiles Cleanup

The first public dotfiles copy was reviewed before publication.

Findings and actions:

| Finding | Action |
|---|---|
| Hyprland backup file present | Removed from public dotfiles directory |
| Personal wallpaper paths | Replaced with generic placeholder path |
| VPN profile name in shell aliases | Replaced with generic placeholder profile name |
| Rofi theme variables flagged by scan | Documented as false positives |
| Starship username/hostname format flagged by scan | Documented as expected prompt formatting |
| Critical sensitive scan | No critical findings after cleanup |

The public dotfiles directory now contains only selected, reviewed configuration files.

## Zsh Initialization Fix

A post-reboot shell issue was identified where terminal colors and syntax highlighting did not initialize correctly after boot.

### Finding

The zsh syntax highlighting plugin was being loaded before the custom highlight styles were defined.

This could cause colors to behave inconsistently when opening a new terminal after boot.

### Action

The zsh configuration was reorganized.

The corrected order is:

1. Safe PATH baseline.
2. Color support.
3. Autosuggestion style.
4. Syntax highlighting styles.
5. Aliases and helpers.
6. Starship prompt initialization.
7. Zsh plugins loaded near the end.

### Result

The shell initialization order is cleaner and more reliable.

The public zshrc was also sanitized to avoid exposing personal wallpaper/logo paths or VPN profile names.
