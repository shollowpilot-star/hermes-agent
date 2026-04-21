# Phase 7.5e hygiene & reversal notes

VPS applied 2026-04-21. These are **non-file-system** mitigations that live on the VPS but are documented here for reversal and audit.

## Update-check suppression (defense-in-depth)

hermes-agent has **no native env/flag** to disable its update-check probe (confirmed via grep of `/opt/hermes/src` — no `HERMES_DISABLE_UPDATE*` pattern exists). Two layers of mitigation:

### Layer 1 — Poisoned cache, immutable file

- File: `/var/lib/hermes/.hermes/.update_check`
- Payload: `{"ts": 2145916800, "behind": 0}` (ts = 2038-01-01 UTC; far-future cache TTL)
- Perms: `644 hermes:hermes`, `chattr +i` (immutable — hermes user cannot rewrite)
- Effect: `hermes_cli/banner.py:149` returns cached `behind=0`; banner print at `hermes_cli/main.py:3134` suppressed.

### Layer 2 — Broken git remote

- File: `/opt/hermes/src/.git/config`
- Original `url`: `https://github.com/shollowpilot-star/hermes-agent.git`
- Modified `url`: `file:///dev/null`
- Backup: `/etc/hermes/git-origin.bak` (0640 root:root) retains original URL verbatim
- Effect: `git fetch` as hermes returns `fatal: '/dev/null' does not appear to be a git repository`, exit 128.

## Mode-777 hygiene

| Path | Before | After | Note |
|------|--------|-------|------|
| `/var/lib/hermes/.cache/uv/.lock` | 777 | 644 | was world-writable |
| `/var/lib/hermes/.local/bin/python3.13` | 777 (cosmetic) | no change | **symlink**; real target at `.../uv/python/cpython-3.13.7-linux-x86_64-gnu/bin/python3.13` is 755. Linux symlinks always stat as 777. Accepted. |

## hermesctl wrapper + sudoers (new)

- `/usr/local/bin/hermesctl` (0755 root:root) — ergonomic admin proxy
- `/etc/sudoers.d/50-hermesctl` (0440 root:root) — NOPASSWD for hermesctl only
- Purpose: opsadmin runs `hermesctl status` without remembering the full `sudo -u hermes -H env HERMES_HOME=... /opt/hermes/venv/bin/hermes ...` invocation
- Preserves zero-trust: binary path is fenced, venv stays inaccessible to opsadmin shell

## Reversal recipes

### Re-enable update-check (for emergency upstream pull)

    sudo chattr -i /var/lib/hermes/.hermes/.update_check
    sudo -u hermes -H git -C /opt/hermes/src remote set-url origin "$(sudo cat /etc/hermes/git-origin.bak)"
    sudo rm /var/lib/hermes/.hermes/.update_check
    sudo -u hermes -H git -C /opt/hermes/src fetch origin

### Remove hermesctl wrapper

    sudo rm /usr/local/bin/hermesctl /etc/sudoers.d/50-hermesctl

## Provenance

- Upstream pin: `v2026.4.16` (Nous Research, hermes-agent)
- Fork: `shollowpilot-star/hermes-agent` — Phase 7.5d PR merged 2026-04-21
- NIST: SSDF PS.3.2 (pin to known-good), AC-6 (least privilege), AU-2/AU-3 (audit events)
