# SDOH_Housing_Health_CA

**Status:** In progress — data loading and inspection phase

Panel analysis examining whether housing unaffordability predicts population 
health outcomes across 58 California counties, 2016–2024.

---

## Research Question

Does housing unaffordability, measured by the Median Multiple (median home 
value divided by median household income), predict worse health outcomes at 
the county level in California, after controlling for relevant covariates?

## Hypothesis

Counties with higher Median Multiple values will show higher rates of 
premature death, preventable hospital stays, and poor mental health days, 
consistent with housing unaffordability operating as a structural social 
determinant of health.

---

## Data Sources

1. **County Health Rankings (Outcomes)**  
   University of Wisconsin Population Health Institute. County Health Rankings 
   & Roadmaps. California Data and Resources, 2016–2024.  
   https://www.countyhealthrankings.org/health-data/california/data-and-resources

2. **Median Household Income (Median Multiple Denominator)**  
   U.S. Census Bureau. American Community Survey 5-Year Estimates, Table S1901: 
   Income in the Past 12 Months. Washington, DC: U.S. Census Bureau, 2016–2024.  
   https://data.census.gov

3. **Median Home Value (Median Multiple Numerator)**  
   Zillow Research. Zillow Home Value Index (ZHVI): County-Level Middle Tier 
   (Smoothed, Seasonally Adjusted), 2016–2024.  
   https://www.zillow.com/research/data/

---

## Variables

**Primary Predictor**  
- Median Multiple: median home value (Zillow ZHVI, calendar year average) 
  divided by median household income (ACS 5-Year Estimates), computed at the 
  county-year level

**Outcome Variables**  
- Premature Death (years of potential life lost before age 75 per 100,000 
  population, age-adjusted)
- Preventable Hospital Stays (discharges for ambulatory care-sensitive 
  conditions per 100,000 Medicare enrollees)
- Poor Mental Health Days (average number of mentally unhealthy days reported 
  in past 30 days)

**Unit of Analysis**  
County-year (58 California counties × 9 years = 522 observations)

---

## Analytical Plan

1. Descriptive statistics and exploratory data analysis
2. Core panel regression: fixed effects models regressing each health outcome 
   on Median Multiple, controlling for county and year fixed effects
3. Mediation analysis: examining poverty rate and low birth weight as potential 
   mediators of the housing-health relationship
4. Visualization: Tableau choropleth dashboard displaying county-level 
   variation in Median Multiple and health outcomes over time

---

## Reproducing the Analysis

Run notebooks in order:

1. `01_data_loading_inspection.ipynb` — load and inspect all three data sources
2. `02_cleaning_merging.ipynb` — clean each source, construct Median Multiple, 
   build analysis-ready panel
3. `03_analysis.ipynb` — EDA, fixed effects regressions, mediation analysis
4. `04_visualizations.ipynb` — summary figures and Tableau-ready outputs

**Dependencies:** see `requirements.txt`

---

## Repository Structure

SDOH_Housing_Health_CA/
├── data/
│   ├── raw/          # Source files, never modified
│   ├── processed/    # Cleaned individual sources
│   └── final/        # Merged analysis-ready panel
├── notebooks/
├── src/              # Utility functions
├── outputs/
│   ├── figures/
│   └── tables/
├── README.md
└── requirements.txt

---

## Author

Douglas Jaser  
MS, Data Science — Illinois Institute of Technology  
Background: Clinical health consulting and functional nutrition (15 years)  
GitHub: github.com/Djaser23