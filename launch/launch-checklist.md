# ArenaX Launch Checklist

Final pre-launch checklist for v1.0.0. Each item has an owner and must be verified against its source document.

## Product

- [x] Tournament rules and prize structure finalized — abantikakundu
- [x] Supported games and formats confirmed — abantikakundu
- [x] User onboarding flow complete — see [wallet flow](../api/flows/wallet-flow.md)
- [ ] Ambassadors and community moderation briefed

## Blockchain

- [x] Reward NFT contract deployed to mainnet — see [smart contracts](../architecture/smart-contracts.md)
- [x] Marketplace contract audited (audit report v1.2, 2026-07-05)
- [x] Treasury wallet configured with 3-of-5 multisig
- [x] Bridge program devnet-tested — see [bridge flow](../api/flows/bridge-flow.md)

## Backend

- [x] All endpoints verified against [API reference](../api/overview.md)
- [x] Load testing passed (300 rps, p95 < 800ms)
- [x] Monitoring and alerting active — see [monitoring guide](../ops/monitoring-guide.md)
- [x] Rollback runbook rehearsed — see [rollback procedure](../ops/rollback-procedure.md)

## Frontend

- [x] QA pass on Chrome, Firefox, Safari, mobile
- [x] Wallet connect verified with Phantom, Solflare, Backpack
- [x] Analytics and error tracking configured
- [ ] A/B experiment tooling verified

## Operations

- [x] On-call rotation published
- [x] Support channels staffed (email, Discord)
- [x] Staging environment green for 7 consecutive days
- [x] Backup/restore drill completed (RPO 24h, RTO 2h)

## Launch Day

- [ ] Freeze window agreed (no deploys 24h prior)
- [ ] Release notes published — see [release notes](release-notes.md)
- [ ] Marketing assets live
- [ ] Emergency contact sheet printed/distributed
- [ ] Go/no-go confirmed by all owners

## Post-Launch

- [ ] 24h monitoring watch
- [ ] Post-launch review scheduled (D+3)
- [ ] First incident drill documented

See [version history](version-history.md) for the release cadence.
