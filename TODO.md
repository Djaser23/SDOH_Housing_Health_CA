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

---