# Replicating the General Spatio-Temporal Factor Model (GSTFM)

Seminar project in time series econometrics, TU Dortmund University, Department of Statistics (2025).

The project reproduces the core diagnostics and the estimation algorithm of the General Spatio-Temporal Factor
Model proposed by Barigozzi, La Vecchia and Liu (arXiv:2312.02591), and applies the estimator to gridded
climate data.

## The idea in short

Lattice data — anything observed on a spatial grid over time — carry two kinds of dependence at once: across
locations and across time. The GSTFM identifies the common component through the behaviour of the spectral
density matrix: as the cross-sectional dimension grows, a fixed number of its eigenvalues diverge while the
rest stay bounded. That eigengap is what separates common factors from idiosyncratic noise.

Stacking the same data into a standard dynamic factor model does not give that separation, which is the point
of the second diagnostic below.

## What the code does

| Step | Content |
|---|---|
| Figure 1 | Simulates a spatio-temporal factor model and traces how the eigenvalues separate as the cross-section grows |
| Figure 2 | Shows that a standard dynamic factor model on the same data produces no clear eigengap |
| Algorithm 1 | Estimates the common component: frequency-domain spectral projectors are turned into truncated spatio-temporal filters by an inverse Fourier approximation |
| Application | Runs the estimator on WeatherBench2 climate data |

Implementation notes: spectral density estimation with kernel smoothing (Bartlett, Epanechnikov), 3D FFT
helpers and periodic indexing written from scratch in base R; `RhpcBLASctl` for numerical stability;
`reticulate` to read the zarr/xarray climate dataset.

## Repository contents

| File | What it is |
|---|---|
| `code.Rmd` | The replication itself — simulations, diagnostics, Algorithm 1, application |
| `Report.Rmd`, `Report.pdf` | Write-up of the results |
| `Seminar-paper.tex`, `paper.pdf` | Seminar paper |
| `references.bib` | Bibliography |
| `*.rds` | Cached simulation results, so the figures can be rebuilt without re-running everything |
| `*.pdf`, `*.png` | Generated figures (eigenvalue curves, spatial snapshots, diagnostics) |

## Reproducing the results

```r
install.packages(c("rmarkdown", "RhpcBLASctl", "reticulate"))
rmarkdown::render("code.Rmd")
```

The simulations are the slow part; cached results in the `.rds` files let you regenerate the figures directly.
The climate application additionally needs Python with `xarray` and `zarr`, and downloads WeatherBench2 data
from Google Cloud Storage.

## Reference

Barigozzi, M., La Vecchia, D., Liu, H. *General Spatio-Temporal Factor Models for High-Dimensional Random
Fields on a Lattice.* arXiv:2312.02591 — https://arxiv.org/abs/2312.02591

The paper is not redistributed here; the link above points to the original.

## Licence

Code: MIT (see `LICENSE`). The seminar paper and report are my own work; please cite rather than reuse.
