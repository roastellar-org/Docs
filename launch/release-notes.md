# Release Notes

## v1.0.0 — Launch

**Release date:** 2026-08-01
**Type:** Initial public release

### Highlights

- Wallet-based authentication with Phantom, Solflare, and Backpack
- Tournament hub: create, register, play, and track standings
- On-chain reward distribution via Reward NFTs
- Global leaderboard with per-tournament filters
- Marketplace for trading Reward NFTs

### New Features

- `POST /auth/wallet` wallet login with signed challenges ([wallet flow](../api/flows/wallet-flow.md))
- `GET /tournaments` with status and game filters
- `POST /tournaments/:id/register` entry with fee handling
- `POST /rewards/distribute` idempotent payout trigger
- `GET /leaderboard` with wallet winnings aggregation
- Reward NFT minting with IPFS-hosted metadata

### Improvements

- Multi-stage Docker builds for smaller images
- CI split into build/test/deploy pipelines with matrix builds
- Prometheus + Grafana observability with 5 alert rules
- NGINX gateway with TLS and security headers
- Terraform-managed EKS infrastructure

### Fixed Issues

| Issue | Reference |
|-------|-----------|
| Reward distribution stalled on RPC rate limits | [IR-0042](../incidents/reward-mint-failure.md) |
| Leaderboard inconsistent with payout snapshot | [IR-0047](../incidents/leaderboard-inconsistency.md) |
| Wallet login failing with Phantom serialization | [IR-0051](../incidents/wallet-login-issue.md) |
| Deployment race condition on DB readiness | [DevOps](https://github.com/roastellar-org/DevOps) `#19` |
| Readiness probe pointed at wrong endpoint | [DevOps](https://github.com/roastellar-org/DevOps) `#20` |

### Known Issues

- Asset Bridge supports Solana only; other chains follow in a later release
- Marketplace offers do not yet support auctions (fixed price only)
- iOS Safari occasionally drops wallet sessions after backgrounding

### Deprecations

- None (initial release).

### Breaking Changes

- None (initial release).

See [version history](version-history.md) for previous milestones.
