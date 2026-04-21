# WA Bridge Deployment Status

**Status:** Installed but NOT enabled as systemd service.

**Reason:** 3 critical CVEs in protobufjs <7.5.5 via @whiskeysockets/libsignal-node (GHSA-xq3m-2v4x-88gg, arbitrary code execution). No upstream fix as of 2026-04-21.

**Re-enable decision:** Pending test of protobufjs override OR upstream Baileys bump.

**Tracking:**
- Upstream: https://github.com/WhiskeySockets/Baileys/issues
- Check periodically: `cd scripts/whatsapp-bridge && npm audit`

**Do NOT start `node bridge.js` from a production systemd unit until resolved.**
