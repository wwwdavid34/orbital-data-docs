# Historical GP

Historical element sets with time-range queries. Data goes back to 2001.

## Date formats

All date parameters (`start`, `end`, `epoch`) accept three formats:

| Format | Example | Description |
|--------|---------|-------------|
| ISO 8601 | `2024-01-01T00:00:00Z` | Standard datetime |
| Julian Date | `jd:2460310.5` | Prefix with `jd:` |
| Unix epoch | `unix:1704067200` | Prefix with `unix:` |

---

## `GET /gp/{norad_id}/history`

Returns all GP element sets for an object within a time range.

### Parameters

| Parameter | In | Type | Required | Description |
|-----------|----|------|----------|-------------|
| `norad_id` | path | integer | Yes | NORAD catalog number |
| `start` | query | string | No | Start of time range |
| `end` | query | string | No | End of time range |
| `data_source` | query | string | No | `spacetrack`, `celestrak`, or `all` (default: `all`) |
| `format` | query | string | No | Response format. Default: `json` |
| `page` | query | integer | No | Page number (default: 1) |
| `per_page` | query | integer | No | Results per page (1–500, default: 100) |

### Example

Get ISS element sets for January 2024:

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  "https://api.orbitaldata.dev/v1/gp/25544/history?start=2024-01-01T00:00:00Z&end=2024-02-01T00:00:00Z"
```

```json
{
  "data": [
    {
      "OBJECT_NAME": "ISS (ZARYA)",
      "NORAD_CAT_ID": 25544,
      "EPOCH": "2024-01-01T03:12:45.000Z",
      "MEAN_MOTION": 15.49712345,
      "INCLINATION": 51.6412,
      "APOGEE_KM": 423.5,
      "PERIGEE_KM": 419.1
    }
  ],
  "pagination": {
    "total": 47,
    "page": 1,
    "per_page": 100,
    "total_pages": 1,
    "next": null,
    "prev": null
  },
  "meta": {
    "norad_id": 25544,
    "object_name": "ISS (ZARYA)",
    "earliest_epoch": "2024-01-01T03:12:45.000Z",
    "latest_epoch": "2024-01-31T21:45:12.000Z",
    "total_elsets": 47
  }
}
```

---

## `GET /gp/{norad_id}/nearest`

Returns the single element set closest in time to the specified epoch. Useful for propagation at a specific historical time.

### Parameters

| Parameter | In | Type | Required | Description |
|-----------|----|------|----------|-------------|
| `norad_id` | path | integer | Yes | NORAD catalog number |
| `epoch` | query | string | Yes | Target epoch |
| `data_source` | query | string | No | `spacetrack`, `celestrak`, or `all` (default: `all`) |
| `format` | query | string | No | Response format. Default: `json` |

### Response headers

| Header | Description |
|--------|-------------|
| `X-Epoch-Delta-Seconds` | Signed offset from requested epoch (seconds). Negative = element set is before the requested epoch. |

### Example

Find the nearest element set to a specific time:

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  "https://api.orbitaldata.dev/v1/gp/25544/nearest?epoch=2024-06-15T12:00:00Z"
```

```json
{
  "OBJECT_NAME": "ISS (ZARYA)",
  "NORAD_CAT_ID": 25544,
  "EPOCH": "2024-06-15T11:45:22.000Z",
  "MEAN_MOTION": 15.50112345,
  "INCLINATION": 51.6419,
  "APOGEE_KM": 421.8,
  "PERIGEE_KM": 417.9
}
```

The `X-Epoch-Delta-Seconds` header would be `-878` (the element set epoch is 878 seconds before the requested time).

---

## `GET /gp/{norad_id}/adjacent`

Returns the two element sets immediately before and after the specified epoch. Useful for interpolation or maneuver detection around a known event time.

### Parameters

| Parameter | In | Type | Required | Description |
|-----------|----|------|----------|-------------|
| `norad_id` | path | integer | Yes | NORAD catalog number |
| `epoch` | query | string | Yes | Target epoch |
| `format` | query | string | No | Response format. Default: `json` |

### Example

Get bracketing element sets around a specific time:

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  "https://api.orbitaldata.dev/v1/gp/25544/adjacent?epoch=2024-06-15T12:00:00Z"
```

```json
{
  "before": {
    "OBJECT_NAME": "ISS (ZARYA)",
    "NORAD_CAT_ID": 25544,
    "EPOCH": "2024-06-15T11:45:22.000Z",
    "MEAN_MOTION": 15.50112345,
    "INCLINATION": 51.6419
  },
  "after": {
    "OBJECT_NAME": "ISS (ZARYA)",
    "NORAD_CAT_ID": 25544,
    "EPOCH": "2024-06-15T14:30:11.000Z",
    "MEAN_MOTION": 15.50115678,
    "INCLINATION": 51.6420
  },
  "gap_hours": 2.75
}
```
