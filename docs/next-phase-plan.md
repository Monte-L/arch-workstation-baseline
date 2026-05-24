# Next Phase Plan

## Purpose

This document defines the next hardening and improvement phases after the initial Arch Linux workstation baseline.

The initial baseline focused on inventory, firewall review, service minimization, VPN/DNS behavior, dotfile sanitization, and repository safety.

The next phase will focus on improving long-term privacy, reliability, backup readiness, and controlled remote access.

## Phase 2 Overview

| Phase | Focus | Priority |
|---|---|---|
| 2A | NetworkManager-managed WireGuard evaluation | High |
| 2B | DNS policy definition | High |
| 2C | VPN kill switch / leak protection planning | High |
| 2D | External DNS leak testing | High |
| 2E | Backup and restore strategy | High |
| 2F | SSH hardening plan | Medium |
| 2G | Repository presentation polish | Medium |

## 2A - NetworkManager-managed WireGuard Evaluation

### Goal

Evaluate whether the current WireGuard setup should be migrated from wg-quick plus resolvconf to a NetworkManager-managed WireGuard profile.

### Reason

The current setup works, but it required a temporary vpn-on-safe helper after a post-reboot resolvconf issue.

NetworkManager is a better long-term candidate for a laptop workstation because it handles:

- Wi-Fi changes
- reconnects
- suspend and resume behavior
- per-connection DNS settings
- VPN profile management
- desktop integration

### Tasks

1. Review the current WireGuard profile privately.
2. Identify whether the endpoint is static or dynamic.
3. Create a NetworkManager WireGuard profile.
4. Test VPN activation.
5. Test VPN deactivation.
6. Confirm public route decision.
7. Confirm DNS behavior.
8. Compare with the current wg-quick setup.
9. Decide whether to migrate permanently.

### Success Criteria

- VPN can be activated through NetworkManager.
- Public traffic routes through the VPN profile.
- DNS behavior is predictable.
- No private keys or endpoints are exposed in public documentation.
- Rollback to wg-quick remains available.

## 2B - DNS Policy Definition

### Goal

Define how DNS should behave in both VPN-on and VPN-off states.

### Current Finding

During baseline testing:

| State | Result |
|---|---|
| VPN active | Quad9 detected directly |
| VPN inactive | Quad9 not detected directly |

### Decision Needed

Choose one DNS model:

| Model | Description |
|---|---|
| VPN DNS only | Trust VPN-provided DNS while connected |
| Quad9 through VPN | Use Quad9 while VPN is active |
| Quad9 always | Enforce Quad9 even when VPN is off |
| VPN-required | Block normal traffic when VPN is inactive |

### Success Criteria

- DNS behavior is intentional.
- VPN-on and VPN-off states are documented.
- DNS leak testing is performed.
- DNS management does not conflict between NetworkManager, resolvconf, and WireGuard.

## 2C - VPN Kill Switch / Leak Protection

### Goal

Prevent normal outbound traffic from leaving through Wi-Fi or Ethernet before the VPN is active.

### Important Clarification

The VPN cannot start before basic network connectivity exists.

The real goal is not VPN before internet.

The real goal is:

- allow only what is required to establish the VPN
- block normal outbound traffic before VPN activation
- allow normal traffic only through the VPN interface

### Possible Approaches

| Approach | Notes |
|---|---|
| ProtonVPN native kill switch | Easier if available and reliable |
| NetworkManager-based VPN control | Better desktop integration |
| Firewall-based kill switch | Stronger and more transparent, but requires careful rollback |
| Hybrid model | NetworkManager for VPN, firewall for leak protection |

### Safety Requirements

Before applying any kill switch:

1. Create rollback commands.
2. Test on Wi-Fi.
3. Test VPN-on state.
4. Test VPN-off state.
5. Test DNS behavior.
6. Confirm local network access requirements.
7. Avoid locking the system out of all connectivity.

### Success Criteria

- Normal outbound traffic is blocked when VPN is inactive.
- VPN endpoint connection is allowed.
- Traffic flows normally when VPN is active.
- DNS does not leak outside the intended resolver path.
- Rollback works.

## 2D - External DNS Leak Testing

### Goal

Validate DNS behavior externally using a trusted DNS leak test.

### Tasks

1. Test with VPN active.
2. Test with VPN inactive.
3. Compare DNS resolvers.
4. Confirm whether resolver location/provider matches expectations.
5. Document results without publishing public IPs or resolver details unless intentionally sanitized.

### Success Criteria

- VPN-active DNS behavior is externally validated.
- Any leaks are documented.
- Follow-up actions are added to the risk register.

## 2E - Backup and Restore Strategy

### Goal

Create a backup and restore strategy for the workstation.

### Scope

The backup plan should cover:

- dotfiles
- documents
- project repositories
- package list
- service state
- selected system configuration
- recovery documentation

### Tools to Evaluate

| Tool | Purpose |
|---|---|
| rsync | File-level backup |
| tar | Archive creation |
| restic | Encrypted backup |
| borg | Deduplicated encrypted backup |
| pacman -Qqe | Package inventory |
| git | Project and dotfile versioning |

### Success Criteria

- Backup target is defined.
- Restore test is performed.
- Recovery steps are documented.
- Sensitive files are handled carefully.
- Backup strategy supports workstation rebuild.

## 2F - SSH Hardening Plan

### Goal

Prepare a secure SSH configuration in case remote access is needed later.

### Current State

SSH is currently disabled and inactive.

No inbound UFW rule allows SSH.

### Future SSH Requirements

If SSH is enabled later:

1. Use key-based authentication.
2. Disable root login.
3. Disable password authentication if possible.
4. Restrict UFW source rules.
5. Keep Fail2ban sshd jail active.
6. Document access model.
7. Test from trusted network only.

### Success Criteria

- SSH is only enabled if needed.
- SSH is not exposed broadly.
- Authentication is key-based.
- Firewall rules are restricted.
- Fail2ban is active.

## 2G - Repository Presentation Polish

### Goal

Improve the repository for portfolio use.

### Tasks

1. Review README formatting on GitHub.
2. Check all Markdown tables.
3. Add screenshots only if sanitized.
4. Add a clear project status section.
5. Add a future roadmap section.
6. Keep private data excluded.
7. Consider adding a LICENSE file.
8. Consider adding a SECURITY.md file later.

## Current Priority Order

The recommended next order is:

1. Review GitHub rendering.
2. Perform final public repository check.
3. Start NetworkManager WireGuard evaluation.
4. Define DNS policy.
5. Plan VPN kill switch safely.
6. Build backup and restore strategy.
7. Revisit SSH only if needed.

## Status

Initial baseline: completed.

Next phase: planned.

## Phase 2A Update - NetworkManager WireGuard Evaluation

Initial NetworkManager WireGuard testing was successful.

Findings:

| Item | Result |
|---|---|
| NetworkManager WireGuard profile | imported |
| Manual activation through NetworkManager | successful |
| Manual deactivation through NetworkManager | successful |
| Public route through VPN | successful |
| Quad9 DNS in NetworkManager profile | configured |
| IPv6 in VPN profile | disabled |
| Autoconnect | disabled during testing |

Current decision:

NetworkManager-managed WireGuard is viable, but still requires post-reboot validation before replacing the current fallback workflow.

The wg-quick plus vpn-on-safe method remains available as fallback.

## Phase 2A Post-Reboot Result

NetworkManager-managed WireGuard passed post-reboot validation.

Result:

| Item | Status |
|---|---|
| Manual activation before reboot | passed |
| Manual deactivation before reboot | passed |
| Activation after reboot | passed |
| VPN route after reboot | passed |
| Quad9 DNS after reboot | passed |

Decision:

NetworkManager is now the preferred VPN control method.

The old wg-quick/resolvconf workflow remains as fallback while DNS policy and kill switch planning continue.

## Phase 2B Update - DNS Policy Definition

DNS policy was defined and tested.

Findings:

| State | Result |
|---|---|
| VPN inactive | Quad9 detected through Wi-Fi profile |
| VPN active | Quad9 detected through NetworkManager WireGuard profile |
| IPv6 | Ignored/disabled in the current tested path |
| DNS management | NetworkManager preferred |

Current decision:

Quad9 is configured for the active Wi-Fi profile and the NetworkManager WireGuard profile.

This improves DNS consistency, but does not replace a VPN kill switch.

New Wi-Fi or Ethernet profiles may require separate configuration or future dispatcher automation.

## Phase 2C Started - VPN Kill Switch Planning

VPN kill switch and leak protection planning has started.

Current decision:

No firewall kill switch rules are applied yet.

The first step is documentation and safety planning.

The next implementation steps are:

1. Collect private kill switch facts.
2. Identify Wi-Fi, Ethernet, VPN interface, and VPN endpoint information.
3. Prepare rollback commands.
4. Design a test-only ruleset.
5. Test VPN-on state.
6. Test VPN-off state.
7. Test rollback.
8. Only then consider persistent enforcement.

The preferred long-term direction remains:

NetworkManager-managed WireGuard plus DNS policy plus firewall-based leak protection.

## Local Helper Scripts Status

Local helper scripts were documented after the first successful manual VPN kill switch test.

Current status:

| Helper Area | Status |
|---|---|
| NetworkManager VPN helpers | available |
| wg-quick fallback helpers | available |
| UFW rollback helper | available |
| Kill switch dry-run helper | available |
| Endpoint resolution dry-run helper | available |
| Temporary kill switch test helper | tested |
| Persistent kill switch | not enabled |

Next recommended phase:

External DNS leak testing before deciding whether to make kill switch behavior persistent.

## Phase 2D Update - External DNS Leak Testing

External DNS leak testing was completed in sanitized form.

Results:

| State | Result |
|---|---|
| VPN inactive | local ISP detected as public IP organization, but local ISP DNS was not observed |
| VPN active | public IP organization changed and local ISP DNS was not observed |

Current assessment:

The DNS policy passed the external leak test.

Future DNS leak tests should be repeated after any persistent kill switch implementation or when adding new Wi-Fi/Ethernet profiles.
