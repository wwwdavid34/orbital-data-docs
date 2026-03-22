# Orbital Data API


**Free, developer-friendly REST API for satellite orbital data.**

The Orbital Data API gives you simple access to General Perturbations (GP) element sets — the TLE and OMM data that describes where satellites are and where they're going. Historical data back to 2001, multiple response formats, and a clean REST interface.

## Why?

Space data shouldn't be hard to get. Space-Track.org is the authoritative source, but its interface was built for military operators, not developers building apps. Orbital Data API sits in front of the raw data and gives you:

- **One API key, one base URL** — no session cookies or multi-step auth flows
- **Historical depth** — ~170 million element sets going back to 2001
- **Multiple formats** — JSON, TLE, 2LE, XML, KVN, CSV
- **Pagination and filtering** — search by name, group, orbit regime, object type
- **Satellite catalog** — SATCAT metadata (owner, launch date, status) alongside orbital data

## Quick example

```bash
curl -H "X-Api-Key: YOUR_KEY" \
  https://orbit-dev.davidhsu.cc/api/v1/gp/25544
```

```json
{
  "OBJECT_NAME": "ISS (ZARYA)",
  "NORAD_CAT_ID": 25544,
  "EPOCH": "2026-03-12T14:22:37.000Z",
  "MEAN_MOTION": 15.50238117,
  "ECCENTRICITY": 0.000573,
  "INCLINATION": 51.6416,
  "APOGEE_KM": 422.1,
  "PERIGEE_KM": 418.3
}
```

## Community-funded

Orbital Data API is free infrastructure for the space community, funded by donations. If you find it useful, consider supporting the project:

- [GitHub Sponsors](https://github.com/sponsors/wwwdavid34)
- [Open Collective](https://opencollective.com/orbital-data)

## Get started

- [**Getting Started**](getting-started.md) — get an API key and make your first request
- [**Endpoints**](endpoints/index.md) — full API reference
- [**Data Sources**](data-sources.md) — where the data comes from
- [**Response Formats**](response-formats.md) — JSON, TLE, XML, and more
