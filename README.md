
## Introduction

This repository contains the data used in an analysis of the Academ-AI repository (<https://www.academ-ai.info>). The data are also available from the Open Science Framework.[^osf] The analysis document can be found on arXiv.[^arxiv]

[^osf]: Glynn A. Academ-AI Analysis. Charlottesville, VA: Open science Framework; 2024. doi:  [10.17605/OSF.IO/S4YGV](https://doi.org/10.17605/OSF.IO/S4YGV)

[^arxiv]: Glynn A. Suspected Undeclared Use of Artificial Intelligence in the Academic Literature: An Analysis of the Academ-AI Dataset. arXiv:2411.15218; 2024 [Preprint]. doi: [10.48550/arXiv.2411.15218](https://doi.org/10.48550/arXiv.2411.15218)

The `reproduce.qmd` file provides instructions (with R code) for reproducing the results of the investigation. To run the code, users must obtain an API key from [Open Exchange Rates](https://openexchangerates.org), which must be stored in one of the following two places:

1. A system environment variable named `OPEN_EXCHANGE_RATES`.
2. The R variable `oxr_key` on line 80 of the `reproduce.qmd` file.

## Dependencies

### Software

- R programming language ([R Project for Statistical Computing](https://www.r-project.org/), Vienna, Austria)
- The following R packages, all available via [CRAN](https://cran.r-project.org/):
	- `box`
  - `coin`
  - `cowplot`
  - `dplyr`
  - `forcats`
  - `ggokabeito`
  - `ggplot2`
  - `gt`
  - `gtsummary`
  - `lemon`
  - `magrittr`
  - `janitor`
  - `jsonlite`
  - `labelled`
  - `lubridate`
  - `purrr`
  - `readr`
  - `stringr`
  - `tibble`
  - `tidyr`
  - `tidyselect`
- Quarto ([Posit Software](https://posit.co/), Boston, MA, United States of America)

### Third-party data

- [Open Exchange Rates](https://openexchangerates.org/) (requires API key)
- [Directory of Open Access Journals data dump](https://doaj.org/docs/public-data-dump/)
- [Scimago Journal Rankings](https://www.scimagojr.com/journalrank.php)

Third-party data are automatically imported by the `reproduce.qmd` file, provided that the user has obtained an Open Exchange Rates API key and stored it appropriately (see Introduction).

## Data dictionary

The `acai-data.csv` file contains the following variables:

| Variable            | Type   | Label                               | Description                                                           | Format\*                                                                                                                               |
|-------------------|---------|-------------------------------------|-----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| `key`               | text    | Document ID                         | Unique document identifier                                            | `/[a-z]+[0-9]{4}[a-z]?/`                                                                                                          |
| `date`              | date    | Document date                       | Date of document publication according to publisher                   | `YYYY-MM-DD`, `YYYY-MM`, `YYYY`                                                                                                            |
| `c_type`            | factor  | Container type                      | Type of publication in which document appears                         | 1=Journal article, 2=Conference paper                                                                                                |
| `c_title`           | text    | Container title                     | Title of publication in which document appears                        |                                                                                                                                      |
| `c_isn`             | text    | Container IS\*N                     | ISSN or ISBN of publication in which document appears                 | `/[\d-]+\|None/`                                                                                                                     |
| `c_isn_valid`       | factor  | Container ISSN validity             | Validity of journal ISSN                                          | 1=Confirmed, 2=Invalid, 3=No ISSN, 4=Provisional, 5=Refers to different journal, 6=Unreported record                                 |
| `c_publisher_std`   | factor  | Container publisher standard        | Publisher if major publisher                                          | 1=Elsevier, 2=Frontiers, 3=IEEE, 4=IOP, 5=MDPI, 6=PLoS, 7=Sage, 8=SPIE, 9=Springer, 10=Taylor & Francis, 11=Wiley, 12=Wolters Kluwer |
| `c_publisher_major` | factor  | Container publisher major           | Container is published by major publisher                             | 0=False, 1=True                                                                                                                      |
| `c_apc_model`       | factor  | Container APC model                 | Open access publishing model                                          | 0=None, 1=Full, 2=Hybrid, 3=Unknown                                                                                                  |
| `c_apc_value`       | integer | Container APC value                 | Article processing charge price in relevant currency                  | \>0                                                                                                                                  |
| `c_apc_currency`    | text    | Container APC currency              | Article processing charge currency                                    | `/[A-Z]{3}/`                                                                                                                         |
| `e_date`            | date    | Erratum date                        | Date of erratum (corrigendum or retraction)                           | `YYYY-MM-DD`                                                                                                                           |
| `e_stealth`         | factor  | Stealth revision                    | Document stealth-revised                                              | 0=False, 1=True                                                                                                                      |
| `e_corr`            | factor  | Corrigendum                         | Article corrected                                                     | 0=False, 1=True                                                                                                                      |
| `e_retr`            | factor  | Retraction                          | Article retracted                                                     | 0=False, 1=True                                                                                                                      |
| `t_update`          | factor  | Text feature: model update          | Example mentions a model update or cutoff                             | 0=False, 1=True                                                                                                                      |
| `t_regenerate`      | factor  | Text feature: regenerate response   | Example includes the phrase "regenerate response"                     | 0=False, 1=True                                                                                                                      |
| `t_certainly`       | factor  | Text feature: certainly here        | Example includes the phrase "certainly here"                          | 0=False, 1=True                                                                                                                      |
| `t_langmod`         | factor  | Text feature: language model        | Example includes self-identification as an AI language model          | 0=False, 1=True                                                                                                                      |
| `t_access`          | factor  | Text feature: lack of access        | Example includes statement that speaker lacks access to relevant data | 0=False, 1=True                                                                                                                      |
| `t_first_person`    | factor  | Text feature: first-person singular | Example includes use of first-person singular                         | 0=False, 1=True                                                                                                                      |
| `t_second_person`   | factor  | Text feature: second-person         | Example includes use of second-person address                         | 0=False, 1=True                                                                                                                      |
| `t_recent`          | factor  | Text feature: referral recent       | Example includes referral to more recent or specialized sources       | 0=False, 1=True                                                                                                                      |

Notes on the format column:

- For text variables, format is given as a regular expression.
- For dates, format is given in ISO 8601 terms.
- For factors, a comma-separated list of possible numeric values and their meanings in the form `value=meaning` is given.

The `acai-data-bibliography.yaml` file contains the bibliographic metadata for every document in the dataset. The `id` field of each document in the YAML file corresponds to the `key` column of the CSV file.
