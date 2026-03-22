# Status

## `GET /status`

API health check and data freshness. **No authentication required.**

### Example

```bash
curl https://orbit-dev.davidhsu.cc/api/v1/status
```

```json
{
  "status": "healthy",
  "version": "1.0.0",
  "catalog_size": 27032,
  "satcat_size": 68151,
  "daily_elsets": 27032,
  "historical_elsets": 166931394,
  "last_gp_ingest": "2026-03-21T06:00:00.000Z"
}
```

### Response fields

| Field | Type | Description |
|-------|------|-------------|
| `status` | string | `healthy` or `degraded` |
| `version` | string | API version |
| `catalog_size` | integer | Active satellites with current GP data |
| `satcat_size` | integer | Total objects in SATCAT catalog |
| `daily_elsets` | integer | Element sets from last ingest run |
| `historical_elsets` | integer | Cumulative historical element sets (2004–present) |
| `last_gp_ingest` | datetime | Last successful GP data sync from Space-Track |

### Notes

- This is the only endpoint that does not require an API key.
- Use this endpoint for uptime monitoring and to verify data freshness.
- A `degraded` status means the API is operational but data may be stale.
- Response is cached for 30 seconds to absorb burst traffic.
