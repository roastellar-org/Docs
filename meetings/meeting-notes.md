# Meeting Notes

Collection of meeting notes for the Roastellar / ArenaX team.

## Format

Each meeting section records:

- **Date and attendees**
- **Decisions** (D-x)
- **Action items** (A-x) with owners and due dates

## Launch Planning — 2026-07-20

**Attendees:** abantikakundu (PM), rupamghosh2006 (Blockchain), rupandos (Backend), frontend lead, marketing lead

### Agenda

1. v1.0.0 launch date and freeze window
2. Remaining checklist items
3. Post-launch support plan

### Decisions

- **D-1:** Launch target set to **2026-08-01**; deploy freeze 24h prior.
- **D-2:** Marketplace opens with fixed-price listings only; auctions deferred to v1.1.0.
- **D-3:** Bridge stays Solana-only for launch; multi-chain in v1.1.0.

### Action Items

| ID | Item | Owner | Due |
|----|------|-------|-----|
| A-1 | Close remaining launch checklist items | abantikakundu | 2026-07-25 |
| A-2 | Verify multisig treasury withdrawal drill | rupamghosh2006 | 2026-07-24 |
| A-3 | Rehearse rollback runbook end-to-end | rupandos | 2026-07-26 |
| A-4 | Publish marketing assets | marketing | 2026-07-28 |
| A-5 | Staff support channels for launch week | abantikakundu | 2026-07-29 |

### Follow-up

- Next meeting 2026-07-28: final go/no-go. See [launch checklist](../launch/launch-checklist.md).

---

## Incident Review — 2026-06-25

**Attendees:** rupandos, rupamghosh2006, on-call engineer

### Decisions

- **D-1:** Reward distributions become idempotent (keyed by `distribution_id`).
- **D-2:** Leaderboard and payouts share the frozen standings snapshot.

### Action Items

| ID | Item | Owner | Due |
|----|------|-------|-----|
| A-1 | Implement idempotent distribution | rupandos | 2026-06-30 |
| A-2 | Standings snapshot for leaderboard | rupandos | 2026-07-02 |
| A-3 | RPC failover with circuit breaker | rupamghosh2006 | 2026-07-05 |

### Follow-up

- Tracked via the incident reports: [IR-0042](../incidents/reward-mint-failure.md), [IR-0047](../incidents/leaderboard-inconsistency.md).

---

## Product Kickoff — 2026-05-05

**Attendees:** abantikakundu, rupamghosh2006, rupandos

### Decisions

- **D-1:** ArenaX is the launch product: competitive tournaments with NFT rewards.
- **D-2:** Wallet-first authentication (Solana) — no email/password at launch.
- **D-3:** Monorepo split: `GameBackend`, `Frontend`, `DevOps`, `Docs` repositories.

### Action Items

| ID | Item | Owner | Due |
|----|------|-------|-----|
| A-1 | Scaffold backend service | rupandos | 2026-05-12 |
| A-2 | Draft smart contract architecture | rupamghosh2006 | 2026-05-15 |
| A-3 | Set up documentation structure | abantikakundu | 2026-05-12 |
