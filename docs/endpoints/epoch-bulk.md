# Epoch Bulk

## `GET /gp/epoch`

Returns the nearest element set for every tracked object at a given epoch. This is a bulk endpoint — use pagination.

### Parameters

| Parameter | In | Type | Required | Description |
|-----------|----|------|----------|-------------|
| `epoch` | query | string | No | Target epoch (defaults to current time). Accepts ISO 8601, `jd:`, or `unix:` formats. |
| `object_type` | query | string | No | `PAYLOAD`, `ROCKET_BODY`, or `DEBRIS` |
| `orbit_regime` | query | string | No | `LEO`, `MEO`, `GEO`, or `HEO` |
| `format` | query | string | No | Response format. Default: `json` |
| `page` | query | integer | No | Page number (default: 1) |
| `per_page` | query | integer | No | Results per page (1–500, default: 100) |

### Example

Get all LEO payloads at a specific epoch:

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  "https://api.orbitaldata.dev/v1/gp/epoch?epoch=2024-06-15T12:00:00Z&orbit_regime=LEO&object_type=PAYLOAD&per_page=2"
```

```json
{
  "data": [
    {
      "OBJECT_NAME": "ISS (ZARYA)",
      "NORAD_CAT_ID": 25544,
      "EPOCH": "2024-06-15T11:45:22.000Z",
      "INCLINATION": 51.6419,
      "APOGEE_KM": 421.8,
      "PERIGEE_KM": 417.9
    },
    {
      "OBJECT_NAME": "HST",
      "NORAD_CAT_ID": 20580,
      "EPOCH": "2024-06-15T10:30:00.000Z",
      "INCLINATION": 28.4698,
      "APOGEE_KM": 540.2,
      "PERIGEE_KM": 537.8
    }
  ],
  "pagination": {
    "total": 8234,
    "page": 1,
    "per_page": 2,
    "total_pages": 4117,
    "next": "https://api.orbitaldata.dev/v1/gp/epoch?epoch=2024-06-15T12:00:00Z&orbit_regime=LEO&object_type=PAYLOAD&per_page=2&page=2",
    "prev": null
  }
}
```

### Notes

- This endpoint can return large result sets. Use `object_type` and `orbit_regime` filters to narrow results.
- The epoch parameter determines the snapshot time. Each returned record is the element set with the epoch closest to the requested time for that object.
- If `epoch` is omitted, the current time is used, which effectively returns the same data as `GET /gp` (latest element sets).
