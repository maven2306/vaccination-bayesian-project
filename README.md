# Vaccination Coverage Analysis using Bayesian Inference

**Author:** Alejandrina Jimenez Guzman, Brian William Hingpit, Joanna Lewandowska Przysiwek, Matteo Venturini
**Date:** May 1, 2025

## Project Overview

This project provides a comprehensive analysis of vaccination coverage data using Bayesian inference techniques. The dataset includes the number of vaccinated children and the total sample size for birth years 2011 through 2020, stratified by three US states: Georgia, Wisconsin, and Mississippi.

The analysis aims to:
1.  Derive and compare posterior distributions of vaccination coverage using both non-informative and informative priors.
2.  Model trends in vaccination coverage over time using logistic regression and MCMC methods with OpenBUGS.
3.  Investigate whether vaccination trends differ by location.
4.  Generate predictions and make pairwise comparisons between locations.

## Methodology

The analysis is conducted in two main parts:

### 1. Analytical Approach (Beta-Binomial Model)

The posterior distribution of vaccination coverage (`θ`) is derived analytically using a Beta-Binomial conjugate model. The analysis is performed under two different prior assumptions:
*   **Non-Informative Prior:** A `Beta(1, 1)` distribution (equivalent to a Uniform distribution) is used to reflect no prior knowledge. The resulting posterior is `Beta(y + 1, n - y + 1)`.
*   **Informative Prior:** A `Beta(9, 1)` distribution is used to reflect a prior belief that vaccination coverage is typically 90% or higher. The resulting posterior is `Beta(y + 9, n - y + 1)`.

Posterior summary measures (mean, mode, median, variance, and standard deviation) are calculated for each birth year, each region, and for each year-region combination.

### 2. Computational Approach (MCMC with BUGS)

Two logistic regression models are implemented in BUGS to analyze trends from 2011-2019:

*   **Model 1: Overall Trend**
    *   Models the number of vaccinated children as a binomial outcome.
    *   The logit of the vaccination coverage (`π`) is modeled as a linear function of the birth year:
      `logit(π_ij) = β₀ + β₁ * BirthYear_j`
    *   Non-informative normal priors (`dnorm(0, 0.001)`) are used for `β₀` and `β₁`.

*   **Model 2: Location-Specific Trends**
    *   Extends the first model by including location-specific intercepts and slopes:
      `logit(π_ij) = β₀ᵢ + β₁ᵢ * BirthYear_j`
    *   This allows for investigating whether the vaccination coverage trends are distinct across Georgia, Wisconsin, and Mississippi.
    *   Non-informative normal priors are used for all parameters.

Convergence of the MCMC chains was assessed using traceplots and autocorrelation plots, which indicated that the models converged successfully.

## Key Findings

1.  **Impact of Priors:** The choice of a non-informative versus an informative prior had a minimal impact on the posterior summary measures. This suggests that the data provides substantial information, outweighing the influence of the prior.

2.  **Vaccination Coverage Trends (2011-2019):**
    *   The overall model suggests a slight decrease in vaccination coverage in more recent years.
    *   The location-specific model reveals distinct trends: Georgia and Wisconsin show a slight decrease in coverage over the years, while Mississippi shows a slight increase.

3.  **Prediction for 2020:** The observed number of vaccinated children in 2020 (482 out of 546) falls within the 95% prediction interval of **[476, 506]** generated from the 2011-2019 model. This indicates the 2020 figures are in line with expectations based on previous years' data.

4.  **Pairwise Comparisons (2019):**
    *   **Georgia vs. Wisconsin:** The vaccination coverage in Georgia is estimated to be between 2% less and 5% more than in Wisconsin.
    *   **Georgia vs. Mississippi:** The coverage in Georgia is estimated to be between 4% less and 2% more than in Mississippi.
    *   **Mississippi vs. Wisconsin:** The coverage in Mississippi is estimated to be between the same as and 6% more than in Wisconsin.

## How to View

To view the full project report, including all tables and plots: 
1) `git clone https://github.com/maven2306/vaccination-bayesian-project.git`
2) open the `project.html` file or run the `project.rmd` file in Rstudio.

## R Libraries Used

*   `tidyverse`
*   `purrr`
*   `DT`
*   `R2OpenBUGS`
*   `coda`
*   `ggplot2`
*   `patchwork`
*   `HDInterval`
