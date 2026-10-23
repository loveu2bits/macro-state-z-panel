# macro-state-z-panel

Daily cross-sectional panel of bounded macro-state z-scores, computed for 94 commodity
futures underlyings over 2011-01-04 → 2026-10-08.

## Contents

| File | Rows | Size |
|---|---|---|
| `z_panel.csv` | 221,243 (excl. header) | ~11 MB |

## Schema

| Column | Type | Description |
|---|---|---|
| `tradedate` | date (`YYYY/M/D`) | Trading day |
| `underlying` | string | Underlying code (94 distinct, lowercase) |
| `z1` | float | Macro factor score 1 |
| `z2` | float | Macro factor score 2 |
| `z3` | float | Macro factor score 3 |
| `z4` | float | Macro factor score 4 |
| `z5` | float | Macro factor score 5 |

## Notes

- Scores are **bounded**: each value is constructed to lie in `[-1, 1]` (observed range
  is narrower, roughly `[-0.57, 0.52]`). Rounding: 6 decimals.
- Missing values are left **empty** rather than zero-filled. About 80% of factor cells
  are populated; gaps occur during the warm-up window (insufficient history) and where a
  factor is undefined for a given day.
- Each `(tradedate, underlying)` pair appears at most once. Rows exist only while an
  underlying is listed, so history length differs by instrument (207–3,826 trading days
  each); the calendar spans 3,826 trading days.
- Encoding: UTF-8 with BOM (Excel-friendly), comma-separated, header row included.

## Example

```python
import pandas as pd

df = pd.read_csv("z_panel.csv", parse_dates=["tradedate"])
wide = df.pivot(index="tradedate", columns="underlying", values="z1")
```
