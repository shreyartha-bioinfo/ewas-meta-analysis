# Fast EWAS Meta-Analysis with `metagen`

Inverse-variance-weighted epigenome-wide association meta-analysis across
multiple cohorts, using a **vectorized matrix implementation** that turns an
hours-long per-CpG `metagen()` loop into a seconds-long scan — with `metagen()`
retained for validation and forest plots on top hits.

## What this does

- **Vectorized fixed-effect (IVW) and DerSimonian–Laird random-effects** meta-analysis across all CpGs simultaneously
- **~9,000× speedup** over the naive per-CpG `metagen()` loop (seconds vs. hours for 900k CpGs)
- **Validated** against `metagen()` (floating-point exact) and against a published meta-analysis (GEMAS asthma EWAS)
- **Real data demo**: GEMAS asthma EWAS (2 cohorts, EPIC 850K, 751k CpGs) with validation against the authors' published results
- **Simulated 14-cohort demo** with injected true hits for the performance benchmark
- **Plots**: volcano, Manhattan, forest plots, QQ plot (genomic inflation λ), funnel plot, L'Abé plot, DMR plot
- **Heterogeneity diagnostics**: I² and tau² distributions, Cochran's Q vs pooled significance, fixed- vs random-effects divergence, and a per-hit heterogeneity table
- **EWAS Catalog cross-referencing**: queries the MRC-IEU EWAS Catalog API to check whether top hits are novel or previously reported
- **DMR aggregation**: lightweight Stouffer p-value combination to identify differentially methylated regions

## Quickstart

```bash
git clone https://github.com/<your-username>/ewas-meta-analysis.git
cd ewas-meta-analysis
```

Open `ewas_metaanalysis_tutorial.Rmd` in RStudio and click **Knit**, or run:

```r
install.packages(c("meta", "data.table", "ggplot2", "patchwork", "httr", "jsonlite", "rmarkdown"))
rmarkdown::render("ewas_metaanalysis_tutorial.Rmd")
```

The tutorial runs end-to-end with no external data — it generates simulated
data and downloads the GEMAS EWAS from Zenodo automatically.

## Prerequisites

| Package | Purpose |
|---------|---------|
| **meta** (≥ 7.0) | `metagen()` for validation and forest plots |
| **data.table** | Fast file I/O and data manipulation |
| **ggplot2** | All plots |
| **patchwork** | Side-by-side heterogeneity diagnostic plots |
| **httr** + **jsonlite** | EWAS Catalog API queries |
| **rmarkdown** | Rendering the tutorial |

```r
install.packages(c("meta", "data.table", "ggplot2", "patchwork", "httr", "jsonlite", "rmarkdown"))
```

## Using your own data

1. Place your per-cohort EWAS result files in `data/` (one file per cohort, CSV or TSV)
2. Each file needs columns: `CpG`, `beta`, `se` (p-value optional). Common column-name aliases are auto-detected.
3. Edit the parameters in Section 2 of the Rmd:

```r
n_cohorts    <- 14
min_cohorts  <- 2
input_dir    <- "data"
file_pattern <- "cohort_.*\\.csv$"
```

4. Delete or skip the simulated demo chunk (Section 3) and the GEMAS demo (Section 13)
5. Knit

## Repository structure

```
ewas-meta-analysis/
├── ewas_metaanalysis_tutorial.Rmd    # The tutorial (copy-paste ready)
├── README.md
├── LICENSE                           # MIT
├── .gitignore
├── .github/
│   └── workflows/
│       └── render-tutorial.yml       # Auto-renders Rmd on push, deploys to GH Pages
├── data/
│   └── README.md                     # Where to put your cohort files
└── results/
    ├── README.md
    └── ewas_metaanalysis_tutorial.html  # Pre-rendered output
```

## Key results from the demo

| Metric | Value |
|--------|-------|
| CpGs meta-analyzed (GEMAS) | 751,128 |
| Vectorized runtime | ~1.8 seconds |
| Speedup vs `metagen` loop | ~9,000× |
| Validation vs `metagen` | Max diff ~1e-16 (floating-point exact) |
| Validation vs published GEMAS | Max diff ~5e-7 (rounding in published file) |
| Genomic inflation λ | 1.04 |
| EWAS Catalog: previously reported hits | 43 / 50 |
| EWAS Catalog: potentially novel hits | 7 / 50 |
| DMRs identified | 10 (all FDR < 0.05) |

## Data sources

- **GEMAS asthma EWAS**: Martin-Gonzalez et al. (2025), Zenodo record [11262085](https://doi.org/10.5281/zenodo.11262085). Two EPIC 850K cohort subsets with per-cohort summary statistics and published meta-analysis results.
- **EWAS Catalog**: MRC-IEU, University of Bristol. [ewascatalog.org](https://www.ewascatalog.org/)

## Citation

If you use this tutorial or the vectorized approach in your work, please cite:

> Martin-Gonzalez E, Perez-Garcia J, Herrera-Luis E, et al. Epigenome-Wide Association Study of Asthma Exacerbations in Europeans. Zenodo. https://doi.org/10.5281/zenodo.11262085

And the `meta` package:

> Balduzzi S, Rücker G, Schwarzer G. How to perform a meta-analysis with R: a practical tutorial. Evidence-Based Mental Health. 2019;22(4):153-160.

## License

MIT — see [LICENSE](LICENSE).
