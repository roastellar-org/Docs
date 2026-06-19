# Incident Report IR-0047 — Leaderboard Inconsistency

**Date:** 2026-06-12
**Severity:** P2
**Status:** Resolved
**Owner:** rupandos

## Summary

After tournament **#T-204** completed, the leaderboard showed rankings that disagreed with the final standings used for payouts. Two players appeared in reversed order, and winnings totals were briefly inconsistent across pages.

## Impact

- 2 misordered entries on the global leaderboard for ~2 hours
- No financial impact: payouts used the correct snapshot
- Player-facing trust issue; support received 3 tickets

## Timeline

| Time (UTC) | Event |
|-----------|-------|
| 18:20 | Tournament #T-204 completes |
| 18:21 | Standings snapshot taken for payouts |
| 18:22 | Leaderboard refresh job runs |
| 19:40 | Player reports wrong ranking on Discord |
| 19:52 | Engineering confirms mismatch between snapshot and leaderboard |
| 20:10 | Root cause identified: race between score ingestion and refresh |
| 20:25 | Leaderboard rebuilt from the payout snapshot |
| 20:31 | Monitoring confirms leaderboard consistent |

## Root Cause

The leaderboard refresh read live `scores` rows while finalization was still writing the payout snapshot. Score ingestion for two matches committed **after** the snapshot but **before** the leaderboard query, causing the refresh to pick up different totals than the payout source of truth.

Two services read the same table with different ordering guarantees; there was no shared "frozen standings" concept.

## Resolution

1. Rebuilt the leaderboard from the authoritative payout snapshot.
2. Added a `standings_frozen_at` marker read by both reward distribution and leaderboard refresh.
3. Both jobs now read from the snapshot table, never from live `scores`.

## Preventative Actions

| Action | Status |
|--------|--------|
| Leaderboard reads frozen standings snapshot | Done — `GameBackend` issue #121 |
| Add consistency check between leaderboard and payout totals | Done — issue #122 |
| Alert when leaderboard refresh lags finalization | Planned — issue #123 |

## Lessons Learned

- Any computation that can affect money must share the same source of truth as payouts.
- Finalization should be atomic: snapshot + state change in one transaction (see [tournament flow](../api/flows/tournament-flow.md)).

## Related

- [Tournament Flow](../api/flows/tournament-flow.md)
- [Monitoring Guide](../ops/monitoring-guide.md)
