# Endpoints

Base URL: `https://orbit-dev.davidhsu.cc/api/v1`

All endpoints require an API key via the `X-Api-Key` header, except `/status`, `/explore/*`, and `/feedback`.

## Current GP

| Method | Path | Description |
|--------|------|-------------|
| `GET` | [`/gp/{norad_id}`](current-gp.md#get-gpnorad_id) | Latest element set for one object |
| `GET` | [`/gp`](current-gp.md#get-gp) | Search/filter current element sets |

## Historical GP

| Method | Path | Description |
|--------|------|-------------|
| `GET` | [`/gp/{norad_id}/history`](historical-gp.md#get-gpnorad_idhistory) | Historical element sets (time-range query) |
| `GET` | [`/gp/{norad_id}/nearest`](historical-gp.md#get-gpnorad_idnearest) | Nearest element set to a given epoch |
| `GET` | [`/gp/{norad_id}/adjacent`](historical-gp.md#get-gpnorad_idadjacent) | Bracketing element sets around an epoch |
| `GET` | [`/gp/epoch`](epoch-bulk.md) | Bulk snapshot — all objects at a point in time |

## Catalog

| Method | Path | Description |
|--------|------|-------------|
| `GET` | [`/catalog/{norad_id}`](catalog.md#get-catalognorad_id) | Satellite metadata for one object |
| `GET` | [`/catalog`](catalog.md#get-catalog) | Search satellite catalog |

## Explorer (no auth required)

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/explore/catalog` | Browse satellite catalog |
| `GET` | `/explore/gp/{norad_id}` | Current GP + metadata for one object |

## Feedback

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/feedback` | Submit feedback (Turnstile captcha required) |

## System

| Method | Path | Description |
|--------|------|-------------|
| `GET` | [`/status`](status.md) | API health and data freshness (no auth required) |
