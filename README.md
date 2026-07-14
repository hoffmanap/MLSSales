# It Pays to Be Patient

**Live story:** https://hoffmanap.github.io/MLSSales/

How above-average days on market net higher sold prices per square foot — a look at closed home
sales in El Paso County, TX, and why the answer depends on the geography you measure it at.

## What this is

An interactive HTML dashboard analyzing the relationship between sale speed (days on market) and
price density (sold price per square foot) across closed MLS transactions. The map lets you toggle
between two geographies — hexbins and census tracts — and two metrics — $/sqft and days on market —
while the correlation panel shows how that relationship holds up (or doesn't) depending on the
spatial resolution used.

## Key finding

The relationship between days on market and $/sqft is weak at the individual-sale level, disappears
entirely when aggregated to census tracts, and re-emerges as a real, moderate positive relationship
when aggregated to ~0.46 km hexbins:

| Level | r | Significant? |
|---|---|---|
| Individual sales (n = 12,853) | 0.12 | Yes, but small effect |
| Census tract medians (n = 179) | 0.12 | No (p ≈ 0.12) |
| Hexbin medians (n = 490) | 0.21 | Yes (p < .001) |

Census tracts are sized by population, not by housing submarket, so they average together
distinct pricing pockets and wash the pattern out. Hexbins — equal-area cells — hold up the
relationship instead, and it gets stronger, not weaker, at finer resolution.

At the hex level, 73% of sales fall into one of the two "expected" quadrants (fast-and-affordable
or slow-and-pricey), versus 27% in the two quadrants that run against the trend
(fast-and-pricey or slow-and-affordable).

## Data

- **Source:** `MLS_Geocoded_Backup.csv` — 12,901 MLS listings, 12,853 retained after cleaning
  (positive square footage, 0–730 days on market, $20–$600/sqft sold price).
- **Coordinates:** rooftop-level geocodes on 10,755 of the cleaned records (83.7%); 97.8% of
  geocoded addresses resolve to a unique coordinate.
- **Hex grid:** Uber H3, resolution 8 (~0.46 km edge), cells with fewer than 5 sales excluded from
  the map and correlation stats.
- **Census tracts:** US Census TIGER 2021, El Paso County (COUNTYFP 141).
- **Location labels:** the two most common street names among the actual MLS addresses inside each
  hex/tract, plus the dominant zip code — a data-derived stand-in for "nearest cross streets," not
  an official neighborhood name.

## Caveats

- Correlation, not causation — this doesn't control for home condition, listing strategy, agent,
  seasonality, or other confounders that plausibly affect both DOM and price.
- Hex- and tract-level correlations are based on aggregated medians (n = 490 and 179 respectively);
  the record-level correlation (n = 12,853) is the only one with individual-sale-level power.
- 2,098 cleaned sales (16.3%) lack a usable coordinate and are included in the countywide/record-level
  figures but not on the map.
