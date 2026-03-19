# Catalog

Satellite catalog (SATCAT) metadata — information about tracked objects such as name, owner, launch details, and operational status.

## `GET /catalog/{norad_id}`

Returns catalog metadata for a single object.

### Parameters

| Parameter | In | Type | Required | Description |
|-----------|----|------|----------|-------------|
| `norad_id` | path | integer | Yes | NORAD catalog number |

### Example

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  https://api.orbitaldata.dev/v1/catalog/25544
```

```json
{
  "NORAD_CAT_ID": 25544,
  "OBJECT_NAME": "ISS (ZARYA)",
  "OBJECT_ID": "1998-067A",
  "OBJECT_TYPE": "PAYLOAD",
  "OPS_STATUS_CODE": "+",
  "OWNER": "ISS",
  "LAUNCH_DATE": "1998-11-20",
  "LAUNCH_SITE": "TTMTR",
  "DECAY_DATE": null,
  "PERIOD_MINUTES": 92.87,
  "INCLINATION_DEG": 51.64,
  "APOGEE_KM": 422.1,
  "PERIGEE_KM": 418.3
}
```

### Response fields

| Field | Type | Description |
|-------|------|-------------|
| `NORAD_CAT_ID` | integer | NORAD catalog number |
| `OBJECT_NAME` | string | Common name |
| `OBJECT_ID` | string | International designator (launch year + piece) |
| `OBJECT_TYPE` | string | `PAYLOAD`, `ROCKET BODY`, `DEBRIS`, or `UNKNOWN` |
| `OPS_STATUS_CODE` | string | Operational status (`+`=active, `-`=inactive, `P`=partial, `D`=decayed, etc.) |
| `OWNER` | string | Owner/operator country or organization code |
| `LAUNCH_DATE` | date | Launch date |
| `LAUNCH_SITE` | string | Launch site code |
| `DECAY_DATE` | date or null | Reentry date (null if still in orbit) |
| `PERIOD_MINUTES` | number or null | Orbital period |
| `INCLINATION_DEG` | number or null | Orbital inclination |
| `APOGEE_KM` | number or null | Apogee altitude |
| `PERIGEE_KM` | number or null | Perigee altitude |

---

## `GET /catalog`

Search the satellite catalog.

### Parameters

| Parameter | In | Type | Required | Description |
|-----------|----|------|----------|-------------|
| `search` | query | string | No | Search by object name (partial match) |
| `owner` | query | string | No | Owner/country code (e.g., `US`, `CIS`, `PRC`) |
| `object_type` | query | string | No | `PAYLOAD`, `ROCKET_BODY`, or `DEBRIS` |
| `launch_year` | query | integer | No | Filter by launch year |
| `active_only` | query | boolean | No | Only active objects (default: `true`) |
| `page` | query | integer | No | Page number (default: 1) |
| `per_page` | query | integer | No | Results per page (1–500, default: 100) |

### Example

Search for active Chinese payloads:

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  "https://api.orbitaldata.dev/v1/catalog?owner=PRC&object_type=PAYLOAD&per_page=2"
```

```json
{
  "data": [
    {
      "NORAD_CAT_ID": 48274,
      "OBJECT_NAME": "CSS (TIANHE)",
      "OBJECT_ID": "2021-035A",
      "OBJECT_TYPE": "PAYLOAD",
      "OPS_STATUS_CODE": "+",
      "OWNER": "PRC",
      "LAUNCH_DATE": "2021-04-29",
      "DECAY_DATE": null
    }
  ],
  "pagination": {
    "total": 312,
    "page": 1,
    "per_page": 2,
    "total_pages": 156,
    "next": "https://api.orbitaldata.dev/v1/catalog?owner=PRC&object_type=PAYLOAD&per_page=2&page=2",
    "prev": null
  }
}
```
