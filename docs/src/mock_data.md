# Mock Data

In order to ease testing and development, we generate mock data that closely ressembles the original data while preserving the privacy of individuals in the cohorts. This page explains how this is done.

First but foremost, for all individuals, a mock sample ID is created. Since there is no sensitive information about individuals, these cannot be identified as long as the genetic data is perturbed. We explain below how this is done.

## Genotyping Arrays

1. Only a subset of genetic variations are kept (e.g. 100 out of 500 000)
2. For each individual, each variant is resampled independently from the cohort's empirical distribution. The probability of this operation to have no effect on an individual is difficult to estimate since it depends on each variant's alleles frequencies. If all variants were different it would be (1/n_samples)^nvariants which for the lower values of $nsamples=1000$ and $nvariants=110$, this is lower than $10^-300$.

## Whole Genome Sequencing

The GVCF mock data arising from whole genome sequencing is built from a very small intersection of variants (e.g., 100) common to all genotyping arrays. Individuals are thus unidentifiable.

## Covariates

Since the covariates are not sensitive, the newly created odap identifier is simply forwarded to covariates.

## How to Mock 

To run, on ODAP, assuming:

- The data output by Dominique is in `/odp-beefgs/a015/linked_data/preqc/array-pre-imputation/` and mounted in the singularity container in `/mnt/data`
- The repo is mounted in `/mnt/genomicc-workflows` (This is not necessary anymore once the code is in the container, just need to point to `/opt/genomicc-workflows`)

```bash
singularity shell --bind /odp-beefgs/a015/linked_data/preqc/array-pre-imputation/:/mnt/data PATH_TO_SINGULARITY_IMAGE
```

Then run 
```bash
JULIA_DEPOT_PATH=$JULIA_DEPOT_PATH:/root/.julia julia --project=/opt/genomicc-workflows /opt/genomicc-workflows/bin/genomicc.jl
```

I also manually:

- Added duplicate sample IDs to reproduce what is in the data.
- Changed the position of `GSA-rs114361133` in `test/assets/genomicc/genotyping_arrays/mock.release_2021_2023.map` to be unliftable.

## Thousands Genomes

The data was downloaded from the 1000GP [FTP](https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/) and pruned using the `bin/make_thousand_genomes_filter_files.jl` script.