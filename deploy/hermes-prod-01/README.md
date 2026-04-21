# Hermes deployment: hermes-prod-01

**Host:** hermes-prod-01 (Hostinger KVM VPS, Ubuntu 24.04 LTS, linux-x86_64)
**Python:** 3.13.7 in `/opt/hermes/venv`
**Node.js:** 22.22.2 (NodeSource repo, pinned)
**Source commit:** v2026.4.16 (1dd6b5d5fb94cac59e93388f9aeee6bc365b8f42)
**Generated:** 2026-04-21

## Artifacts in this directory

| File | Purpose |
|------|---------|
| `python-requirements.locked.txt` | Hash-pinned PyPI artifacts — 3447 lines, 2738 SHA256 hashes, 158 packages |
| `python-requirements.frozen.txt` | Flat version-pinned dep list (pre-hash) — 158 packages |
| `manifest.txt` | Provenance: generator identity, tooling versions, git commit, SHA256 anchors |
| `README.md` | This file |

## Relationship to upstream `uv.lock`

The repo root ships `uv.lock` — upstream's native-format lockfile for the `uv`-workflow install path.

This deployment uses a different install path: **pip-compatible editable install** via `uv pip install -e .[all-minus-dev]`. The resulting transitive closure is captured here as a hash-pinned PEP 508 requirements file, **independent of `uv.lock`**.

Both files are valid for their respective paths. For this host, use files in this directory. For upstream's uv-native workflow, use `uv.lock`.

## Install reproducibility

To reproduce this exact dep state on a matching host (Ubuntu 24.04, Python 3.13.7, linux-x86_64):

    uv pip sync --python /opt/hermes/venv/bin/python \
      deploy/hermes-prod-01/python-requirements.locked.txt

## Integrity verification

Before installing, verify the lockfile SHA256 against `manifest.txt`:

    sha256sum deploy/hermes-prod-01/python-requirements.locked.txt
    grep locked.txt.sha256 deploy/hermes-prod-01/manifest.txt

These must match.

## Platform scope

**Pins are linux-x86_64-specific.** The wheel hashes include platform-tagged artifacts (manylinux, musllinux). Do not use on macOS, Windows, or ARM hosts without regenerating.

For a new host, generate a fresh bundle under `deploy/<hostname>/` — do not share.

## WhatsApp bridge

Not enabled on this host. See `scripts/whatsapp-bridge/DEPLOYMENT-NOTE.md` for deferral rationale.
