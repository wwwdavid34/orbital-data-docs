# Current GP

Current-epoch General Perturbations data — the latest element set for each tracked object.

## `GET /gp/{norad_id}`

Returns the latest GP element set for a single object.

### Parameters

| Parameter | In | Type | Required | Description |
|-----------|----|------|----------|-------------|
| `norad_id` | path | integer | Yes | NORAD catalog number (1–999999999) |
| `format` | query | string | No | Response format: `json`, `tle`, `2le`, `xml`, `kvn`, `csv`. Default: `json` |

### Response headers

| Header | Description |
|--------|-------------|
| `X-Data-Source` | Data source (`spacetrack` or `celestrak`) |
| `X-Epoch-Age-Hours` | Hours since the element set epoch |

### Example

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  https://orbit-dev.davidhsu.cc/api/v1/gp/25544
```

```json
{
  "OBJECT_NAME": "ISS (ZARYA)",
  "OBJECT_ID": "1998-067A",
  "NORAD_CAT_ID": 25544,
  "EPOCH": "2026-03-12T14:22:37.000Z",
  "MEAN_MOTION": 15.50238117,
  "ECCENTRICITY": 0.000573,
  "INCLINATION": 51.6416,
  "RA_OF_ASC_NODE": 290.4299,
  "ARG_OF_PERICENTER": 30.7454,
  "MEAN_ANOMALY": 132.9751,
  "BSTAR": 0.00022936,
  "MEAN_MOTION_DOT": 0.00012769,
  "MEAN_MOTION_DDOT": 0.0,
  "REV_AT_EPOCH": 41425,
  "CLASSIFICATION_TYPE": "U",
  "ELEMENT_SET_NO": 999,
  "PERIOD_MINUTES": 92.87,
  "APOGEE_KM": 422.1,
  "PERIGEE_KM": 418.3,
  "SEMI_MAJOR_AXIS_KM": 6798.3,
  "DATA_SOURCE": "spacetrack",
  "CREATION_DATE": "2026-03-12T18:00:00.000Z",
  "_collected_at": "2026-03-12T19:15:00.000Z"
}
```

### Errors

| Status | When |
|--------|------|
| `404` | NORAD ID not found in catalog |

---

## `GET /gp`

Search and filter current-epoch element sets across multiple objects. Returns the latest element set per matching object.

### Parameters

| Parameter | In | Type | Required | Description |
|-----------|----|------|----------|-------------|
| `search` | query | string | No | Search by object name (case-insensitive, partial match) |
| `group` | query | string | No | Predefined satellite group (see below) |
| `intdes` | query | string | No | International designator (e.g., `2020-025`) — all objects from a launch |
| `norad_ids` | query | string | No | Comma-separated NORAD IDs (max 100) |
| `orbit_regime` | query | string | No | `LEO`, `MEO`, `GEO`, or `HEO` |
| `object_type` | query | string | No | `PAYLOAD`, `ROCKET_BODY`, or `DEBRIS` |
| `decay` | query | boolean | No | Include decayed objects (default: `false`, active only) |
| `format` | query | string | No | Response format. Default: `json` |
| `page` | query | integer | No | Page number (default: 1) |
| `per_page` | query | integer | No | Results per page (1–500, default: 100) |

### Satellite groups

| Group | Description |
|-------|-------------|
| `stations` | Space stations (ISS, CSS, etc.) |
| `starlink` | SpaceX Starlink constellation |
| `oneweb` | OneWeb constellation |
| `gps-ops` | GPS operational satellites |
| `glo-ops` | GLONASS operational satellites |
| `galileo` | Galileo navigation satellites |
| `beidou` | BeiDou navigation satellites |
| `weather` | Weather/Earth observation satellites |
| `geo` | Geostationary satellites |
| `active` | All actively tracked objects |
| `analyst` | Analyst-tracked objects |
| `last-30-days` | Objects cataloged in the last 30 days |

### Example

Search for Starlink satellites:

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  "https://orbit-dev.davidhsu.cc/api/v1/gp?group=starlink&per_page=2"
```

```json
{
  "data": [
    {
      "OBJECT_NAME": "STARLINK-1007",
      "NORAD_CAT_ID": 44713,
      "EPOCH": "2026-03-12T10:00:00.000Z",
      "INCLINATION": 53.0536,
      "APOGEE_KM": 550.2,
      "PERIGEE_KM": 549.8
    },
    {
      "OBJECT_NAME": "STARLINK-1008",
      "NORAD_CAT_ID": 44714,
      "EPOCH": "2026-03-12T09:45:00.000Z",
      "INCLINATION": 53.0541,
      "APOGEE_KM": 550.1,
      "PERIGEE_KM": 549.7
    }
  ],
  "pagination": {
    "total": 6421,
    "page": 1,
    "per_page": 2,
    "total_pages": 3211,
    "next": "https://orbit-dev.davidhsu.cc/api/v1/gp?group=starlink&per_page=2&page=2",
    "prev": null
  }
}
```

Search by name:

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  "https://orbit-dev.davidhsu.cc/api/v1/gp?search=hubble"
```

Multiple NORAD IDs:

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  "https://orbit-dev.davidhsu.cc/api/v1/gp?norad_ids=25544,43873,44506"
```
