# Replication of "Urban Public Works in Spatial Equilibrium: Experimental Evidence from Ethiopia"

This repository contains an empirical replication and analysis of:

> **Franklin, S., Imbert, C., Abebe, G., & Mejia-Mantilla, C. (2024).** *Urban Public Works in Spatial Equilibrium: Experimental Evidence from Ethiopia.* American Economic Review, 114(5), 1382–1414.

## Overview

The original paper evaluates the Urban Productive Safety Net Program (UPSNP) in Addis Ababa, Ethiopia, leveraging a randomized rollout across 90 eligible local administrative units (*woredas*) alongside georeferenced commuting flows. By combining reduced-form shift-share identification following Borusyak and Hull (2023) with a structural spatial equilibrium model, the paper quantifies both direct program effects on beneficiaries and spatial general equilibrium spillovers (private-sector wages, commuting responses, and neighborhood amenities).

This replication covers the core empirical and structural results of the paper:

1. **Spatial Commuting Flows (Paper Figure 2 / Writeup Figure 1):** Baseline out-commuting and in-commuting rates across Addis Ababa woredas by treatment assignment, replicated in both R (`ggplot2` / `sf`) and Python (`geopandas` / `matplotlib`).
2. **Direct Effects on Employment and Amenities (Paper Table 2 / Writeup Table 1):** Estimation of public employment generation, private employment crowd-out, and neighborhood amenity improvements across the whole sample, eligible households, and ineligible households using `fixest::feols()`.
3. **Private Sector Wage Spillovers (Paper Table 3 / Writeup Table 2):** Naive origin-based vs. recentered destination exposure effects on private-sector log hourly wages, with p-values computed via Randomization Inference across 2,000 simulated treatment assignments.
4. **Commuting Gravity & Wage Elasticity (Paper Table 5 / Writeup Table 3):** PPML (`glm` Poisson) and control-function Poisson-IV estimates of the Fréchet commuting elasticity parameter ($\theta \approx 2.08$) using recentered destination exposure as an instrument.
5. **Spatial Equilibrium Welfare Decomposition (Paper Table 6 / Writeup Table 4):** Structural decomposition of welfare gains for the urban poor into direct effects, private wage spillovers, and amenity improvements under partial and complete rollouts compared against an equivalent cash transfer benchmark (incorporating LASSO-selected controls via `glmnet`).

## Repository Structure

```text
.
├── Replication-Log.Rmd      # RMarkdown source containing R & Python replication code
├── Replication-Log.pdf      # Rendered RMarkdown appendix log with code and outputs
├── writeup/
│   ├── main.tex             # Main replication report (XeLaTeX source)
│   ├── main.pdf             # Compiled replication report PDF
│   ├── references.bib       # BibTeX bibliography
│   ├── images/              # Figures included in the writeup
│   └── tables/              # Exported table assets
├── .gitignore               # Excludes raw source data and LaTeX auxiliary files
└── README.md                # Project documentation
```

## Data Setup

Raw replication data files are excluded from version control via `.gitignore`. To re-run [`Replication-Log.Rmd`](Replication-Log.Rmd) from scratch, obtain the official replication package from the *American Economic Review* / OpenICPSR repository and place the data files in a `data/` directory at the root of this repository:

```text
data/
├── pi_i_j.dta
├── data_ind.dta
├── B_i_hh.dta
├── data_woreda.dta
├── RI_treatment.dta
├── RI_exposure.dta
└── shapefiles/
    └── woreda_randomization_clean.shp (along with .shx, .dbf, .prj)
```

## Reproducing the Outputs

### 1. Replication Code Log (`Replication-Log.Rmd`)

Requires **R** (with packages `tidyverse`, `haven`, `sf`, `RColorBrewer`, `fixest`, `panelsummary`, `modelsummary`, `kableExtra`, `sandwich`, `glmnet`, `reticulate`, `here`) and **Python** (with `pandas`, `geopandas`, `matplotlib`):

```r
rmarkdown::render("Replication-Log.Rmd")
```
