# Rate Limits

The API has a single free tier with the following limits:

| Limit | Value |
|-------|-------|
| Requests per second | 10 |
| Burst | 20 |
| Monthly requests | 30,000 |

## Rate limit headers

Every response includes rate limit information:

| Header | Description |
|--------|-------------|
| `X-RateLimit-Limit` | Maximum requests allowed per window |
| `X-RateLimit-Remaining` | Requests remaining in current window |
| `X-RateLimit-Reset` | UTC epoch seconds when the window resets |

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

**Cache responses locally.** Current GP data is updated roughly once per day per satellite. There's no need to poll more frequently than every few hours for most use cases.

**Use bulk endpoints.** Instead of requesting one satellite at a time in a loop, use `GET /gp` with filters (e.g., `?group=starlink`) to fetch many records in a single request.

**Respect `X-RateLimit-Remaining`.** Check this header and back off before you hit zero. Implement exponential backoff on `429` responses.

**Use `If-None-Match` / `ETag`.** The API returns `ETag` headers on responses. Send `If-None-Match` with the ETag value on subsequent requests — if the data hasn't changed, you'll get a `304 Not Modified` with no body, which doesn't count against your rate limit.
