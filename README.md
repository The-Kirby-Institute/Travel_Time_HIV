# Travel Time to HIV-related Facilities and Late HIV Diagnosis
 
This project contains the code to estimate the population-weighted average travel
time from each Primary Health Network (PHN) to its nearest HIV-related facility
in Australia, and to examine the association between that travel time and late
HIV diagnosis. The workflow builds a national motorised-travel friction surface,
computes least-cost travel time to the nearest facility, weights it by the 2024
population grid to summarise travel time at the PHN level, and fits logistic
regression models relating PHN travel-time category to late diagnosis.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21816969.svg)](https://doi.org/10.5281/zenodo.21816969)
 
## Aims
 
The aim is to quantify and map geographic variation in physical access to
HIV-related care across Australian PHNs, and to assess whether poorer access (a
longer population-weighted travel time to the nearest facility) is associated
with a higher likelihood of late HIV diagnosis. The code produces a per-PHN
travel-time estimate, a set of publication-ready maps, and univariate and
multivariable logistic regression results.
 
## Outputs
 
Running the analysis reproduces the following outputs:
 
- **Table** – frequency of late diagnosis by exposure, sex, birthplace, age
  group, travel-time category, and period.
- **Table** – univariate logistic regression (odds ratios and 95% CIs) for each
  predictor of late diagnosis.
- **Table** – multivariable logistic regression, including a travel-time × period
  interaction.
- **Figure** – facility locations over the national friction surface.
- **Figure** – per-cell travel time to the nearest facility, with facility points.
- **Figure** – PHN-level population-weighted mean travel time (Australia).
- **Figure** – conceptual diagram of the within-cell travel-time correction.
## Maintainers and developers
 
1. [Rongxing Weng](https://github.com/RongxingW); ORCiD ID: [0000-0003-1792-2186](https://orcid.org/0000-0003-1792-2186)
Affiliation: The Kirby Institute, UNSW Sydney, NSW, Australia
 
For any inquiries, please contact Rweng@kirby.unsw.edu.au or flag an issue.
The code will be updated as required to correct issues or to improve or add
features. Please check for updated versions periodically.
 
## Project structure
 
The analysis is contained in two RMarkdown scripts. `0_Setupmodel.Rmd` creates
the project folder skeleton, and `1_travel_time.Rmd` runs the full analysis top
to bottom. Project-specific inputs and outputs live under `projects/`.
 
```bash
├── projects/
│   └── late_hiv_diagnosis/     # specific project name
│       ├── data                # model input data (NOT tracked; see Data availability)
│       │   ├── phn.shp         # PHN boundary shapefile (+ .dbf/.shx/.prj)
│       │   ├── s100_*.xlsx      # S100 community HIV prescriber locations
│       │   ├── nhsd_general_practice_full_*.xlsx   # general practice locations
│       │   ├── nhsd_hospitals_full_*.xlsx          # hospital locations
│       │   ├── public_sexual_health_clinics_*.xlsx # sexual health clinic locations
│       │   ├── abs_popgrid_1km_2024.tif            # ABS 1-km population grid (2024)
│       │   └── _cache          # cached rasters / per-PHN layer (see below)
│       ├── output              # outputs: tables (.csv)
│       └── figures             # outputs: figures (.png)
├── 0_Setupmodel.Rmd            # creates the project folder skeleton
├── 1_travel_time.Rmd           # main analysis (run this)
├── LICENSE
└── README.md
```
 
Code, inputs, and outputs for the analysis are stored within the
`projects/late_hiv_diagnosis/` folder. The travel-time calculation is
computationally expensive, so its results are cached under `data/_cache/`
(friction and travel-time rasters, the points used, a metadata JSON, and a
per-PHN GeoPackage). The main script can then reload the cache and skip the slow
step.
 
## Using the code
 
To use the code, clone or download this repository to a convenient location on
your computer. You will need the following software and associated packages:
 
1. R (>= 4.1, for the native `|>` pipe). (Optional) RStudio, a user interface for R.
2. R packages used by the analysis:
   `tidyverse`, `sf`, `terra`, `geodata`, `traveltime`, `readxl`, `tidyterra`,
   `gdistance`, `broom`, `exactextractr`, `ggnewscale`
   Most install from CRAN. `traveltime` installs from the idem-lab R-universe:
```r
   install.packages(c("tidyverse","sf","terra","geodata","readxl",
                      "tidyterra","gdistance","broom","exactextractr","ggnewscale"))
   install.packages("traveltime", repos = c("https://idem-lab.r-universe.dev"))
```
 
### Steps to run
 
1. Run `0_Setupmodel.Rmd` to create
   `projects/late_hiv_diagnosis/{data, data/_cache, output, figures}`.
2. Place the required inputs (see **Project structure** and **Data
   availability**) in the project's `data/` folder. Set `HIVDataFolder` in the
   `Initialise` chunk of `1_travel_time.Rmd` to your own local copy of the HIV
   notification data.
3. Open `1_travel_time.Rmd`, check the file paths in the `Initialise` and
   `Input` chunks, then run the chunks top to bottom.
The main script offers two paths for the travel-time step. If you have the
cached files, run the **Load runned travel time files** chunk to reload them and
skip the slow calculation. Otherwise, run the **Travel time calculation** chunk,
which builds the friction surface (requires internet on first run) and writes the
cache for subsequent runs.
 
> **Note on facility sets.** The analysis can be run for different definitions of
> "HIV-related facility" (`s100`, `s100_shc`, `all`, `gp`). The set is chosen in
> the calculation chunk. 
 
## Data availability
 
The HIV notification data used by this analysis are **not** publicly available
and are **not included** in this repository, due to privacy and ethics
restrictions.
 
The remaining inputs are publicly available and should be obtained from their
original sources (facility locations, the ABS 1-km population grid, and the PHN
boundary shapefile). The Australia country outline is downloaded automatically at
run time via the `geodata` package (GADM).
 
## Publication
 
The following publication is associated with this project and used the code in
this repository to generate the results and figures.
 
(...)
 
## License
 
Code in this repository is released under the MIT License (see `LICENSE`).

## References

This analysis relies on the following software and data. Please cite them if you
use or adapt this code.

**Software**
- traveltime R package — Ryan GE, Tierney N, Golding N and Weiss DJ. traveltime: an R package to calculate travel time across a landscape from user-specified locations. Gates Open Res 2025, 9:50. (https://doi.org/10.12688/gatesopenres.16356.1)
- Malaria Atlas Project motorised friction surface (motor2020) — Weiss DJ, Nelson A, Vargas-Ruiz CA, et al. Global maps of travel time to healthcare facilities. Nat Med. 2020;26(12):1835-1838. (https://doi.org/10.1038/s41591-020-1059-1)

**Data sources**
- [ABS 1-km population grid](https://digital.atlas.gov.au/maps/digitalatlas::abs-australian-population-grid-2024/about), 2024 — [access date: 10 October 2025]
- Facility locations ([S100 prescribers](https://ashm.org.au/prescriber-programs/find-a-prescriber/find-a-hiv-prep-prescriber/); [NHSD general practices and hospitals](https://ecat.ga.gov.au/geonetwork/srv/eng/catalog.search#/metadata/149331);
  [public sexual health clinics](https://www.racp.edu.au/docs/default-source/fellows/resources/achsmh/register-of-public-sexual-health-clinics.pdf?sfvrsn=e64a2d1a_18)) — [access date: 10 October 2025]

 
## Disclaimer
 
The code has been made publicly available for transparency and replication
purposes and in the hope that it will be useful. We take no responsibility for
results generated with the code or their interpretation, but are happy to assist
with its use and application.
