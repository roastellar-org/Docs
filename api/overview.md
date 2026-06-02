# ArenaX API Reference

REST API reference for the ArenaX backend. Base URL: `https://api.arenax.gg` (staging: `https://api.staging.arenax.gg`).

## Authentication

All endpoints except `POST /auth/wallet` and `GET /health` require a bearer token obtained via the wallet flow.

| Header | Value |
|--------|-------|
| `Authorization` | `Bearer <jwt>` |

See [flows/wallet-flow.md](flows/wallet-flow.md) for the full authentication sequence.

## Endpoints

### POST /auth/wallet

Authenticate with a signed challenge message.

| Field | Type | Description |
|-------|------|-------------|
| `wallet` | string | Solana public key |
| `signature` | string | Signed challenge |
| `message` | string | Challenge that was signed |

**200:** `{ "token": "<jwt>", "wallet": "<public-key>", "expiresIn": 86400 }`
**401:** invalid signature or expired challenge

### GET /tournaments

List tournaments with optional filters.

| Query | Type | Description |
|-------|------|-------------|
| `status` | string | `upcoming`, `live`, `completed` |
| `game` | string | Game filter |
| `page` / `limit` | number | Pagination (default limit 50) |

**200:**

```json
{
  "tournaments": [
    { "id": "t-118", "name": "ArenaX Open #118", "game": "FPS", "status": "live", "entryFee": 1.5, "prizePool": 300 }
  ],
  "page": 1,
  "total": 1
}
```

### GET /tournaments/:id

Tournament details, bracket, and registrations.

**200:** full tournament object including participants and match schedule.

### POST /tournaments/:id/register

Register the authenticated wallet for a tournament.

**201:** `{ "registrationId": "r-4451", "status": "confirmed" }`
**409:** already registered or tournament full.

### POST /rewards/distribute

Trigger reward distribution for a completed tournament.

| Field | Type | Description |
|-------|------|-------------|
| `tournamentId` | string | Completed tournament ID |

**202:** `{ "status": "processing", "distributionId": "d-9021" }`

Idempotent: calling twice returns the same `distributionId`.

### GET /leaderboard

Global leaderboard, optionally filtered by tournament.

| Query | Type | Description |
|-------|------|-------------|
| `tournamentId` | string | Optional filter |
| `limit` | number | Default 50, max 200 |

**200:**

```json
{
  "entries": [
    { "rank": 1, "wallet": "9x...", "winnings": 150.0 }
  ]
}
```

### GET /rewards/wallet/:wallet

Reward NFTs held by a wallet.

**200:** array of `{ nftMint, tournamentId, rank, prize, metadataUri }`.

## Errors

| Code | Meaning |
|------|---------|
| `400` | Invalid request body |
| `401` | Missing/invalid token |
| `404` | Resource not found |
| `409` | Conflict (already registered, duplicate distribution) |
| `429` | Rate limit exceeded |
| `500` | Internal error |

Standard error body: `{ "error": { "code": "string", "message": "string" } }`

## Rate Limits

- 60 requests/min per wallet for read endpoints
- 10 requests/min for writes
- Headers `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`

## Flows

Detailed request sequences:

- [Wallet Authentication Flow](flows/wallet-flow.md)
- [NFT Reward Flow](flows/nft-reward-flow.md)
- [Tournament Flow](flows/tournament-flow.md)
- [Bridge Flow](flows/bridge-flow.md)
