# ArenaX Architecture Overview

System-level view of the ArenaX platform: what it is, which components exist, and how they interact.

## System Context

```mermaid
graph TB
    subgraph Players
        U[Player Browser]
        W[Wallet Extension]
    end
    subgraph ArenaX Platform
        F[Web Frontend]
        B[Backend API]
        SC[Solana Programs]
        TR[Treasury]
    end
    subgraph Infrastructure
        NG[NGINX Gateway]
        DB[(PostgreSQL)]
        RD[(Redis)]
        PG[Prometheus / Grafana]
    end
    U --> F
    W <-->|sign messages / transactions| B
    F --> NG
    NG --> B
    B --> DB
    B --> RD
    B --> SC
    SC --> TR
    B --> PG
```

## Components

| Component | Responsibility | Repository |
|-----------|---------------|------------|
| Web Frontend | Tournament hub, wallet connect, leaderboard UI | Frontend |
| Backend API | Auth, tournaments, rewards orchestration, leaderboards | [GameBackend](https://github.com/roastellar-org/GameBackend) |
| Reward NFT Contract | Mint winners' NFTs | GameBackend `contracts/` |
| Marketplace Contract | Trading of reward NFTs | GameBackend `contracts/` |
| Asset Bridge | Cross-chain reward settlement | GameBackend `contracts/` |
| Treasury | Custody of fees and unclaimed rewards | GameBackend `contracts/` |
| NGINX | TLS termination, reverse proxy | [DevOps](https://github.com/roastellar-org/DevOps) |
| PostgreSQL | Primary data store | [DevOps](https://github.com/roastellar-org/DevOps) |
| Redis | Session cache, queue buffers | [DevOps](https://github.com/roastellar-org/DevOps) |
| Prometheus / Grafana | Metrics and dashboards | [DevOps](https://github.com/roastellar-org/DevOps) |

## Core Flows

```mermaid
sequenceDiagram
    participant P as Player
    participant B as Backend
    participant S as Solana Programs
    participant T as Treasury

    P->>B: Connect wallet + sign challenge
    B-->>P: JWT session
    P->>B: Register for tournament
    B-->>P: Registration confirmed
    Note over P,B: Tournament runs on platform
    B->>S: Complete tournament, snapshot standings
    S->>T: Mint Reward NFTs per prize structure
    S-->>B: Mint receipts
    B-->>P: Rewards visible on leaderboard
```

## Environments

| Environment | Purpose | Deployed By |
|-------------|---------|-------------|
| Staging | Integration testing | CI on `main` pushes |
| Production | Player-facing | CI on version tags |

See [ops/deployment-guide.md](../ops/deployment-guide.md) for environment details.

## Related Documents

- [Backend Architecture](backend.md)
- [Smart Contract Architecture](smart-contracts.md)
- [API Reference](../api/overview.md)
