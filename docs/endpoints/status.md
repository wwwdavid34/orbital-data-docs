# Status

## `GET /status`

API health check and data freshness. **No authentication required.**

### Example

```bash
curl https://api.orbitaldata.dev/v1/status
```

```json
{
  "status": "healthy",
  "version": "1.0.0",
  "catalog_size": 49823,
  "historical_elsets": 172456789,
  "last_gp_ingest": "2026-03-16T06:00:00.000Z",
  "last_celestrak_ingest": "2026-03-16T06:15:00.000Z",
  "last_analysis_run": "2026-03-16T07:00:00.000Z"
}
```

### Response fields

| Field | Type | Description |
|-------|------|-------------|
| `status` | string | `healthy` or `degraded` |
| `version` | string | API version |
| `catalog_size` | integer | Total objects in current catalog |
| `historical_elsets` | integer | Total historical element sets stored |
| `last_gp_ingest` | datetime | Last successful GP data sync from Space-Track |
| `last_celestrak_ingest` | datetime | Last successful CelesTrak sync |
| `last_analysis_run` | datetime | Last analysis pipeline run |

### Notes

- This is the only endpoint that does not require an API key.
- Use this endpoint for uptime monitoring and to verify data freshness.
- A `degraded` status means the API is operational but data may be stale (ingest has not completed recently).
