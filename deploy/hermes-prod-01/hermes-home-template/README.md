# hermes-home-template — Phase 7.5e

Seed config for `$HERMES_HOME` on hermes-prod-01.

## Target paths (VPS)

| File | Path | Owner | Mode |
|------|------|-------|------|
| config.yaml | `/var/lib/hermes/.hermes/config.yaml` | root:hermes | 0640 |
| SOUL.md     | `/var/lib/hermes/.hermes/SOUL.md`     | root:hermes | 0640 |

Parent dir: `/var/lib/hermes/.hermes/` 0750 hermes:hermes (managed by `StateDirectory=hermes` in systemd unit).

## systemd env bindings (Phase 7.7)

```ini
User=hermes
Group=hermes
StateDirectory=hermes
Environment=HOME=/var/lib/hermes
Environment=HERMES_HOME=/var/lib/hermes/.hermes
```

## Design — Minimum Viable Config (MVC)

Only 3 sections set explicitly:
- `model.default` — OpenRouter model id
- `terminal.backend` — local (runs inside hermes sandbox; full isolation via systemd directives)
- `network.force_ipv4` — VPS is IPv4-only

All other keys fall through to in-code defaults. Secrets load from `/etc/hermes/env` (not written here).

Schema reference: `hermes_cli/setup.py` (wizard source) and `hermes_constants.py` (path resolution).

## Reproducibility

Generated 2026-04-21 on hermes-prod-01. See parent `README.md` for full provenance.

## Integrity

See `SHA256SUMS.txt` alongside this file.
