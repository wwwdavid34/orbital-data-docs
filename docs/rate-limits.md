# Rate Limits

The API has a single free tier with the following limits:

| Limit | Value |
|-------|-------|
| Requests per second | 10 |
| Burst | 20 |
| Monthly requests | 30,000 |

## Per-endpoint throttle overrides

Historical endpoints have lower rate limits to protect against abuse:

| Endpoint | Rate limit |
|----------|-----------|
| `/gp/{id}/history` | 2 req/s |
| `/gp/{id}/nearest` | 2 req/s |
| `/gp/{id}/adjacent` | 2 req/s |
| All other endpoints | 10 req/s (plan default) |

## Pagination limit

Maximum pagination offset is 10,000. Use filters to narrow results for large datasets.

## Rate limit headers

Every response includes rate limit information:

| Header | Description |
|--------|-------------|
| `X-RateLimit-Limit` | Monthly quota |
| `X-RateLimit-Reset` | UTC epoch seconds when the quota resets (start of next month) |

## Exceeding the limit

If you exceed the rate limit, you'll receive a `429 Too Many Requests` response:

```json
{
  "error": "TooManyRequests",
  "message": "Rate limit exceeded. Try again in 3 seconds.",
  "status": 429
}
```

## Best practices

**Cache responses locally.** Current GP data is updated once per day. There's no need to poll more frequently than every few hours for most use cases.

**Use bulk endpoints.** Instead of requesting one satellite at a time in a loop, use `GET /gp` with filters (e.g., `?group=starlink`) to fetch many records in a single request.

**Respect `X-RateLimit-Limit`.** Implement exponential backoff on `429` responses.
