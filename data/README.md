# Data

This directory is for your per-cohort EWAS result files.

## Expected format

One file per cohort (CSV or TSV), each containing at minimum:

| Column  | Description                          |
|---------|--------------------------------------|
| `CpG`   | CpG identifier (e.g. `cg00000029`)   |
| `beta`  | effect estimate (regression coefficient) |
| `se`    | standard error of the estimate       |
| `p`     | p-value from the cohort-level EWAS (optional) |

The column-name alias mapper in the tutorial handles common variants
(`estimate`, `StdErr`, `SE`, `P.Value`, `logFC`, etc.).

## Demo data

The tutorial generates simulated data automatically (Section 3) and downloads
the GEMAS asthma EWAS from Zenodo at runtime (Section 13) — no manual data
download is needed to run the tutorial.

## Your real data

Place your 14 cohort files here, then update the parameters in Section 2 of the
tutorial:

```r
input_dir    <- "data"
file_pattern <- "cohort_.*\\.csv$"
```
