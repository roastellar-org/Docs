# Incident Report IR-0051 — Wallet Login Issue

**Date:** 2026-06-18
**Severity:** P2
**Status:** Resolved
**Owner:** rupandos

## Summary

Approximately 8% of login attempts failed for players using **Phantom** on desktop. The failure was intermittent, showed as `INVALID_SIGNATURE`, and was not reproducible on mobile.

## Impact

- ~8% of daily logins failed over 3 days
- Retried logins eventually succeeded after refresh, confusing users
- Support volume up 2x during the window

## Timeline

| Time (UTC) | Event |
|-----------|-------|
| 2026-06-15 | Gradual rise in `INVALID_SIGNATURE` errors observed in Grafana |
| 2026-06-16 | Alert fired: error rate above 5% threshold |
| 2026-06-17 | Root cause investigation begins; client telemetry enabled |
| 2026-06-18 | Root cause identified: signature serialization mismatch |
| 2026-06-18 | Backend normalization deployed (mitigation) |
| 2026-06-19 | Frontend SDK bumped; error rate back to baseline |

## Root Cause

Phantom's desktop extension returned the signature in two different serializations depending on transaction type:

- **base58 string** (expected by the backend)
- **raw byte array** (produced in some message-signing paths)

The backend rejected byte arrays, and the frontend did not normalize before sending. Logins that happened to take the byte-array path failed deterministically.

## Resolution

1. Backend now accepts both `base58` strings and byte arrays, normalizing internally.
2. Frontend SDK updated to always send base58 (see [wallet flow](../api/flows/wallet-flow.md)).
3. Added a contract test locking the accepted serializations.

## Preventative Actions

| Action | Status |
|--------|--------|
| Backend accepts both signature formats | Done — `GameBackend` issue #134 |
| Frontend normalizes before send | Done — Frontend issue #56 |
| Alert on `INVALID_SIGNATURE` rate | Done — [monitoring guide](../ops/monitoring-guide.md) |

## Lessons Learned

- Wallet SDKs evolve serialization formats without notice; the API boundary must tolerate both.
- Client-side telemetry was essential to find this; added to default instrumentation.

## Related

- [Wallet Authentication Flow](../api/flows/wallet-flow.md)
- [API Reference](../api/overview.md)
