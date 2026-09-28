# user_activity_metrics

Grain (a guess from the data, confirm it): no id column found. 998 rows.

## Columns

| Name | Type | Null rate | Distinct |
| --- | --- | --- | --- |
| date | datetime64[ns] | 0.00% | 998 |
| dau | int64 | 0.00% | 842 |
| wau | int64 | 0.00% | 950 |
| mau | int64 | 0.00% | 985 |

## Date ranges

- `date`: 2024-01-01T00:00:00 to 2026-09-24T00:00:00

## Low-cardinality values

- None

## Meaning

Grain in plain words (inferred): one row per calendar day, 2024-01-01 to 2026-09-24, with no gaps (998 distinct dates over 998 rows).

- `date`: the day measured.
- `dau`: users active that day (inferred).
- `wau`: users active in the 7 days ending that day (inferred).
- `mau`: users active in the 30 days ending that day (inferred). The DAU/MAU ratio falls at September 2025 (quirk "September 2025 spike").

This table is platform-wide only: it has no user_id, plan or segment, so it cannot say how any group of users behaved.

