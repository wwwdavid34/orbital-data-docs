# Errors

All error responses use a consistent JSON schema:

```json
{
  "error": "ErrorType",
  "message": "Human-readable description of what went wrong.",
  "status": 400
}
```

## Error codes

| Status | Error | Description |
|--------|-------|-------------|
| `400` | `BadRequest` | Invalid parameters (e.g., malformed NORAD ID, invalid date format, TLE requested for high catalog number) |
| `403` | `Forbidden` | Missing or invalid API key |
| `404` | `NotFound` | Object not found in catalog, or no element sets matching query |
| `429` | `TooManyRequests` | Rate limit exceeded — see [Rate Limits](rate-limits.md) |
| `500` | `InternalError` | Unexpected server error — if persistent, please [report it](https://github.com/orbital-data/docs/issues) |

## Common scenarios

### Invalid NORAD ID

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  https://api.orbitaldata.dev/v1/gp/0
```

```json
{
  "error": "BadRequest",
  "message": "NORAD catalog ID must be between 1 and 999999999.",
  "status": 400
}
```

### Object not found

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  https://api.orbitaldata.dev/v1/gp/999999999
```

```json
{
  "error": "NotFound",
  "message": "No element set found for NORAD ID 999999999.",
  "status": 404
}
```

### Missing API key

```bash
curl https://api.orbitaldata.dev/v1/gp/25544
```

```json
{
  "error": "Forbidden",
  "message": "Missing or invalid API key. Include X-Api-Key header.",
  "status": 403
}
```

### Rate limit exceeded

```json
{
  "error": "TooManyRequests",
  "message": "Rate limit exceeded. Try again in 3 seconds.",
  "status": 429
}
```
