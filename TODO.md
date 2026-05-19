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
---