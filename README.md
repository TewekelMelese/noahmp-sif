# noahmp-sif
SIF enhanced Ball Berry scheme
# Noah-MP SIF-Enhanced Model

This repository contains the modified source code for the Noah-MP land surface model (version 5.1.1) implementing a Solar-Induced Fluorescence (SIF) constraint in the Ball-Berry stomatal conductance scheme, as described in:

> Gemechu, T.M., et al. "Enhancing Water Use Efficiency and Drought Resilience in a Semi-Arid Basin: Integrating Solar-Induced Fluorescence into the Noah-MP Land Surface Model." *International Journal of Applied Earth Observation and Geoinformation* (under review).

## Files

- `module_sf_noahmplsm.F` – Modified Noah-MP source code with SIF-enhanced Ball-Berry scheme
- `SIF_Ball_Beryy_Analysis.ipynb` – Jupyter notebook for post-processing and analysis
- `SIF_ANALYIS.ipynb` – Additional analysis notebook

## Modifications

The key modification scales the net photosynthesis rate (PSN) by a SIF-derived stress factor:
PSN = MIN(WJ,WC,WE) * IGS * (1.0 + SIF_FACTOR * SIF)


Where `SIF` is a normalized stress factor (0–1) derived from GOSIF v2 data.

## Requirements

- Noah-MP v5.1.1 source code base
- Fortran compiler (gfortran or ifort)
- Python 3.x with numpy, pandas, matplotlib for analysis notebooks

## Usage

1. Replace the original `module_sf_noahmplsm.F` with the modified version
2. Compile Noah-MP following standard procedures
3. Provide SIF stress factor as model input

## Data

The SIF stress factor used in this study was derived from GOSIF v2 data (Li & Xiao, 2019). Pre-processed input files for the Awash Basin are available upon request.



## Contact

Tewekel Melese Gemechu – [email]
