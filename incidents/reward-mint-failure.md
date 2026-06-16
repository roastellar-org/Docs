# Incident Report IR-0042 — Reward Mint Failure

**Date:** 2026-06-09
**Severity:** P1
**Status:** Resolved
**Owner:** rupamghosh2006

## Summary

Reward distribution for tournament **#T-118** stalled for approximately 45 minutes. Winners did not receive their Reward NFTs within the expected SLA, and leaderboard rankings for the affected tournament were not updated. No funds were lost.

## Impact

- 32 of 32 winners affected (rewards delayed)
- Leaderboard stale for ~45 minutes
- Zero data loss; all rewards eventually issued

## Timeline

| Time (UTC) | Event |
|-----------|-------|
| 14:02 | Tournament #T-118 completes; distribution job enqueued |
| 14:05 | Monitoring alerts on reward queue depth |
| 14:12 | On-call engineer paged; investigation begins |
| 14:18 | Root cause identified: RPC rate limit from primary provider |
| 14:26 | Failover to secondary RPC endpoint implemented |
| 14:47 | Queue drained; all rewards minted, leaderboard updated |
| 14:52 | Incident resolved; postmortem initiated |

## Root Cause

The reward distribution service exhausted the **primary RPC provider's rate limit** during a burst of 32 simultaneous mints. The service had:

- No automatic failover between RPC endpoints
- No backoff/retry strategy for RPC errors
- A queue consumer that stopped processing on first error

## Resolution

1. Manually failed over to the secondary RPC endpoint.
2. Restarted the queue consumer with error-tolerant processing.
3. Confirmed all 32 mints issued and receipts persisted.

## Preventative Actions

| Action | Status |
|--------|--------|
| Automatic RPC failover with circuit breaker | Done — `GameBackend` issue #112 |
| Exponential backoff for mint retries | Done — issue #113 |
| Alert on RPC error rates | Done — [monitoring guide](../ops/monitoring-guide.md) |
| Idempotent distribution keyed by `distribution_id` | Done — see [nft reward flow](../api/flows/nft-reward-flow.md) |

## Lessons Learned

- Reward jobs must be idempotent; manual re-runs are expected.
- RPC providers need redundant configuration from day one.
- Queue consumers should isolate per-item errors, not abort the batch.

## Related

- [NFT Reward Flow](../api/flows/nft-reward-flow.md)
- [Monitoring Guide](../ops/monitoring-guide.md)
