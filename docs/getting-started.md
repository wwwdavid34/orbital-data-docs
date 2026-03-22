# Getting Started

## Get an API key

1. Visit [orbit-dev.davidhsu.cc/account](https://orbit-dev.davidhsu.cc/account)
2. Sign up with email + password or Google
3. Verify your email
4. Your API key is generated automatically

Your key and usage stats are available on the [account dashboard](https://orbit-dev.davidhsu.cc/account).

## Base URL

All requests go to:

```
https://orbit-dev.davidhsu.cc/api/v1
```

## Authentication

Include your API key in every request using the `X-Api-Key` header:

```
X-Api-Key: YOUR_API_KEY
```

## Your first request

Get the current orbital data for the International Space Station (NORAD ID 25544):

=== "curl"

    ```bash
    curl -H "X-Api-Key: YOUR_KEY" \
      https://orbit-dev.davidhsu.cc/api/v1/gp/25544
    ```

=== "Python"

    ```python
    import requests

    response = requests.get(
        "https://orbit-dev.davidhsu.cc/api/v1/gp/25544",
        headers={"X-Api-Key": "YOUR_KEY"},
    )
    data = response.json()
    print(f"{data['OBJECT_NAME']} — period: {data['PERIOD_MINUTES']:.1f} min")
    ```

=== "JavaScript"

    ```javascript
    const response = await fetch(
      "https://orbit-dev.davidhsu.cc/api/v1/gp/25544",
      { headers: { "X-Api-Key": "YOUR_KEY" } }
    );
    const data = await response.json();
    console.log(`${data.OBJECT_NAME} — period: ${data.PERIOD_MINUTES.toFixed(1)} min`);
    ```

## Response walkthrough

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

| Field | Description |
|-------|-------------|
| `OBJECT_NAME` | Common name of the satellite |
| `NORAD_CAT_ID` | NORAD catalog number (unique identifier) |
| `EPOCH` | When this element set is valid |
| `MEAN_MOTION` | Revolutions per day |
| `ECCENTRICITY` | Orbit shape (0 = circular, 1 = parabolic) |
| `INCLINATION` | Orbit tilt relative to equator (degrees) |
| `BSTAR` | Drag term (atmospheric drag coefficient) |
| `PERIOD_MINUTES` | Orbital period (derived) |
| `APOGEE_KM` / `PERIGEE_KM` | Highest / lowest altitude (derived) |
| `DATA_SOURCE` | Where this record came from |

## Browse without an API key

You can explore satellite data without signing up using the [Satellite Catalog Explorer](https://orbit-dev.davidhsu.cc/explore/). Click any object to view its current orbital elements, and log in to access full historical data.

## Next steps

- [Search and filter satellites](endpoints/current-gp.md) — name search, groups, orbit regime
- [Query historical data](endpoints/historical-gp.md) — time-range queries back to 2004
- [Choose a response format](response-formats.md) — TLE, XML, CSV, and more
