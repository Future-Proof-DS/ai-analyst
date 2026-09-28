# payments

Grain (a guess from the data, confirm it): one row per payment_id. 100,914 rows.

## Columns

| Name | Type | Null rate | Distinct |
| --- | --- | --- | --- |
| payment_id | int64 | 0.00% | 100,914 |
| subscription_id | int64 | 0.00% | 11,122 |
| payment_date | datetime64[ns] | 0.00% | 13,232 |
| amount_usd | float64 | 0.00% | 3 |
| method | object | 0.00% | 4 |

## Date ranges

- `payment_date`: 2025-01-01T00:00:00 to 2026-09-24T23:51:30

## Low-cardinality values

- `amount_usd`: 10.0 (59,064), 12.0 (37,384), 100.0 (4,466)
- `method`: card (50,237), paypal (20,272), apple_pay (15,298), google_pay (15,107)

## Meaning

Grain in plain words (inferred): one row per billing charge on a paid subscription.

- `payment_id`: the charge's key (inferred).
- `subscription_id`: the subscription charged; every payment matches a monthly or annual subscription, none a free one (checked 2026-09-28). 11,122 distinct subscriptions.
- `payment_date`: when the charge happened. Covers 2025-01-01 to 2026-09-24 only, so subscriptions that started before 2025 have no payment history before that date. No payment falls before its subscription's start or after its end.
- `amount_usd`: 10 and 12 are monthly charges, 100 is annual (checked against plan). Every monthly charge before 2026-03-01 is $10 and every one from 2026-03-01 on is $12: the new price applied to existing subscribers at once, not only to new ones (quirk "Monthly price change"). Annual stays at $100 throughout.
- `method`: card, paypal, apple_pay or google_pay (inferred: how the user paid).

Monthly subscriptions are billed about every 30 days, so some months carry two charges for the same subscription (always months with 31 days). Count paying subscriptions per month with distinct `subscription_id`, not rows.

