# ArenaX API Overview

Overview of the ArenaX backend API.

Base URL: `https://api.arenax.gg`

## Authentication

All endpoints require a signed wallet message.

### POST /auth/wallet

Authenticate with a wallet signature.

| Field | Type | Description |
|-------|------|-------------|
| `wallet` | string | Public key of the wallet |
| `signature` | string | Signed challenge message |
| `message` | string | Challenge message that was signed |

**Response:** `{ "token": "<jwt>", "wallet": "<public-key>" }`

## Tournaments

### GET /tournaments

List active tournaments.

**Response:**

```json
{
  "tournaments": [
    {
      "id": "string",
      "name": "string",
      "status": "upcoming | live | completed",
      "entryFee": "number",
      "prizePool": "number"
    }
  ]
}
```

## Rewards

### POST /rewards/distribute

Trigger reward distribution for a completed tournament.

| Field | Type | Description |
|-------|------|-------------|
| `tournamentId` | string | ID of the completed tournament |

**Response:** `{ "status": "processing", "distributionId": "string" }`

## Leaderboard

### GET /leaderboard

Fetch the global leaderboard, optionally filtered by tournament.

| Query | Type | Description |
|-------|------|-------------|
| `tournamentId` | string | Optional tournament filter |
| `limit` | number | Result count (default 50) |

**Response:**

```json
{
  "entries": [
    {
      "wallet": "string",
      "winnings": "number",
      "rank": "number"
    }
  ]
}
```
