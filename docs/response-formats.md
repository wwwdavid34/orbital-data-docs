# Response Formats

The API supports six response formats. Set the format using the `Accept` header or the `format` query parameter.

## Supported formats

| Format | Accept Header | `format` Param | Description |
|--------|---------------|----------------|-------------|
| JSON (OMM) | `application/json` | `json` | Default. Structured OMM data with derived fields. |
| TLE 3-line | `text/plain` | `tle` | Traditional three-line element set (name + two data lines). |
| TLE 2-line | `text/x-tle-2line` | `2le` | Two data lines only (no name line). |
| OMM XML | `application/xml` | `xml` | CCSDS OMM standard in XML. |
| OMM KVN | `text/x-omm-kvn` | `kvn` | CCSDS OMM Keyword-Value Notation. |
| CSV | `text/csv` | `csv` | Comma-separated values with header row. |

## Format negotiation

You can request a format in two ways:

=== "Accept header"

    ```bash
    curl -H "X-Api-Key: YOUR_KEY" \
         -H "Accept: text/plain" \
         https://api.orbitaldata.dev/v1/gp/25544
    ```

=== "Query parameter"

    ```bash
    curl -H "X-Api-Key: YOUR_KEY" \
         "https://api.orbitaldata.dev/v1/gp/25544?format=tle"
    ```

If both are specified, the `format` query parameter takes precedence.

## Format examples

### JSON (default)

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

### TLE 3-line

```
ISS (ZARYA)
1 25544U 98067A   26071.59902778  .00012769  00000-0  22936-3 0  9990
2 25544  51.6416 290.4299 0005730  30.7454 132.9751 15.50238117414250
```

### TLE 2-line

```
1 25544U 98067A   26071.59902778  .00012769  00000-0  22936-3 0  9990
2 25544  51.6416 290.4299 0005730  30.7454 132.9751 15.50238117414250
```

### CSV

```csv
OBJECT_NAME,NORAD_CAT_ID,EPOCH,MEAN_MOTION,ECCENTRICITY,INCLINATION,APOGEE_KM,PERIGEE_KM
ISS (ZARYA),25544,2026-03-12T14:22:37.000Z,15.50238117,0.000573,51.6416,422.1,418.3
```

## TLE format limitations

!!! warning "NORAD ID > 99,999"
    The TLE fixed-width format allocates only 5 characters for the NORAD catalog number. Objects with IDs above 99,999 (increasingly common as the catalog grows) **cannot be represented in TLE or 2LE format**.

    For these objects, use JSON, XML, KVN, or CSV. Requesting TLE format for an object with a high catalog number will return a `400` error.

## List endpoints

For endpoints that return multiple records (e.g., `GET /gp`), the format applies to each record in the collection:

- **JSON**: returns `{ "data": [...], "pagination": {...} }`
- **TLE/2LE**: returns concatenated element sets separated by blank lines
- **CSV**: returns a single CSV with header row and one row per record
- **XML/KVN**: returns a single document containing all records
