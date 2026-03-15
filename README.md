# Whack API — Railway Deployment

## Deploy Steps

1. Push this `api/` folder to a GitHub repo
2. Go to railway.app → New Project → Deploy from GitHub repo
3. Select the repo
4. Go to **Variables** tab and add:

```
SUPABASE_URL      = https://svhpyokkvjnpmdaxdpem.supabase.co
SUPABASE_SERVICE_KEY = your_service_role_key_here
PORT              = 3000
```

5. Railway auto-builds via Dockerfile and deploys
6. Copy your Railway URL (e.g. https://whack-api.up.railway.app)
7. Paste that URL into `lua/Whack/Config.lua` as `API_BASE`

## Endpoints (all require X-Api-Key + X-Timestamp headers)

| Method | Path | Description |
|--------|------|-------------|
| GET | /health | Health check (no auth) |
| POST | /game/heartbeat | Server alive ping + player list |
| POST | /game/join | Player join — ban check |
| POST | /game/leave | Player leave — save playtime |
| POST | /game/flag | Report a cheat detection |
| GET | /game/commands | Poll pending dashboard commands |
| POST | /game/commands/ack | Acknowledge a command was executed |

## Security

- Every request needs `X-Api-Key` (format: `whk_[64 hex chars]`)
- Every request needs `X-Timestamp` (Unix seconds — rejected if >30s old)
- Rate limited per game per endpoint
- All inputs validated before any DB query
- Errors never leak internal details
