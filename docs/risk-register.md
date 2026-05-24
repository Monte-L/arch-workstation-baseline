# Risk Register

## Purpose

This document tracks security, privacy, operational, and configuration risks identified during the workstation baseline review.

## Risk Table

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-001 | VPN routing required validation | Medium | Mitigated | Active VPN test confirmed ProtonVPN WireGuard interface vpn0 and public IPv4 route decision through VPN policy table 51820. |
| R-002 | Quad9 DNS not detected directly in /etc/resolv.conf | Medium | Open | DNS is managed by NetworkManager. Need to confirm whether DNS is handled by Wi-Fi, VPN, Quad9, or another resolver. |
| R-003 | Raw network and system outputs may contain sensitive information | High | Mitigated | Raw outputs are stored under private/raw/, and private/ is excluded through .gitignore. |
| R-004 | Dotfiles may contain private paths, usernames, tokens, or machine-specific data | High | Open | Dotfiles must be reviewed before being copied into the public project structure. |
| R-005 | WireGuard public keys and VPN endpoints may appear in command output | High | Mitigated | Raw VPN output must remain private. Public documentation only records sanitized behavior and routing conclusions. |
| R-006 | DNS behavior while VPN is active has not been fully validated | Medium | Open | Need DNS leak testing and resolver confirmation while ProtonVPN is active. |

## Severity Guide

| Severity | Meaning |
|---|---|
| Low | Minor issue or documentation improvement |
| Medium | Security or privacy issue requiring validation |
| High | Could expose sensitive data or weaken system security |
| Critical | Immediate risk of credential exposure, compromise, or data loss |

## Current Priority

The next priorities are:

1. Verify DNS behavior while VPN is active.
2. Review firewall status.
3. Review listening services.
4. Review Fail2ban status.
5. Review dotfiles before copying anything into the public folder.

## Firewall Cleanup Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-011 | Inbound SSH rule was open from anywhere | Medium | Mitigated | Removed UFW allow rule for port 22. SSH can be reintroduced later with stricter controls. |
| R-012 | Inbound development port 8000 was open from anywhere | Medium | Mitigated | Removed UFW allow rule for port 8000/tcp. |
| R-013 | Inbound WireGuard port 51820/udp was open from anywhere | Medium | Mitigated | Removed UFW allow rule for 51820/udp. Not required for current ProtonVPN client mode. |
| R-014 | Firewall had permissive inbound rules after initial setup | Medium | Mitigated | UFW now has no explicit inbound allow rules and uses default deny incoming policy. |

## Fail2ban Review Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-015 | Fail2ban service misconfiguration | Medium | Mitigated | Fail2ban is enabled, active, and running one jail for sshd. |
| R-016 | SSH may be reintroduced later without strict controls | Medium | Open | If SSH is enabled in the future, it should require key-based authentication, restricted UFW source rules, and active Fail2ban protection. |

## SSH and Fail2ban Follow-Up

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-017 | SSH service exposure | Medium | Mitigated | sshd is disabled and inactive, and UFW has no inbound SSH allow rule. |
| R-018 | Fail2ban sshd jail active while SSH is inactive | Low | Documented | This is not a current exposure. The jail is ready if SSH is enabled later. |

## Package and Service Review Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-019 | Docker is enabled and running by default | Medium | Open | Docker is useful for lab and development work, but increases local attack surface. Decide whether it should start at boot or be started manually. |
| R-020 | AUR package usage requires manual trust review | Medium | Open | The system has 11 foreign/AUR packages. New AUR packages should be reviewed before installation. |
| R-021 | tor-browser-alpha-bin is alpha software | Low | Open | Alpha software may be less stable than a standard release. Review whether alpha build is necessary. |
| R-022 | yay-debug may be unnecessary | Low | Open | Debug package should be removed if not needed. |
| R-023 | wireguard-dkms may be unnecessary | Low | Open | Review whether DKMS package is required with the current kernel and WireGuard setup. |
| R-024 | NetworkManager-wait-online.service may delay boot | Low | Open | Review whether this service is needed for the workstation workflow. |

## VPN Kill Switch Review

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-025 | Outbound traffic is allowed before VPN activation | Medium | Open | UFW currently allows outgoing traffic by default. This means traffic may leave through Wi-Fi before ProtonVPN is active. A VPN kill switch should be evaluated. |
| R-026 | VPN autostart may fail if Wi-Fi is not ready | Low | Open | WireGuard can be started automatically, but on a laptop using Wi-Fi it may require NetworkManager integration or dispatcher scripts. |

## Service Hardening Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-027 | Docker service running continuously | Medium | Mitigated | docker.service was disabled and stopped. Docker can be started manually when needed. |
| R-028 | Docker socket could trigger Docker automatically | Medium | Mitigated | docker.socket was disabled and stopped to prevent socket activation. |
| R-029 | containerd listener remained active after Docker service stop | Low | Mitigated | containerd.service was stopped and is now inactive during normal workstation use. |

## Enabled Services Review Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-030 | Unnecessary network-facing services enabled at boot | Medium | Mitigated | No unnecessary network-facing services such as Docker, SSH, CUPS, Avahi, Samba, or Bluetooth were observed as enabled during the review. |
| R-031 | SSH-related local Unix socket observed | Low | Documented | An sshd-related local Unix socket was observed, but sshd.service is disabled/inactive and SSH is not exposed through UFW. |
| R-032 | Enabled services should be rechecked after reboot | Low | Open | A post-reboot validation should confirm Docker, containerd, and NetworkManager-wait-online remain disabled/inactive. |

## Package Cleanup Final Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-033 | Unnecessary debug packages installed | Low | Mitigated | Debug packages such as yay-debug, wireguard-dkms, and eww-debug were removed or confirmed absent where not required. |
| R-034 | Go package appears as orphan | Low | Documented | Go is installed but not required by another package. It is kept for now as a possible development/build tool. |
| R-035 | Tor Browser alpha package remains installed | Low | Open | tor-browser-alpha-bin remains installed and should be reviewed later against a stable Tor Browser option. |

## DNS Behavior Review Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-036 | Quad9 not detected during earlier DNS baseline | Medium | Partially mitigated | Quad9 was detected directly in /etc/resolv.conf during VPN-active testing. VPN-off behavior still requires review. |
| R-037 | DNS leak protection not externally validated | Medium | Open | VPN-active routing and Quad9 detection are positive signs, but an external DNS leak test is still required. |
| R-038 | DNS behavior may differ between VPN-on and VPN-off states | Medium | Open | Earlier and later checks showed different resolver behavior. Both states must be documented separately. |

## VPN Off DNS Review Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-039 | VPN-off traffic routes through regular Wi-Fi | Medium | Documented | When ProtonVPN is inactive, public route decision uses the Wi-Fi interface. |
| R-040 | Quad9 not detected during VPN-off DNS test | Medium | Open | Quad9 was detected during VPN-active testing but not during VPN-off testing. |
| R-041 | DNS behavior depends on VPN state | Medium | Open | VPN-on and VPN-off states produce different DNS behavior. Future hardening should evaluate DNS enforcement or VPN kill switch. |

## Post-Reboot VPN Startup Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-042 | VPN startup failed after reboot due to DNS/resolvconf conflict | Medium | Partially mitigated | A temporary vpn-on-safe helper was created to update resolvconf before starting WireGuard. |
| R-043 | Current VPN startup workaround is not final architecture | Medium | Open | Long-term preferred direction is NetworkManager-managed WireGuard with DNS policy and VPN kill switch. |

## NetworkManager WireGuard Evaluation Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-044 | wg-quick and resolvconf workflow may be fragile after reboot | Medium | Partially mitigated | NetworkManager-managed WireGuard was tested successfully and is now a viable replacement candidate. |
| R-045 | NetworkManager WireGuard profile not yet post-reboot validated | Medium | Open | Manual NetworkManager activation worked, but post-reboot validation is still required before making it the primary workflow. |
| R-046 | VPN autoconnect behavior not finalized | Medium | Open | Autoconnect remains disabled during testing. Final behavior should be decided after DNS policy and kill switch planning. |

## NetworkManager WireGuard Post-Reboot Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-047 | NetworkManager WireGuard may fail after reboot | Medium | Mitigated | Post-reboot validation confirmed NetworkManager can activate the WireGuard profile successfully. |
| R-048 | Temporary wg-quick/resolvconf workaround still exists | Low | Documented | The workaround remains as fallback, but NetworkManager is now the preferred VPN control method. |

## DNS Policy Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-049 | VPN-off DNS used network-provided resolver | Medium | Mitigated | Quad9 DNS was configured on the active Wi-Fi NetworkManager profile. |
| R-050 | DNS policy may not apply to new Wi-Fi or Ethernet profiles | Medium | Open | Current DNS policy applies to the configured profile. Future dispatcher automation may be needed for new connections. |
| R-051 | DNS policy does not replace VPN kill switch | Medium | Open | DNS is more consistent, but ordinary traffic can still leave via Wi-Fi when VPN is inactive. |

## VPN Kill Switch Planning Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-052 | Ordinary outbound traffic can leave when VPN is inactive | Medium | Open | DNS policy is improved, but traffic can still route through Wi-Fi when VPN is off. Kill switch planning has started. |
| R-053 | Incorrect kill switch rules could break connectivity | High | Open | No rules should be applied before rollback commands and test plan are prepared. |
| R-054 | VPN endpoint information is sensitive | Medium | Open | Endpoint details are required for implementation but must remain private and excluded from public documentation. |
| R-055 | Kill switch persistence may break boot connectivity if misconfigured | High | Open | Initial implementation should be manual/test-only before enabling persistence. |

## VPN Kill Switch Facts Collection Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-056 | Outgoing firewall policy still allows non-VPN traffic | Medium | Open | UFW currently allows outgoing traffic by default. Kill switch design is required to prevent fallback leaks. |
| R-057 | Kill switch requires private endpoint details | Medium | Documented | Endpoint information was collected privately and must not be published. |
| R-058 | UFW may be limited for advanced VPN leak protection | Medium | Open | UFW-based testing is planned first. nftables may be evaluated if UFW is insufficient. |

## VPN Kill Switch Rollback Preparation

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-059 | No emergency firewall rollback command available | High | Mitigated | A local ufw-baseline-restore helper was created with an explicit --apply requirement. |
| R-060 | Accidental rollback execution could remove custom rules | Low | Mitigated | The rollback script does nothing unless called with --apply. |

## VPN Kill Switch Manual Test Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-061 | Ordinary outbound traffic leaks when VPN disconnects | High | Partially mitigated | Manual UFW kill switch test successfully blocked ordinary outbound traffic when VPN was disconnected. |
| R-062 | Kill switch not persistent yet | Medium | Open | Test rules were rolled back after validation. Persistent behavior still requires design and reboot testing. |
| R-063 | Real VPN endpoint may appear in local firewall output | Medium | Documented | Raw outputs must remain private and must not be published. Public documentation uses sanitized placeholders. |

## External DNS Leak Test Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-064 | Local ISP DNS may appear during VPN-off state | Medium | Mitigated | External DNS leak test did not show local ISP DNS while VPN was inactive. |
| R-065 | Local ISP DNS may appear during VPN-on state | High | Mitigated | External DNS leak test did not show local ISP DNS while VPN was active. |
| R-066 | Browser DNS behavior may differ from system DNS | Low | Open | Future testing should review browser DNS-over-HTTPS settings if unexpected DNS providers appear. |

## Backup and Restore Planning Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-067 | No tested backup strategy before persistent hardening | High | Open | Backup strategy was drafted, but no full backup or restore test has been completed yet. |
| R-068 | Private configuration may be backed up insecurely | High | Open | VPN, SSH, browser, and token data must only be stored in private/encrypted backups. |
| R-069 | Restore process not yet tested | Medium | Open | A restore checklist was created, but restore testing is still required. |

## USB Backup Test Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-070 | No tested removable backup workflow | Medium | Mitigated | A USB backup and small restore test was completed with non-sensitive data. |
| R-071 | Unencrypted USB storage could expose sensitive data | High | Open | Sensitive files were intentionally excluded. Encrypted backup is still recommended for private data. |
| R-072 | Backup integrity not verified | Medium | Partially mitigated | SHA256 checksums were generated, but larger restore testing is still pending. |

## Restic Encrypted Backup Test Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-073 | No tested encrypted backup workflow | High | Mitigated | Restic encrypted backup and restore test passed using non-sensitive data. |
| R-074 | Restic password loss would make backups unrecoverable | High | Open | Password storage strategy must be defined before real sensitive backups. |
| R-075 | Sensitive files may be included accidentally | High | Open | A backup include/exclude policy is needed before backing up real private data. |

## Restic Backup Policy Results

| ID | Risk | Severity | Status | Notes |
|---|---|---|---|---|
| R-076 | Backup may include caches or unnecessary large files | Medium | Mitigated | A Restic exclude policy was created. |
| R-077 | Backup may accidentally include private keys or tokens | High | Partially mitigated | Exclude patterns were defined, but backup paths must still be reviewed manually. |
| R-078 | Restic password storage strategy not defined | High | Open | Sensitive backups should wait until password handling is defined. |
