# TODO.md

## Status: In Progress — Data Loading and Inspection Phase

---

## Active To-Do

**README**
- [ ] Verify CHR outcome variable definitions match exact column names in source files
- [ ] Confirm poverty rate and low birth weight are available in CHR files before committing to them as mediators
- [ ] Add `requirements.txt` once environment is set

**Data**
- [ ] Open and inspect CHR files (2016–2024)
- [ ] Open and inspect ACS income CSVs
- [ ] Open and inspect Zillow ZHVI file
- [ ] Confirm 2020 ACS gap — is it missing entirely or partially available?

**Notebook 01**
- [ ] Build skeleton once initial data inspection is done

---

## Completed

*(Items move here when done)*

---

## Session Notes

**2026-05-13**
- Set up project folder structure
- Drafted README.md
- Established analytical plan: 4 notebooks, long format panel, 
  522 target rows, calendar year average for Zillow collapse, 
  FIPS codes as county identifier
- Next: open CHR files and begin data inspection

**2026-05-13**
- Inspected CHR 2024 file: confirmed 6 sheets, selected Select Measure Data 
  as primary sheet for all three outcome variables and both mediator variables
- Confirmed variables: Premature Death (YPLL Rate), Preventable Hospital Stays 
  (Preventable Hospitalization Rate), Poor Mental Health Days (Average Number 
  of Mentally Unhealthy Days), Low Birthweight, Children in Poverty
- Confirmed FIPS present, 59 rows (58 counties + 1 state row to drop)
- Noted: FIPS loads as integer, needs zero-padding to 5 digits in cleaning
- Noted: small counties (e.g. Alpine) will have suppressed data — handling 
  strategy TBD
- Noted: 2016-2019 files are .xls, 2020-2024 are .xlsx — need to account for 
  in loading loop
- Resolved kernel mismatch (Anaconda vs system Python)
- Notebook file moved to correct notebooks/ folder
- Next: update TODO, commit notebook, then begin CHR loading loop across all years


**2026-05-14**
- Resolved working directory issue — notebooks run from notebooks/ folder, 
  requires ../ prefix for all file paths
- Wrote markdown cells documenting variable selection rationale
- Confirmed outcome variables: Premature Death (YPLL Rate), Preventable 
  Hospital Stays (Preventable Hospitalization Rate), Poor Mental Health Days 
  (Average Number of Mentally Unhealthy Days)
- Confirmed mediators: Children in Poverty, Low Birthweight
- Discussed and rejected Life Expectancy as outcome in favor of YPLL
- Discussed mediation analysis framework — to be implemented after core 
  regression if time permits
- Successfully extracted outcome and mediator columns from df_select into 
  separate dataframes
- Next: drop state row (FIPS 6000), inspect missingness across outcome 
  variables, begin loading loop across all years


  **Research Questions for Future Analysis**
- [ ] Investigate threshold effects in Median Multiple — do poverty rates and 
  health outcomes accelerate above certain levels (moderately unaffordable >3.0, 
  seriously unaffordable >4.1, severely unaffordable >5.1)?
- [ ] Test for nonlinearity in core regression models — if residuals suggest 
  nonlinear relationship, explore threshold modeling
- [ ] Consider whether Median Multiple levels predict childhood poverty 
  nonlinearly — does housing insecurity compound poverty at severe thresholds?

  **2026-05-15**
- Installed xlrd for .xls file support via conda
- Built loop to check sheet names across all years
- Discovered significant format change: 2016-2023 use 'Ranked Measure Data', 
  2024 uses 'Select Measure Data' — sheets are not consistent across analysis window
- Next: open a 2016-2023 file and check whether 'Ranked Measure Data' contains 
  the same outcome variables (Premature Death YPLL, Preventable Hospital Stays, 
  Poor Mental Health Days) before building the full loading loop

  **2026-05-15**
- Built loop to check sheet names across all years — confirmed format change:
  2016-2023 use 'Ranked Measure Data', 2024 uses 'Select Measure Data'
- Built loop to check first-level column names across all years
- Confirmed all three outcome variables present in every year
- Identified two naming inconsistencies requiring standardization in cleaning:
  1. Capitalization: 2016-2022 lowercase, 2023-2024 title case
  2. Second-level column names differ between years (e.g. 'Mentally Unhealthy 
     Days' vs 'Average Number of Mentally Unhealthy Days')
- Next: review loop logic, then check second-level column names across years,
  then begin building the full loading pipeline

  **2026-05-16**
- Reviewed CHR column name loop logic line by line
- Confirmed all CHR naming inconsistencies mapped across all years
- Began ACS inspection: loaded Column-Metadata and Data files for 2024
- Confirmed target column: 'S1901_C01_012E' (Median Household Income)
- Confirmed GEO_ID filter for California counties: '0500000US06'
- Noted suppression codes '(X)' and 'N' require handling in cleaning
- Noted 2020 ACS gap due to COVID-19 — known limitation
- Noted Data file uses single-row header unlike CHR
- Next: inspect Zillow ZHVI file, then notebook 01 is complete and 
  notebook 02 cleaning pipeline can begin

**2026-05-18**
- Built complete CHR cleaning pipeline in notebook 02
- All 9 years loading correctly with right sheet names
- Standardized first-level column names to title case
- Mapped and handled second-level naming inconsistencies across years
- Selected 7 target columns, renamed to consistent standard names
- Added year column, stacked all years into chr_panel: 522 rows x 8 columns
- FLAG: Preventable Hospital Stays values differ dramatically between years 
  (2016 ~35-46 vs 2024 ~1774-3939) — possible methodology/unit change. 
  Must investigate CHR documentation before analysis.
- Next: investigate preventable hospital stays unit change, then build 
  ACS and Zillow cleaning pipelines  

  **2026-05-20**
- Investigated Preventable Hospital Stays unit change via CHR documentation
  - 2016-2018: per 1,000 Medicare enrollees
  - 2019-2024: per 100,000 Medicare enrollees
  - Correction applied in loop: 2016-2018 values multiplied by 100
- CHR panel confirmed clean: 522 rows x 8 columns, saved to data/processed/chr_panel.csv
- Began ACS cleaning pipeline
- Discovered significant data gap: ACS 1-year estimates only cover 40 of 58 
  CA counties — 18 rural counties suppressed due to small population
- Missing counties: Alpine, Amador, Calaveras, Colusa, Del Norte, Glenn, 
  Inyo, Lassen, Mariposa, Modoc, Mono, Plumas, San Benito, Sierra, 
  Siskiyou, Tehama, Trinity, Tuolumne
- Decision: use 5-year ACS estimates as anchor points for missing counties,
  then interpolate annual values using rural income growth rates from 
  observed 1-year counties
- Next: download 5-year ACS files for multiple years covering 2016-2024 
  window, then build imputation pipeline for 18 missing counties


  **2026-05-20**
- Downloaded 5-year ACS files for 2016-2024 (all 9 years)
- Verified all 18 missing rural counties present in 5-year files
- Built complete ACS pipeline combining 1-year (329 rows) and 5-year (153 rows) estimates
- Added income_source flag ('1yr' or '5yr') for transparency
- Handled duplicate counties using sort + drop_duplicates to prefer 1-year data
- Final ACS panel: 482 rows across 8 years (2020 rural only)
- Saved acs_panel.csv to data/processed/
- Built complete Zillow pipeline:
  - Filtered to 58 CA counties
  - Constructed 5-digit FIPS from StateCodeFIPS + MunicipalCodeFIPS
  - Collapsed monthly to annual averages
  - Melted to long format: 522 rows x 4 columns
- Saved zillow_panel.csv to data/processed/
- Next: merge all three panels, construct Median Multiple, begin EDA

**2026-05-21**
- Completed full panel merge: CHR + ACS + Zillow = 522 rows x 12 columns
- Constructed Median Multiple (median_home_value / median_household_income)
- Confirmed nulls: 18 premature death (Alpine/Sierra suppression), 
  40 income/median_multiple (2020 COVID ACS gap — documented, not imputed)
- Standardized FIPS codes across all three sources to 5-digit zero-padded string
- Saved full_panel.csv to data/processed/
- Began notebook 02 cleanup — CHR section commented and summarized
- Deleted 3 diagnostic cells (LBW check, title case verify, Premature Death check)
- Next: continue notebook 02 cleanup (ACS, Zillow, merge sections), 
  then begin notebook 03 EDA and analysis

## Notebook 02 — Complete

All three data sources have been cleaned, standardized, and merged into a 
single analysis-ready panel saved to `data/processed/full_panel.csv`. 
Proceed to notebook 03 for exploratory data analysis and regression modeling.

**2026-05-22**
- Began notebook 03 EDA and analysis
- Loaded full_panel.csv and confirmed integrity (522 rows x 12 columns)
- Ran descriptive statistics and wrote interpretation markdown
- Built first visualization: Median Multiple distribution histogram with 
  Demographia affordability threshold (3.0) marked
- Key finding: virtually all California county-years fall above the 3.0 
  affordability threshold — housing unaffordability is near-universal in CA
- Next: complete histogram interpretation markdown, then build distribution 
  plots for outcome variables, correlation matrix, time trends

**Observation regarding limitations** 
  Furthermore, the use of debt to maintain baseline living standards in 
high-cost counties may blunt near-term health effects. Residents may 
sustain adequate nutrition, healthcare access, and housing quality through 
credit — deferring the health consequences of unaffordability. Given that 
Premature Death is measured as years of potential life lost before age 75, 
the true health burden of housing unaffordability may manifest on a 
generational timescale that a 9-year panel analysis cannot capture.
This is refered to as latency effects in epidemiology and intergenerational health transmission. 
Longitudinal cohort studies would be better suited to capture these delayed effects than cross-sectional or short panel data.


**2026-05-23**
- Completed Median Multiple distribution analysis and interpretation
- Calculated: only 1.2% of county-years fall below affordability threshold (3.0)
- Identified mode (5.1), documented range restriction and residential sorting 
  as methodological limitations
- Built outcome variable distribution histograms (premature death, poor mental 
  health days, preventable hospitalization rate)
- Identified trimodal pattern in premature death — suggests distinct geographic 
  county clusters
- Investigated outliers for premature death and preventable hospitalization rates
- Key finding: Trinity and Lake counties dominate high premature death rates; 
  Marin and San Mateo dominate low rates — confirming wealth/geography confounding
- Mono County identified as anomaly warranting further investigation
- Preventable hospitalization rates declined ~50% from 2016-2024
- Correlation matrix confirmed negative MM-health outcome correlations — 
  interpreted as wealth confounding, motivates fixed effects approach
- Next: time trend visualizations, then fixed effects regression
---