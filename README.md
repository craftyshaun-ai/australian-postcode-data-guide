# Australian postcode data guide

A practical reference to the **ATO postcode taxation statistics** and **AIHW Medicare local-area statistics** used in the Cash Savvy pawn-location research. This repository contains documentation only—no government data files or derived postcode records.

## What this enables

If an organisation has a cohort of users with postcodes, it can:

1. aggregate the cohort by postcode;
2. attach public area-level attributes to each postcode;
3. build a comparable area profile using rates, medians and per-person measures;
4. find other postcodes with similar profiles; and
5. validate those candidate areas with local research.

The result is an **area-level comparison**, not a claim about any individual user and not proof of causation.

## Sources at a glance

| Source | Publisher | Native geography | What it describes | Main formats |
|---|---|---|---|---|
| [Taxation statistics – postcode data](https://data.gov.au/data/dataset/taxation-statistics-postcode-data) | Australian Taxation Office (ATO) | Return-address postcode, with state and SA4 fields | Tax-lodger counts, income, tax, deductions, investment, business, super and related labels | XLSX; older files XLS |
| [Individuals – Table 25](https://data.gov.au/data/dataset/taxation-statistics-2023-24/resource/d6122018-989a-453c-bd40-294990adbe40) | ATO | Postcode | Ready-made counts, averages, medians and claimant proportions for selected items | XLSX |
| [Medicare-subsidised care across local areas](https://www.aihw.gov.au/reports/primary-health-care/medicare-subsidised-gp-allied-health-specialist/data) | Australian Institute of Health and Welfare (AIHW) | National, PHN and SA3 | Use of non-hospital Medicare-subsidised GP, allied-health, specialist and related services | XLSX and zipped CSV |

## 1. ATO postcode taxation statistics

### What is available

The canonical data.gov.au archive provides annual postcode workbooks from **1994–95 to 2023–24**. The current broad workbook contains:

- **Table 6A:** taxable status × state/territory × SA4 × postcode;
- **Table 6B:** all individuals × state/territory × SA4 × postcode;
- paired counts and aggregate dollar amounts for taxable income, salary and wages, tax, deductions, rent, capital gains, interest, dividends, HELP, superannuation, business income and related labels.

ATO **Table 25** is a smaller, easier postcode profile containing counts, averages, medians and proportions for selected measures.

### Where to find it

- [Canonical annual postcode archive](https://data.gov.au/data/dataset/taxation-statistics-postcode-data)
- [Machine-readable CKAN catalogue](https://data.gov.au/data/api/3/action/package_show?id=5fa69f19-ec44-4c46-88eb-0f6fd5c2f43b)
- [2023–24 resource page](https://data.gov.au/data/dataset/taxation-statistics-postcode-data/resource/a9e5414f-68e7-48b2-89d4-3b32e927d57e)
- [2023–24 direct XLSX](https://data.gov.au/data/dataset/5fa69f19-ec44-4c46-88eb-0f6fd5c2f43b/resource/a9e5414f-68e7-48b2-89d4-3b32e927d57e/download/ts24individual06taxablestatusstatesa4postcode.xlsx)
- [Index of Labels](https://data.gov.au/data/dataset/taxation-statistics-postcode-data/resource/9979b631-779a-42a5-865f-90aa562cd103)
- [2023–24 Table 25](https://data.gov.au/data/dataset/taxation-statistics-2023-24/resource/d6122018-989a-453c-bd40-294990adbe40)

### Format and reliable access

Use the annual **XLSX** workbook as the canonical source and parse `Table 6A` or `Table 6B`. Preserve postcode as four-character text. For automated discovery, query the CKAN `package_show` endpoint and select the resource by year/name rather than guessing a download URL.

Do not assume data.gov.au's generated CSV/JSON view represents the workbook tables: for the 2023–24 resource it exposes the Notes sheet rather than a useful flattened Table 6. Older archive years are XLS.

### Correct interpretation

- The population is **people represented in individual tax returns**, not every resident.
- Postcode comes from residential/postal details on returns and is not automatically identical to an ABS Postal Area (POA).
- Means can be distorted by a small number of high-income lodgers; retain counts and use medians where available.
- Privacy rules can combine or omit cells. Missing or suppressed values are not zero.
- For 2010–11 onward the archive includes taxable and non-taxable individuals; 2009–10 and earlier is taxable individuals only.
- Use one year, one sheet and one cohort definition when comparing places.

## 2. AIHW Medicare local-area statistics

### What is available

The current PHC 19 release covers **2017–18 to 2024–25** and reports non-hospital Medicare-subsidised services. Measures include:

- percentage of people who had a service;
- services per 100 people and age-standardised service rates;
- Medicare benefits per 100 people;
- patient and service counts;
- total Medicare benefits and provider fees; and
- estimated resident population used for rates.

The service hierarchy includes GP, diagnostic imaging, allied health, specialist, nursing and Aboriginal and Torres Strait Islander health-worker services. Some demographic breakdowns are available, subject to geography and suppression.

### Where to find it

- [Stable AIHW data landing page](https://www.aihw.gov.au/reports/primary-health-care/medicare-subsidised-gp-allied-health-specialist/data)
- [Current XLSX data tables](https://www.aihw.gov.au/getmedia/deb8354f-9231-41d4-8035-a6561c11a762/aihw-phc-019-data-tables-1718-2425.xlsx)
- [2023–24 and 2024–25 zipped CSV files](https://www.aihw.gov.au/getmedia/a395607b-91b5-411d-983c-27a0b79feaf7/aihw-phc-019-csv-file-2324-2425.zip)
- [Technical notes: geography](https://www.aihw.gov.au/reports/primary-health-care/medicare-subsidised-gp-allied-health-specialist/contents/technical-notes/geography)
- [Technical notes: scope and measures](https://www.aihw.gov.au/reports/primary-health-care/medicare-subsidised-gp-allied-health-specialist/contents/technical-notes/scope-and-measures-of-the-report)

### Format and reliable access

AIHW publishes a presentation-oriented **XLSX** and machine-readable annual **CSV files in ZIP archives**. The CSV schema includes reporting year, state/territory, geographic unit/code/name, service level and type, demographic group, measure name/value, and suppression flags. No query API is documented for this product; download from the stable landing page.

### Geography: the key caveat

AIHW publishes local results at **PHN and SA3**, not postcode. Patient geography is derived from Medicare enrolment postcode, then allocated to published areas. It is based on where the patient is recorded as living—not where care was delivered.

To relate this source to a postcode cohort:

1. obtain an ABS POA-to-SA3 correspondence, or spatially map a POA representative point to ASGS 2021 SA3 boundaries;
2. retain the SA3 code, mapping method and boundary/quality flags;
3. attach the SA3 value to each postcode; and
4. label it **“SA3 measure mapped to postcode”**, never a postcode-level Medicare rate.

Every postcode mapped to the same SA3 will share the same Medicare value. A centroid method can differ from AIHW's weighted treatment of postcodes that cross boundaries.

### Correct interpretation

- These data cover services for which an MBS benefit was paid, not all health-care use.
- Residence geography may be stale, invalid or a PO Box, and is not provider location.
- Suppressed or flagged results must remain missing/flagged, not zero.
- Changes to MBS items and geographic boundaries can break time-series comparability.
- Higher or lower use is not inherently better and does not prove need, access or health status.
- `100 × benefits ÷ provider fees` can be derived as a benefit-to-fee percentage, but it is **not a bulk-billing rate**.

## Reusable cohort-to-similar-postcode method

```text
user postcodes
      ↓ aggregate counts; apply privacy threshold
cohort postcode distribution
      ↓ join ATO directly by postcode
      ↓ map postcode/POA → SA3, then join AIHW
area feature matrix
      ↓ choose one period/cohort; create rates/medians
      ↓ log/robust-scale skewed measures; retain counts
cohort profile
      ↓ weighted distance or nearest-neighbour search
ranked similar postcodes
      ↓ local validation and sensitivity checks
candidate areas—not individual conclusions
```

### Minimum analysis controls

1. **Define the cohort first.** Record inclusion dates, postcode source, duplicate handling and minimum count.
2. **Apply privacy thresholds.** Do not publish small user counts or reverse-engineer suppressed government cells.
3. **Align periods.** Do not silently compare 2023–24 tax data with another period as though simultaneous.
4. **Use comparable features.** Prefer proportions, medians and per-person/per-lodger measures over aggregate totals.
5. **Respect geography.** ATO postcode, ABS POA and AIHW SA3 are related but not interchangeable.
6. **Retain reliability fields.** Keep lodger/population counts, suppression flags, mapping method and source year.
7. **Standardise before matching.** Otherwise dollars or population size dominate distance calculations.
8. **Test sensitivity.** Re-run with different feature groups/weights and inspect whether candidates remain stable.
9. **Validate locally.** Similar public-data profiles identify places to investigate; they do not prove opportunity or cause.

## Example uses

- Find areas whose tax-lodger income, deductions and claimant mix resemble an existing customer catchment.
- Add SA3-level GP service-use context to a postcode market profile.
- Rank candidate postcodes by similarity to a successful cohort before local competition/property research.
- Identify matched comparison areas for pilots or service planning.
- Segment an existing footprint into area archetypes without exposing individual records.

## What not to conclude

Do not infer an individual's income, tax position, health status or service use from their postcode. Do not describe postcode similarity as proof that the same commercial or social outcome will occur. These are aggregate ecological datasets with different populations, periods and geographies.

## Proven implementation baseline

This guide is based on the ingestion approach already exercised in the Cash Savvy pawn-location model:

- ATO 2023–24 postcode workbook parsed into postcode-level tax-lodger features;
- AIHW 2023–24 CSV measures filtered at SA3, then mapped to POAs;
- ABS 2021 POA centroids mapped to ASGS 2021 SA3 boundaries;
- source hashes, geography and feature semantics retained in a manifest; and
- ATO and Medicare feature groups tested separately against an ABS baseline.

In that model, the extra ATO/Medicare features added useful descriptive context but did **not** improve grouped out-of-sample performance. That is a useful control: more postcode data does not automatically produce a better model.

## Repository scope

This repository intentionally contains no source datasets, user data, model output or proprietary code. Follow the publishers' current licences and attribution requirements when downloading or redistributing their files.

## Agents

When updating this reference:

1. verify links against the official AIHW, ATO and data.gov.au pages;
2. use the stable landing/catalogue pages for discovery and direct URLs only as download conveniences;
3. preserve native geography and population definitions;
4. update periods, formats and caveats together;
5. never add source data or user-level records to the repository; and
6. test `index.html` at desktop and mobile widths before publishing.
