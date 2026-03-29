# Structural Determinants of Non-Communicable Disease Mortality Across Countries

> **BSc Applied Economics** · Corvinus University of Budapest · 2025  
> **Course:** Project 2 Research · **Instructor:** Dr. Ferenci Tamás

---

## Abstract

This paper investigates the structural drivers of premature non-communicable disease (NCD) mortality across **183 countries** using 2019 cross-sectional data from the World Bank, WHO, and IHME. Employing both **Ordinary Least Squares (OLS)** and **Quantile Regression (QR)**, the study finds that national income (GNI per capita), air pollution mortality, and suicide rate are the strongest and most consistent predictors of NCD mortality. The dual-method approach reveals important heterogeneity — predictor effects differ substantially between low- and high-burden countries — highlighting that one-size-fits-all policy responses are insufficient.

---

## Research Hypothesis

> *Country-level premature NCD mortality is shaped by a complex interplay of structural determinants — including economic capacity, behavioural risk factors, and environmental stressors. Specifically, higher alcohol consumption, lower national income, elevated pollution, and limited access to clean water are jointly associated with higher NCD mortality across countries.*

---

## Key Findings

### OLS Results — Final Model (Model 5, Adjusted R² = 0.706)

| Variable | Coefficient | P-Value | Significant? |
|---|---|---|---|
| GNI per Capita (per $1,000) | −0.08 | **0.001** | ✅ Yes |
| Suicide Rate (per 100k) | +0.21 | **<0.001** | ✅ Yes |
| PM2.5 Exposure | −0.10 | **0.01** | ✅ Yes |
| Air Pollution Mortality (per 100k) | +0.08 | **<0.001** | ✅ Yes |
| Alcohol per Capita | −0.15 | 0.24 | ❌ No |
| Safe Water Access (%) | +0.04 | 0.15 | ❌ No |

### Quantile Regression — Key Patterns

| Variable | Q25 | Q50 | Q75 | Pattern |
|---|---|---|---|---|
| GNI per Capita | −0.093 | −0.095 | −0.079 | Consistently protective |
| Suicide Rate | +0.165 | +0.299 | +0.360 | Effect grows with burden |
| Air Pollution Mortality | +0.089 | +0.090 | +0.087 | Stable across all quantiles |
| Alcohol per Capita | +0.093 | −0.004 | −0.135 | Reverses sign at high burden |

### Model Selection (BIC Ranking)

Model 5 (*Full Socio-Environmental, GNI*) achieved the lowest BIC of **699.33**, indicating the best balance of fit and parsimony across 9 candidate models.

---

## Data

| Source | Variables |
|---|---|
| [World Bank World Development Indicators](https://databank.worldbank.org/source/world-development-indicators) | GNI, GDP, health expenditure, hospital beds |
| [WHO Global Health Observatory](https://www.who.int/data/gho) | NCD mortality, alcohol, suicide rate |
| [IHME Global Burden of Disease](https://www.healthdata.org/gbd) | Air pollution mortality, PM2.5 |
| [WHO/UNICEF JMP](https://washdata.org/) | Safe water access |

- **Year:** 2019 (cross-sectional; chosen for consistency across all indicators)
- **Initial sample:** ~230 observations (countries + regional aggregates)
- **Final sample:** 183 individual countries

### Key Variables

| Variable | Description | Source |
|---|---|---|
| `ncd_mortality_pct` | Probability (%) of dying aged 30–70 from cardiovascular disease, cancer, diabetes, or chronic respiratory disease | WHO / SDG 3.4.1 |
| `alcohol_per_capita` | Total alcohol consumption (litres of pure alcohol, aged 15+) | WHO |
| `gni_usd_by1000` | GNI per capita in USD, scaled by 1,000 | World Bank |
| `suicide_rate` | Suicide mortality rate per 100,000 population | WHO / SDG 3.4.2 |
| `pm2_5` | Mean annual PM2.5 exposure (µg/m³) | World Bank |
| `air_pollution_mort` | Mortality rate per 100,000 from household + ambient air pollution | WHO / SDG 3.9.1 |
| `safe_water_pct` | Share of population with access to safely managed drinking water | JMP / SDG 6.1.1 |

---

## Methodology

1. **Data Cleaning** — reshaped World Bank long-format export to wide; removed regional aggregates; dropped variables with missingness or near-perfect collinearity (>0.95 pairwise correlation)
2. **Multicollinearity Screening** — correlation heatmaps + VIF analysis (threshold: VIF > 5)
3. **Model Selection** — 9 OLS specifications ranked by Bayesian Information Criterion (BIC)
4. **OLS Regression** — estimates average structural effects across all countries
5. **Quantile Regression** — estimates effects at the 25th, 50th, and 75th percentiles to capture distributional heterogeneity

**OLS Model Equation:**

```
NCD_Mortality_i = β₀ + β₁·Alcohol_i + β₂·GNI_i + β₃·Suicide_i
                     + β₄·PM2.5_i + β₅·AirPollMort_i + β₆·SafeWater_i + ε_i
```

---

## Repository Structure

```
ncd-mortality-determinants/
│
├── README.md                                          # This file
├── CITATION.cff                                       # Machine-readable citation
├── .gitignore                                         # R/Quarto/OS artefacts
│
├── Structural_Determinants_of_NCDs_Mortality_...pdf   # Full paper
├── main_code.qmd                                      # Quarto code - analysis file (R)
├── Data_dirty.xlsx                                    # Raw World Bank dataset
│
└── outputs/                                           # Auto-generated on render
    ├── data_ready.xlsx                                # Cleaned analysis dataset
    ├── df_cor_matrix.xlsx                             # Full correlation matrix
    ├── bic_table.xlsx                                 # BIC model rankings
    ├── vif_table.xlsx                                 # VIF diagnostics
    ├── ols_regression_model5.xlsx                     # Final OLS results
    ├── ols_regression_model6.xlsx                     # Robustness check (GDP)
    ├── quantile_regression_coef_table.xlsx            # QR coefficient table
    └── descriptive_stats.xlsx                         # Descriptive statistics
```

---

## How to Reproduce

### Requirements

- **R** (≥ 4.2) with **Quarto** installed
- Packages are auto-installed on first run: `readxl`, `dplyr`, `tidyr`, `ggplot2`, `corrplot`, `car`, `quantreg`, `psych`, `writexl`, `janitor`, `skimr`

### Steps

1. Clone or download this repository
2. Place `Data_dirty.xlsx` in the **same directory** as `main_work-Project2.qmd`
3. Open a terminal in that directory and run:

```bash
quarto render main_work-Project2.qmd
```

4. All output files will be saved to the `outputs/` folder (created automatically)

> **Note:** The first render may take a few minutes while packages install and the cache builds. Subsequent renders are faster due to Quarto's caching.

---

## Author

| Name | Institution |
|---|---|
| **Meshkovski Nikola** | Corvinus University of Budapest, BSc Applied Economics |

---

## References

- Eckelman, M. J., & Sherman, J. (2016). Environmental impacts of the U.S. health care system. *PLOS ONE*, 11(6).
- Ezzati, M. et al. (2018). NCD mortality in low- and middle-income tropical countries.
- López-Tenorio, J. et al. (2024). Microbiome and NCD onset — a comprehensive review.
- Murphy, A. et al. (2020). The household economic burden of NCDs in 18 countries. *BMJ Global Health*, 5(2).
- Schwarz, G. (1978). Estimating the dimension of a model. *Annals of Statistics*, 6(2), 461–464.
- Wang & Wang (2020). Multilevel modelling of NCD mortality. WHO data, 176 countries.
- World Bank (2024). World Development Indicators. https://databank.worldbank.org
- WHO (2021). Noncommunicable diseases: Mortality. https://www.who.int
- Zhou, M. et al. (2020). Modelling and prediction of global NCDs. *BMC Public Health*, 20.

---

## License

This project is for academic purposes. Raw data is sourced from publicly accessible World Bank, WHO, and IHME databases and subject to their respective terms of use.
