# Incident Report

## Incident #IR-0042 — Reward Distribution Delayed

**Date:** 2026-07-21
**Severity:** P1
**Status:** Resolved

## Summary

Reward distribution for tournament #T-118 stalled for approximately 45 minutes. Winners did not receive their Reward NFTs within the expected SLA, and leaderboard rankings for the affected tournament were not updated.

## Timeline

- **14:02 UTC** — Tournament #T-118 completes; distribution job enqueued.
- **14:05 UTC** — Monitoring alerts on reward job queue depth exceeding threshold.
- **14:12 UTC** — On-call engineer paged; investigation begins.
- **14:18 UTC** — Root cause identified: RPC rate limit from the Solana endpoint provider.
- **14:26 UTC** — Failover to secondary RPC endpoint implemented.
- **14:47 UTC** — Queue drained; all rewards minted and leaderboard updated.
- **14:52 UTC** — Incident resolved; postmortem initiated.

## Root Cause

The reward distribution service exhausted the primary RPC provider's rate limit during a burst of simultaneous mints. The service lacked automatic failover and retry backoff for RPC failures.

## Resolution

- Failover to a secondary RPC endpoint restored service within 20 minutes.
- All affected rewards were issued without data loss.

## Follow-up Actions

- [ ] Add automatic RPC failover with circuit breaker.
- [ ] Implement exponential backoff for mint retries.
- [ ] Add alerting on RPC error rates.
