# subscriptions

Grain (a guess from the data, confirm it): one row per subscription_id. 30,747 rows.

## Columns

| Name | Type | Null rate | Distinct |
| --- | --- | --- | --- |
| subscription_id | int64 | 0.00% | 30,747 |
| user_id | int64 | 0.00% | 30,747 |
| plan | object | 0.00% | 3 |
| start_date | datetime64[ns] | 0.00% | 9,584 |
| end_date | datetime64[ns] | 87.90% | 1,789 |
| status | object | 0.00% | 3 |

## Date ranges

- `start_date`: 2022-01-11T00:00:00 to 2026-10-04T17:15:33
- `end_date`: 2022-03-07T00:00:00 to 2026-11-11T18:47:46

## Low-cardinality values

- `plan`: free (18,234), monthly (9,376), annual (3,137)
- `status`: active (27,028), expired (2,929), canceled (790)

## Meaning

Grain in plain words (inferred): one row per user's subscription. `user_id` is as distinct as `subscription_id`, so each user has exactly one subscription row and plan changes (upgrade, downgrade) are not recorded as history.

- `subscription_id`: the subscription's key; joins to `payments.subscription_id` (inferred).
- `user_id`: the user who holds it; one row per user, joins to `users.user_id` (inferred; all 30,747 match a user as of 2026-09-28).
- `plan`: free, monthly or annual. Free rows never end (quirk "Free plans never end"); exclude them from churn and retention.
- `start_date`: when the subscription began. Max is in the future (2026-10-04): 47 scheduled starts, all free as of 2026-09-28 (quirk "Future start dates").
- `end_date`: when the subscription ended or is scheduled to end. Null means still running; the 87.90% null rate is exactly the `active` rows. Values after today are scheduled cancellations and those subscriptions are still active today (quirk "Future end dates").
- `status`: active, expired or canceled. Every row with an `end_date` is expired or canceled and every row without one is active (inferred from a crosstab, 2026-09-28). The label is not a behaviour: every end_date up to July 2026 is `expired` and every end_date from August 2026 on (including future ones) is `canceled`, so the label changed around August 2026. Do not use it to split voluntary cancellations from lapses, and do not treat `canceled` as "ended already" (not yet in quirks.md; proposed 2026-09-28).


**Batch-stamped start dates (not yet in quirks.md; proposed 2026-09-28).** Three days carry far more starts than any normal day, all at midnight: 2025-09-17 (3,377, the September 2025 spike), 2026-07-30 (1,052, of which 403 paid) and 2026-09-25 (1,468, after the data cutoff). A month-end count of the paid or active base jumps on those days for that reason, so do not read the July 2026 jump in the paid base as behaviour. Subscriptions start 0 to 90 days after signup (median 33), and from August 2026 every start is exactly 14 days after signup. Found by `outputs/2026-09-28_engagement-growth-trial-launch/analysis.py`.
