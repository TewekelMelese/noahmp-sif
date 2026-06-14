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
For each grid cell, we compute a dimensionless stress index from the observed SIF:
SIFnorm=SIFobs/(SIF95 ) 					(1)   
where SIF₉₅ is the 95th percentile of the long-term monthly SIF time series for that pixel. 

In the standard Noah MP, the net photosynthesis rate (PSN) is calculated as:
PSNbase=min(WJ,WC,WE)*IGS   		(2)   
where WJ is the light-limited (RuBP regeneration-limited) photosynthesis rate (μmol CO₂ m⁻² s⁻¹), determined by absorbed photosynthetically active radiation (APAR) and the maximum electron transport rate (Jₘₐₓ); WC is the Rubisco-limited photosynthesis rate (μmol CO₂ m⁻² s⁻¹), determined by intercellular CO₂ concentration (Cᵢ) and the maximum carboxylation rate (Vcₘₐₓ); WE is the export-limited (triose phosphate utilization-limited) photosynthesis rate (μmol CO₂ m⁻² s⁻¹), representing the capacity of the Calvin cycle to export carbohydrates; and IGS is the growing season index (dimensionless, 0–1), diagnosed from accumulated growing degree days and phenology thresholds (Niu et al., 2011). The minimum of these three rates determines which process is most limiting at any given time step, following Farquhar (Farquhar et al., 1980) as implemented in Noah-MP.
We introduce a down regulation factor based on SIFnorm:
PSNmod=PSNbase*(1−α*(1−SIFnorm)) 	(3)
In the Fortran code, the normalised SIF value is passed as the variable SIF. The Fortran variable SIF is dimensionless. Here, α is an empirical parameter (0 ≤ α ≤ 1) that sets the maximum fractional reduction in photosynthesis when SIFnorm = 0 (complete stress). When SIFnorm=1, PSNmod=PSNbase. The term (1−SIFnorm) represents the stress level. In the Fortran code, this is implemented as PSN = PSN_base * (1.0 - alpha * (1.0 - SIFnorm)). In our implementation, α is defined as a module‑level parameter (SIF_FACTOR = 0.10), but it can be adjusted via the namelist for sensitivity tests




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
