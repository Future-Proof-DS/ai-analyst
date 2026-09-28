---
name: describe-csv
description: Prints a short fixed-format description of any CSV (rows, columns, types, nulls, distinct counts, date ranges) so a person can tell in thirty seconds whether the file is what they think it is. Use when someone asks what is in a CSV, or hands you one to look at. Works in any folder; needs only Python with pandas.
user-invocable: true
allowed-tools: Read Bash
argument-hint: "<path to a CSV>"
---

You describe a CSV before anyone relies on it. Someone hands you a file and assumes it holds what its name says. You print its shape and, per column, the type, the null rate, the distinct count and the date range, in the same format every time, plus up to three things worth a second look. The person decides in thirty seconds whether the file is what they think it is. You need no helpers and no project setup: one pandas snippet does all the counting.

## What this is NOT

- Not a profile note. You write nothing to a knowledge folder or anywhere else.
- Not analysis. You answer no business question with the file.
- Not a cleaning step. You never modify, filter or rewrite the file.

## Act 1: Check the file (the gate)

If no path was given, or the file does not exist, say so and stop. Do not guess a near match.

## Act 2: Count with one snippet

Run this with Bash, replacing `<path>` with the file. Every number you report comes from its output.

```bash
python3 - "<path>" <<'PY'
import sys, pandas as pd
df = pd.read_csv(sys.argv[1])
dates = [c for c in df.columns if c == "date" or c.endswith(("_date", "_at"))]
for c in dates: df[c] = pd.to_datetime(df[c], errors="coerce")
print(f"{len(df)} rows, {df.shape[1]} columns (today {pd.Timestamp.today().date()})")
for c in df.columns:
    s = df[c]; rng = f"{s.min().date()} to {s.max().date()}" if c in dates and s.notna().any() else ""
    print(f"{c} | {s.dtype} | {s.isna().mean():.1%} | {s.nunique()} | {rng}")
PY
```

## Act 3: Render

Print exactly this markdown and nothing else:

```
## <file name>
<rows> rows, <columns> columns

| Name | Type | Nulls | Distinct | Range |
|------|------|-------|----------|-------|
| one row per column; Range only for date columns, blank otherwise |

### Worth a look
- at most three bullets
```

Choose the bullets only from these: a column over 20% null; an id column (name is `id` or ends in `_id`) with no nulls whose distinct count is below the row count, meaning duplicates; a date range that ends before or after today; a column with one distinct value. If none apply, the single bullet says "Nothing stands out."

## Principles

- **The snippet counts, you read.** No number comes from reading rows yourself; never print raw rows.
- **Same format every time.** A fixed layout is what makes the description readable in thirty seconds.
- **Point, do not explain.** Name what is worth a look; leave the why to whoever owns the data.
