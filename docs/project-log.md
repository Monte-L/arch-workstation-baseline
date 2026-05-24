# Project Log

## Day 1 - Foundation and Initial Baseline

### Completed

- Created project structure.
- Created initial README.
- Created private raw collection directory.
- Collected sanitized system inventory.
- Documented hardware, storage, graphics, and operating system baseline.
- Reviewed network configuration.
- Verified NetworkManager DNS handling.
- Tested ProtonVPN WireGuard routing.
- Confirmed VPN policy routing through interface vpn0.
- Enabled and validated UFW.
- Removed unnecessary inbound firewall rules.
- Confirmed no explicit inbound allow rules remain.
- Reviewed listening services.
- Identified containerd as localhost-only listener.
- Reviewed Fail2ban status.
- Confirmed Fail2ban is enabled and active.
- Confirmed sshd jail is configured.
- Confirmed sshd service is disabled and inactive.
- Reviewed dotfiles candidates.
- Fixed broken shell PATH.
- Copied selected public dotfiles.
- Removed backup files from public dotfiles.
- Sanitized wallpaper paths.
- Sanitized VPN aliases.
- Ran critical sensitive scan on public dotfiles.

### Main Findings

| Area | Finding | Result |
|---|---|---|
| Shell | PATH was misconfigured | Fixed in zshrc |
| Firewall | UFW had open inbound rules | Rules removed |
| VPN | ProtonVPN uses policy routing | Confirmed |
| DNS | Quad9 not directly detected | Needs follow-up |
| SSH | sshd disabled and inactive | Safe current state |
| Fail2ban | sshd jail active | Ready for future SSH use |
| Dotfiles | Public copy required cleanup | Sanitized |

### Current Status

The workstation now has a documented initial baseline with sanitized public dotfiles and no explicit inbound firewall allow rules.

### Next Steps

1. Review DNS behavior while VPN is active.
2. Decide whether Quad9 should be enforced outside VPN.
3. Review package list.
4. Review enabled systemd services.
5. Prepare local Git commit.
6. Review repository before any public upload.

## Service Hardening Update

### Completed

- Disabled docker.service from starting automatically.
- Disabled docker.socket to prevent automatic socket activation.
- Stopped containerd.service after Docker cleanup.
- Confirmed previous containerd localhost listener was no longer observed.
- Preserved Docker as an installed tool for manual use during infrastructure labs.

### Result

Docker is now installed but not always running.

This reduces the default workstation service footprint while keeping Docker available for lab and development work.

## Post-Hardening Services Snapshot

A post-hardening services snapshot was collected after Docker and containerd cleanup.

### Expected State

- docker.service disabled and inactive
- docker.socket disabled and inactive
- containerd.service disabled and inactive
- WireGuard tools still available
- UFW still enabled
- Fail2ban still enabled
- NetworkManager still enabled
- No unnecessary Docker/containerd listener during normal workstation use

### Notes

Docker remains installed and can be started manually when needed for infrastructure labs.

This keeps the workstation lighter during normal use while preserving lab functionality.

## Enabled Services Full Review

### Completed

- Reviewed enabled systemd unit files.
- Reviewed enabled sockets.
- Reviewed enabled timers.
- Reviewed selected running services.
- Reviewed active timers.
- Confirmed no enabled Docker, containerd, SSH, CUPS, Avahi, Samba, Bluetooth, or systemd-resolved services were observed.
- Documented sshd-related local Unix socket as not equivalent to network-exposed SSH.

### Result

The enabled service set is minimal and appropriate for a personal Arch Linux workstation.

No obvious unnecessary network-facing services were found enabled.

## Package Cleanup Final Review

### Completed

- Confirmed yay-debug is not installed.
- Confirmed wireguard-dkms is not installed.
- Confirmed wg and wg-quick remain available.
- Reviewed orphan packages.
- Removed eww-debug.
- Kept Go as a documented review item.
- Kept tor-browser-alpha-bin as a documented review item.

### Result

The package set is cleaner and unnecessary debug packages were removed or confirmed absent.

Remaining package review items are Go and tor-browser-alpha-bin.

## DNS Behavior Review

### Completed

- Tested DNS behavior with ProtonVPN active.
- Confirmed VPN-active public route through vpn0 table 51820.
- Confirmed Quad9 direct detection during VPN-active test.
- Tested DNS behavior with ProtonVPN inactive.
- Confirmed VPN-off public route through Wi-Fi.
- Confirmed Quad9 was not directly detected during VPN-off test.
- Documented that DNS behavior differs between VPN-on and VPN-off states.

### Result

VPN-active state is preferred.

VPN-off state requires further review because normal traffic returns to Wi-Fi and Quad9 was not directly detected.

Future hardening should evaluate VPN kill switch or DNS enforcement after the inventory phase is complete.

## Post-Reboot VPN Validation

### Finding

After reboot, the first manual VPN startup attempt failed because wg-quick could not update DNS through resolvconf cleanly.

Running resolvconf update before starting WireGuard allowed the VPN to connect successfully.

### Temporary Action

A local vpn-on-safe helper was created.

The helper performs:

1. resolvconf update
2. WireGuard startup
3. route verification

### Decision

This is documented as a temporary workaround.

The preferred long-term design is NetworkManager-managed WireGuard with explicit DNS policy and VPN leak protection.

## Zsh Dotfile Initialization Fix

### Completed

- Reviewed the real zshrc after post-reboot shell color issue.
- Identified plugin loading order as the likely cause.
- Reordered zsh configuration.
- Kept syntax highlighting plugin near the end of the file.
- Preserved safe PATH baseline.
- Preserved vpn-on-safe and vpn-off-safe aliases.
- Updated the public sanitized zshrc.

### Result

The shell configuration is now cleaner, more reliable after boot, and safer for public documentation.

## Post-Reboot Validation Completed

### Completed

- Rebooted the workstation after service hardening.
- Confirmed UFW remained active.
- Confirmed Fail2ban remained active.
- Confirmed sshd remained inactive.
- Confirmed Docker services remained disabled and inactive.
- Confirmed containerd remained disabled and inactive.
- Confirmed NetworkManager-wait-online remained disabled and inactive.
- Confirmed VPN routing worked after vpn-on-safe workaround.
- Confirmed Quad9 DNS was detected after VPN activation.

### Result

The post-reboot validation passed.

The workstation retained the intended baseline state after reboot.

## NetworkManager WireGuard Evaluation

### Completed

- Reviewed NetworkManager WireGuard readiness.
- Confirmed NetworkManager is enabled and active.
- Confirmed wg and wg-quick remain available.
- Imported a WireGuard profile into NetworkManager.
- Tested manual VPN activation through NetworkManager.
- Tested manual VPN deactivation through NetworkManager.
- Confirmed public route decision through the VPN interface.
- Confirmed Quad9 DNS is configured in the NetworkManager WireGuard profile.
- Kept wg-quick plus vpn-on-safe as fallback.

### Result

NetworkManager-managed WireGuard is viable.

The next required step is post-reboot validation before making NetworkManager the primary VPN workflow.

## NetworkManager WireGuard Post-Reboot Validation

### Completed

- Rebooted after NetworkManager WireGuard import and testing.
- Confirmed VPN was inactive before manual activation.
- Confirmed vpn-nm-on successfully activated the NetworkManager WireGuard profile.
- Confirmed public route decision through the VPN interface.
- Confirmed Quad9 DNS was detected after activation.
- Confirmed UFW and Fail2ban remained active.
- Confirmed Docker, containerd, and SSH remained inactive.

### Result

NetworkManager-managed WireGuard passed post-reboot validation.

NetworkManager is now the preferred VPN control method, while wg-quick plus vpn-on-safe remains available as fallback.

## DNS Policy Definition

### Completed

- Reviewed DNS behavior after NetworkManager WireGuard validation.
- Configured Quad9 DNS on the active Wi-Fi NetworkManager profile.
- Ignored IPv6 on the tested Wi-Fi profile to reduce leak risk in the current setup.
- Confirmed Quad9 DNS is detected when VPN is inactive.
- Confirmed Quad9 DNS is detected when VPN is active.
- Confirmed VPN-active public route uses the NetworkManager WireGuard interface.

### Result

DNS behavior is now more consistent.

Quad9 is detected in both VPN-off and VPN-on states.

This improves the baseline, but does not replace VPN kill switch or external DNS leak testing.

## VPN Kill Switch Planning Started

### Completed

- Started Phase 2C for VPN kill switch and leak protection planning.
- Created initial design document.
- Clarified that the VPN cannot start before base network connectivity exists.
- Defined the real goal as blocking ordinary outbound traffic outside the VPN.
- Documented possible approaches.
- Selected a cautious hybrid direction: NetworkManager for VPN control and firewall rules for leak protection.
- Deferred rule application until rollback and testing are prepared.

### Result

No kill switch rules were applied yet.

The project is now ready for private facts collection and rollback planning.

## VPN Kill Switch Facts Collection

### Completed

- Collected private kill switch facts.
- Identified Wi-Fi, Ethernet, and VPN interface roles.
- Confirmed VPN route through the NetworkManager WireGuard interface.
- Confirmed Quad9 DNS was active during VPN-on state.
- Confirmed UFW is active with deny incoming and allow outgoing.
- Confirmed VPN endpoint information is available privately.
- Created sanitized public facts summary.
- Created initial test-only kill switch design.

### Result

The system has enough information to design a manual kill switch test.

No kill switch rules have been applied yet.

## Local UFW Rollback Script

### Completed

- Collected a private UFW baseline snapshot.
- Created local helper script: ufw-baseline-restore.
- Added explicit --apply requirement to prevent accidental firewall reset.
- Documented the rollback behavior.

### Result

A local emergency recovery command is now available before kill switch rules are tested.

No kill switch rules were applied.

## VPN Kill Switch Dry-Run Helper

### Completed

- Created a local dry-run helper for the planned VPN kill switch.
- The helper detects the local NetworkManager WireGuard profile.
- The helper detects the VPN endpoint port.
- The helper redacts the endpoint from output.
- The helper prints the intended UFW model.
- The helper does not apply any firewall rules.

### Result

The project now has a preview step before any restrictive firewall rules are tested.

No kill switch rules were applied.

## VPN Kill Switch Manual Test

### Completed

- Applied a temporary UFW-based VPN kill switch test.
- Confirmed traffic works while VPN is active.
- Confirmed ordinary outbound traffic is blocked when VPN is disconnected.
- Confirmed traffic works again after reactivating the VPN.
- Executed rollback after the test.
- Confirmed baseline firewall policy was restored.

### Result

The manual kill switch test was successful.

The test confirmed that the planned UFW model can block ordinary Wi-Fi fallback traffic when the VPN is inactive.

The kill switch is not persistent yet.

## Local Helper Scripts Documentation

### Completed

- Documented local VPN helper scripts.
- Documented NetworkManager VPN helpers.
- Documented legacy wg-quick fallback helpers.
- Documented UFW rollback helper.
- Documented kill switch dry-run helper.
- Documented endpoint resolution dry-run helper.
- Documented temporary kill switch test helper.

### Result

The local helper workflow is now documented.

NetworkManager remains the preferred VPN control method.

The UFW kill switch test passed, but persistent kill switch enforcement is not enabled yet.

## External DNS Leak Testing

### Completed

- Checked public IP organization with VPN inactive.
- Confirmed VPN-inactive public IP organization matched the local ISP privately.
- Ran external DNS leak test with VPN inactive.
- Confirmed DNS provider shown externally differed from the local ISP.
- Checked public IP organization with VPN active.
- Confirmed VPN-active public IP organization changed.
- Ran external DNS leak test with VPN active.
- Confirmed external DNS behavior changed with VPN active.
- Confirmed no local ISP DNS was observed in the external DNS leak test.

### Result

External DNS leak testing passed in sanitized form.

The current DNS policy appears to work in both VPN-on and VPN-off states.

Raw IPs, resolver addresses, locations, and ISP names were not published.

## Backup and Restore Strategy

### Completed

- Started Phase 2E for backup and restore planning.
- Created backup and restore strategy document.
- Created restore checklist.
- Collected private backup inventory snapshot.
- Defined what should be backed up.
- Defined what must not be published.
- Defined initial restore priorities.

### Result

Backup planning has started.

No full backup has been executed yet.

The next step is to choose a backup destination and test a small restore.

## USB Backup Test

### Completed

- Identified USB flash drive.
- Mounted the USB drive for backup testing.
- Created backup directory structure.
- Exported package lists.
- Exported service lists.
- Created a tracked-only project archive using git archive.
- Generated SHA256 checksums.
- Performed a small restore test.
- Confirmed README, docs, and dotfiles restored successfully.
- Synchronized and unmounted the USB drive.

### Result

The first USB backup and small restore test passed.

Only non-sensitive data was included because the USB drive was treated as non-encrypted removable storage.

## Restic Encrypted Backup Test

### Completed

- Confirmed Restic is installed.
- Initialized a Restic repository on the USB backup drive.
- Created non-sensitive test data.
- Created an encrypted Restic backup snapshot.
- Listed Restic snapshots.
- Ran Restic repository integrity check.
- Restored the latest snapshot into a temporary directory.
- Confirmed test files restored successfully.

### Result

The encrypted USB backup workflow passed.

Restic is now a viable candidate for future sensitive backups, provided the backup password is managed securely.

## Restic Backup Include/Exclude Policy

### Completed

- Created a Restic exclude policy file.
- Created a Restic backup policy document.
- Defined initial include categories.
- Defined initial exclude categories.
- Marked VPN, SSH, browser, token, and raw private data as sensitive.
- Defined that sensitive backups require password strategy first.

### Result

The project now has a backup include/exclude policy before larger Restic backups are attempted.

Next step is a selected-directory Restic backup using the exclude file.

## Restic Selected Directory Backup Test

### Completed

- Updated Restic exclude policy to exclude private/ and .git/.
- Used Restic to back up the workstation baseline project directory.
- Applied the Restic exclude file during backup.
- Restored the latest snapshot into a temporary directory.
- Confirmed README.md was restored.
- Confirmed docs/ was restored.
- Confirmed backup policy files were restored.
- Confirmed private/ was excluded.
- Confirmed .git/ was excluded.

### Result

The selected-directory encrypted Restic backup test passed.

Restic successfully backed up a real project directory while excluding private data and Git internals.

## Restic Password Strategy

### Completed

- Created Restic password strategy document.
- Defined password handling requirements.
- Defined unsafe password storage locations.
- Defined recommended password storage approach.
- Marked sensitive backups as blocked until password storage is confirmed.

### Result

Restic remains the preferred encrypted backup tool.

Sensitive backup execution is intentionally delayed until password storage and recovery are clearly defined.

## Restic Password Storage Direction

### Completed

- Selected the password storage direction for future Restic backups.
- Defined password manager as the primary storage method.
- Defined offline written recovery copy as emergency fallback.
- Confirmed the Restic password must not be stored in Git, plaintext files, screenshots, chat, or the same USB backup drive.

### Result

Sensitive backups remain blocked until the real Restic password is stored safely and recovery access is confirmed.

## Private Backup Planning

### Completed

- Created private backup planning document.
- Defined conservative private backup categories.
- Marked browser profiles, VPN configs, SSH private keys, tokens, and raw outputs as excluded by default.
- Created a local-only private include candidate list under private/.
- Created a public include-list template without real private paths.

### Result

Private backup planning has started.

No sensitive backup has been executed yet.

The next step is to choose a small reviewed private path for a controlled encrypted backup test.

## Persistent Kill Switch Post-Reboot Validation

### Completed

- Rebooted with persistent UFW kill switch rules active.
- Confirmed ordinary web traffic did not work before VPN activation.
- Started VPN manually using the NetworkManager helper.
- Confirmed internet access returned after VPN activation.
- Confirmed public IP path changed to VPN after activation.

### Result

Persistent kill switch behavior passed post-reboot validation.

VPN autoconnect is not enabled yet.

## VPN Autoconnect Validation

### Completed

- Enabled autoconnect for the NetworkManager WireGuard profile.
- Confirmed Wi-Fi autoconnect remained enabled.
- Tested VPN autoconnect behavior after boot.
- Confirmed internet access worked through the VPN.
- Confirmed the persistent kill switch remained active.
- Confirmed the current model prevents ordinary traffic from leaving outside the VPN when the VPN is inactive.

### Result

NetworkManager VPN autoconnect passed validation.

The workstation now uses a stronger privacy model: persistent kill switch plus VPN autoconnect.
