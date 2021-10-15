# 2021_4DVARNET_CHALLENGE_NATL60

This repository contains codes and sample notebooks for downloading and processing the 4dvarnet data challenge.

## Motivation
The goal is to investigate how to best reconstruct sequences of Sea Surface Height (SSH) maps from partial satellite altimetry observations. This data challenge follows an _Observation System Simulation Experiment_ framework: "Real" full SSH are from a numerical simulation with a realistic, high-resolution ocean circulation model: the reference simulation. Satellite observations are simulated by sampling the reference simulation based on realistic orbits of past, existing or future altimetry satellites. A baseline reconstruction method is provided (see below) and the practical goal of the challenge is to beat this baseline according to scores also described below and in Jupyter notebooks.

### Reference simulation
The reference simulation is the NATL60 simulation based on the NEMO model (Ajayi et al. 2020 doi:[10.1029/2019JC015827](https://doi.org/10.1029/2019JC015827)). The simulation is run without tidal forcing. 

### Observations
The SSH observations include simulations of Topex-Poseidon, Jason 1, Geosat Follow-On, Envisat, and SWOT altimeter data. This nadir altimeters constellation was operating during the 2003-2005 period and is still considered as a historical optimal constellation in terms of spatio-temporal coverage. The data challenge simulates the addition of SWOT to this reference constellation. No observation error is considered in this challenge.

### Data sequence and use
The SSH reconstructions are assessed over the period from 2012-10-22 to 2012-12-02: 42 days, which is equivalent to two SWOT cycles in the SWOT science phase orbit.
For reconstruction methods that need a spin-up, the **observations** can be used from 2012-10-01 until the beginning of the evaluation period (21 days). This spin-up period is not included in the evaluation. For reconstruction methods that need learning from full fields, the **reference data** can be used from 2013-01-02 to 2013-09-30. The reference data between 2012-12-02 and 2013-01-02 should never be used so that any learning period or other method-related-training period can be considered uncorrelated to the evaluation period.

## Results to provide

1. Train model on GF zone and test on GF zone.
1. Train model on GF2 (lat : [42,52] and lon : [-45,35], patch 200\*200) located along GF (but different of GF test zone) over one year and test on GF zone during BOOST_SWOT period.
1. fine-tuning of NATL model on GF zone.

## Leaderboard
| Method     |   µ(RMSE) |   σ(RMSE) |   λx (degree) |   λt (days) | Notes                     | Reference        |
|:-----------|------------------------:|---------------------:|-------------------------:|-----------------------:|:--------------------------|:-----------------|
|GF/GF | **0.95**  | 0.014  | **0.87** 	 | 6.91 | xxx | eval_4dvarnet|
|GF2/GF | 0.94  | **0.013**  | 0.95 	 | 7.87 | xxx | eval_4dvarnet|
|Fine Tuning/GF :trophy: | **0.95**  | **0.013**  | 0.92 	 | **6.47** | xxx | eval_4dvarnet|


**µ(RMSE)**: average RMSE score.  
**σ(RMSE)**: standard deviation of the RMSE score.  
**λx**: minimum spatial scale resolved.  
**λt**: minimum time scale resolved. 

## Dowload the data
1- Ground truth data: wget --directory-prefix=../data https://store3.gofile.io/download/7c2b3332-f46c-4901-9a09-b141d62cd4e4/2021a_SSH_mapping_NATL60_4DVarNet_nadirswot_gt.nc
2- Experiences data: wget --directory-prefix=../data https://store3.gofile.io/download/65078c91-55b8-4963-b89e-dac8e7cfc02f/2021a_SSH_mapping_NATL60_4DVarNet_nadirswot_FineTuning.nc
wget --directory-prefix=../data https://store3.gofile.io/download/e1f81597-4430-4077-8dbf-a0fab20b9029/2021a_SSH_mapping_NATL60_4DVarNet_nadirswot_GF2_GF.nc


## Baseline and evaluation

### Baseline
The baseline mapping method is optimal interpolation (OI), in the spirit of the present-day standard for DUACS products provided by AVISO. OI is implemented in the [`baseline_oi`](https://github.com/ocean-data-challenges/2020a_SSH_mapping_NATL60/blob/master/notebooks/baseline_oi.ipynb) Jupyter notebook. The SSH reconstructions are saved as a NetCDF file in the `results` directory. The content of this directory is git-ignored.

### Evaluation
The evaluation of the mapping methods is based on the comparison of the SSH reconstructions with the *reference* dataset. It includes two scores, one based on the Root-Mean-Square Error (RMSE), the other based on Fourier wavenumber spectra. The evaluation notebook [`example_data_eval`](https://github.com/CIA-Oceanix/2021_4DVARNET_CHALLENGE_NATL60/blob/main/notebooks/eval_4dvarnet.ipynb) implements the computation of these two scores as they could appear in the leaderboard. The notebook also provides additional, graphical diagnostics based on RMSE and spectra.

## Data processing

