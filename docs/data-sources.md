# Data Sources

## Primary source: Space-Track.org

All GP/TLE element sets come from [Space-Track.org](https://www.space-track.org), the public catalog maintained by the **18th Space Defense Squadron (18 SDS)** of the **United States Space Command (USSPACECOM)**.

Space-Track provides General Perturbations (GP) data — the mathematical models that describe satellite orbits. Each element set contains the orbital parameters needed to predict a satellite's position using SGP4/SDP4 propagators.

## Data types

### GP element sets (TLE/OMM)

The core data product. Each record describes a satellite's orbit at a specific epoch:

- **Keplerian elements**: inclination, eccentricity, RAAN, argument of perigee, mean anomaly, mean motion
- **Drag terms**: B* coefficient, mean motion derivatives
- **Metadata**: NORAD catalog ID, international designator, classification, element set number

These are published in two equivalent representations:

- **TLE (Two-Line Element)**: legacy fixed-width text format, widely supported
- **OMM (Orbit Mean-elements Message)**: modern CCSDS standard, supports JSON/XML/KVN

### SATCAT (Satellite Catalog)

Metadata about each tracked object:

- Object name, type (payload, rocket body, debris)
- Owner/operator country
- Launch date and site
- Operational status
- Decay date (if applicable)

## Historical coverage

| Metric | Value |
|--------|-------|
| Coverage period | 2004 to present |
| Total element sets | ~167 million |
| Cataloged objects | ~68,000 (SATCAT) |
| Active satellites | ~27,000 (with current GP) |
| Update cadence | Daily (GP history archived, current GP refreshed) |

## Data quality notes

- **TLE format limitations**: The traditional TLE format cannot represent NORAD catalog IDs above 99,999. For newer objects with higher IDs, use JSON, XML, KVN, or CSV formats.
- **Duplicate handling**: Space-Track occasionally publishes multiple element sets with the same epoch for an object. We deduplicate by keeping the most recently published version.
- **Epoch gaps**: Some objects have irregular tracking cadence. Gaps in element set availability are normal, especially for debris and objects in highly elliptical orbits.
- **Derived fields**: `PERIOD_MINUTES`, `APOGEE_KM`, `PERIGEE_KM`, and `SEMI_MAJOR_AXIS_KM` are computed from the mean elements. These are approximate and assume a spherical Earth (WGS-84 equatorial radius).

## Required citation

When using data from the Orbital Data API, you must cite the original source:

!!! important "Data Attribution"
    **Source: USSPACECOM via Space-Track.org**

    Per Space-Track's terms of use, all users of derived data must acknowledge the original source.

See [Attribution](attribution.md) for full details.
