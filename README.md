
# merra2ools <a href="https://optimal2050.github.io/merra2ools/"><img src="man/figures/logo.png" align="right" height="103" alt="merra2ools website" /></a>

<!-- badges: start -->

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
[![CRAN
status](https://www.r-pkg.org/badges/version/merra2ools)](https://CRAN.R-project.org/package=merra2ools)
[![Contributor
Covenant](https://img.shields.io/badge/Contributor%20Covenant-2.1-4baaaa.svg)](code_of_conduct.md)
<!-- badges: end -->

**merra2ools** is an R package and dataset for evaluating the potential
output of variable energy resources (VER) — specifically wind and solar
— globally, using over 40 years of hourly MERRA-2 reanalysis data. The
dataset was assembled, and the package initially developed, to support
the authors’ internal and collaborative energy modeling studies, and is
made available for public use. Further development will be guided by the
needs of related research projects and contributions from the broader
community. See
[{dev-status}](https://optimal2050.github.io/merra2ools/articles/roadmap.html)
for details.

## Purpose

The primary goal of merra2ools is to provide energy modelers and
analysts with:  
- A curated, long-term subset of MERRA-2 data relevant to energy
modeling.  
- A toolkit for estimating hourly potential output and capacity factors
of wind and solar energy.  
- Basic support for hydro power output potential using precipitation and
surface runoff indicators.

## Features

- Access to a global, multi-decade MERRA-2 dataset compressed for
  efficient use.  
- Scaled and rounded time series data stored as compressed integers to
  reduce storage while preserving relevant signal.
- Functions to estimate capacity factors and potential output across
  regions and time.

## Data & Compression

The `merra2ools` dataset includes a **41-year (1980–2020)** hourly time
series of selected indicators from the NASA MERRA-2 reanalysis dataset.
The data is preprocessed, scaled, and compressed to reduce file size
while preserving sufficient accuracy for renewable energy modeling. The
data is downloadable from
[Dryad](https://doi.org/10.5061/dryad.v41ns1rtt) cloud repository. A
sample data can be installed from the `merra2sample` package.

### Index columns

Each record is indexed by:

- **`UTC`** — Hourly timestamp in Coordinated Universal Time (UTC)  
- **`locid`** — Location ID (integer from 1 to 207,936, corresponding to
  MERRA-2 grid cells)

### Variables

| Variable | Description |
|----|----|
| `W10M` | Wind speed at 10 m, computed as `sqrt(U10M² + V10M²)`, rounded to 0.1 m/s |
| `W50M` | Wind speed at 50 m, computed similarly, rounded to 0.1 m/s |
| `WDIR` | Wind direction at 50 m, `atan2(V50M, U50M)`, rounded to nearest 10° |
| `T10M` | Air temperature at 10 m (°C), rounded to nearest integer |
| `SWGDN` | Surface shortwave radiation (W/m²), rounded to nearest integer |
| `ALBEDO` | Surface albedo \[0–1\], rounded to 0.01 |
| `PRECTOTCORR` | Total precipitation (kg/m²/h), bias-corrected, rounded to 0.1 |
| `RHOA` | Surface air density (kg/m³), rounded to 0.01 |

### Compression

- All values are stored as **scaled integers** to reduce size while
  maintaining numerical usability.
- Files are compressed using a high-efficiency format (e.g., `fst` with
  potential conversion to `parquet`, or custom binary) to keep the total
  dataset under 300 GB.
- Partial data loading is supported, allowing users to work with only
  the necessary time periods or variables.

## Installation

``` r
# install.packages("pak") # if not installed
pak::pkg_install("optimal2050/merra2ools") # install the package
pak::pkg_install("optimal2050/merra2sample") # optional - example dataset
```

Download datasets from <https://doi.org/10.5061/dryad.v41ns1rtt> to a
local folder. It is not required to download all the data, but it is
suggested to download at least for one full year (12 months).

``` r
library(merra2ools)
# link the package with the downloaded data
set_merra2_options(merra2.dir = "PATH TO THE DOWNLOADED DATA")
get_merra2_dir()  # check if the path is saved
check_merra2(detailed = T)
```

See [Get
started](https://optimal2050.github.io/merra2ools/articles/merra2ools.html)
article (or call `vignette("merra2ools", package = "merra2ools")`) for
details.

## Example data

The package reproduces basic algorithms of solar geometry, irradiance
decomposition, and the Plane-Of-Array models for different types of
solar PV trackers.  
See [Solar
Power](https://optimal2050.github.io/merra2ools/articles/solarpower.html)
article (or call`vignette("solarpower", package = "merra2ools")`) for
details.

<img src="man/figures/ghi_40y_avr_24steps.png" width="2550" />

The package includes basic algorithms for estimating wind power
potential and capacity factors. It uses the MERRA-2 wind speed at 10 and
50 meters above ground level (AGL) to extrapolate wind speed at higher
altitudes and estimates the wind power potential based on given wind
power curves.  
See [Wind
Power](https://optimal2050.github.io/merra2ools/articles/windpower.html)
article (or call `vignette("windpower", package = "merra2ools")`) for
details.

<img src="man/figures/wind_50m_40y_avr_24steps.png" width="3150" />

See the [MERRA-2
subset](https://optimal2050.github.io/merra2ools/articles/merra2.html)
article (or call `vignette("merra2", package = "merra2ools")`) for a
complete list of included time series and a detailed description of the
dataset.

## Data movies

Youtube channel [merra2ools](https://www.youtube.com/@merra2ools)
features visualization of 40 years of hourly (60 h/s) wind speed using
the `merra2ools` package.  
[![Watch the
demo](https://img.youtube.com/vi/d83ciEVmkos/hqdefault.jpg)](https://youtu.be/d83ciEVmkos?si=t3cxZswjv_-MX706)

> Click the image to watch the wind speed visualization demo on YouTube.

## Contributing

We welcome contributions to the merra2ools package! Please see our
[Contributing
Guidelines](https://optimal2050.github.io/merra2ools/CONTRIBUTING.html)
for details on how to get involved. Or simply open an issue, pull
request, or feature request on GitHub.

## License

This package is licensed under the [GNU Affero General Public License
v3.0](https://www.gnu.org/licenses/agpl-3.0.en.html). The data is
available under the [CC0 1.0 Universal (CC0 1.0) Public Domain
Dedication](https://creativecommons.org/publicdomain/zero/1.0/).

## Citing merra2ools

- Lugovoy, O., & Gao, S. (2023). *merra2ools: Satellite data and tools
  for retrospective assessment of renewable energy production* (R
  package version 0.1.05). Available at:
  <https://optimal2050.github.io/merra2ools/>

- The solar PV performance estimation module in `merra2ools` reproduces
  core elements of the modeling approach described by the [Solar PV
  Performance Modeling Collaborative
  (PVPMC)](https://pvpmc.sandia.gov/modeling-guide/1-weather-design-inputs/).

## Citing MERRA-2 Data

- Gelaro, R., et al. (2017). *The Modern-Era Retrospective Analysis for
  Research and Applications, Version 2 (MERRA-2)*. *Journal of Climate*,
  **30**(14), 5419–5454. <https://doi.org/10.1175/JCLI-D-16-0758.1>

- Bosilovich, M. G., Lucchesi, R., & Suarez, M. (2016). *MERRA-2: File
  Specification*. GMAO Office Note No. 9 (Version 1.1), 73 pp. Available
  at: <http://gmao.gsfc.nasa.gov/pubs/office_notes>

- NASA Global Modeling and Assimilation Office (GMAO). *MERRA-2: The
  Modern-Era Retrospective analysis for Research and Applications,
  Version 2*. Available at:
  <https://gmao.gsfc.nasa.gov/reanalysis/merra-2/>

- Global Modeling and Assimilation Office (GMAO) (2015), *MERRA-2
  tavg1_2d_rad_Nx: 2d, 1-Hourly, Time-Averaged, Single-Level,
  Assimilation, Radiation Diagnostics, 0.625 x 0.5 degree, V5.12.4
  (M2T1NXRAD)* at GES DISC. Accessed: 2019–2020. DOI:
  [10.5067/Q9QMY5PBNV1T](https://doi.org/10.5067/Q9QMY5PBNV1T)

- Global Modeling and Assimilation Office (GMAO) (2015), *MERRA-2
  tavg1_2d_slv_Nx: 2d, 1-Hourly, Time-Averaged, Single-Level,
  Assimilation, Single-Level Diagnostics, V5.12.4 (M2T1NXSLV)* at GES
  DISC. Accessed: 2019–2020. DOI:
  [10.5067/VJAFPLI1CSIV](https://doi.org/10.5067/VJAFPLI1CSIV)
