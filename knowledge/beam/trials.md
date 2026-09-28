# trials

Grain (a guess from the data, confirm it): one row per trial_id. 2,073 rows.

## Columns

| Name | Type | Null rate | Distinct |
| --- | --- | --- | --- |
| trial_id | int64 | 0.00% | 2,073 |
| user_id | int64 | 0.00% | 2,073 |
| started_at | datetime64[ns] | 0.00% | 2,071 |
| ends_at | datetime64[ns] | 0.00% | 2,071 |
| outcome | object | 20.16% | 2 |
| outcome_at | datetime64[ns] | 20.16% | 1,655 |

## Date ranges

- `started_at`: 2026-08-01T00:29:17 to 2026-09-24T22:59:05
- `ends_at`: 2026-08-15T00:29:17 to 2026-10-08T22:59:05
- `outcome_at`: 2026-08-06T05:37:23 to 2026-09-24T23:55:08

## Low-cardinality values

- `outcome`: converted (828), canceled (827)

## Meaning

Grain in plain words (inferred): one row per trial; each user has at most one trial (`user_id` is as distinct as `trial_id`). Every trial user exists in `users`.

- `trial_id`: the trial's key (inferred).
- `user_id`: the user who took the trial; joins one to one to `users.user_id`, and to `subscriptions.user_id` for every resolved trial (converted trials hold a monthly or annual subscription, canceled trials a free one; open trials have no subscription row yet), checked 2026-09-28.
- `started_at`: when the trial began; 2026-08-01 (launch) to 2026-09-24. Compare on dates (quirk "Timestamps everywhere").
- `ends_at`: `started_at` plus exactly 14 days for every row (quirk "Trials mature late").
- `outcome`: converted or canceled; empty for 418 trials (20.16%), all of them with `ends_at` after 2026-09-24, so they are still open, not failures (quirk "Trials mature late").
- `outcome_at`: when the outcome happened; null exactly when `outcome` is null. Converted trials resolve exactly at `ends_at` (day 14, every row), so conversion looks automatic at trial end (inferred: an opt-out trial). Canceled trials resolve between day 4 and day 13 (median day 8).

Data cutoff (inferred): the latest `started_at` and `outcome_at` are both on 2026-09-24, so the export runs to the end of 2026-09-24.

**Maturity bias (not yet in quirks.md; proposed 2026-09-28).** Because cancellations resolve during the trial and conversions only at day 14, a trial that started less than 14 days before the cutoff can already show `canceled` but can never show `converted`. Conversion rates over immature cohorts are biased down, not just incomplete. Only trials whose `ends_at` is on or before the cutoff (started on or before 2026-09-10) have a final outcome.

