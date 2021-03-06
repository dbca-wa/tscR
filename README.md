
<!-- README.md is generated from README.Rmd. Please edit that file -->

# `tscr`: An R client for the Threatened Species and Communities DB (TSC) <img src="man/figures/tscr_logo.png" align="right" alt="tscr logo" width="120" />

<!-- badges: start -->

[![CI](https://github.com/dbca-wa/tscr/workflows/tic/badge.svg)](https://github.com/dbca-wa/tscr/actions)
[![Test
coverage](https://codecov.io/gh/dbca-wa/tscr/branch/master/graph/badge.svg)](https://codecov.io/gh/dbca-wa/tscr?branch=master)
[![Last-changedate](https://img.shields.io/github/last-commit/dbca-wa/tscr.svg)](https://github.com/dbca-wa/tscr/commits/master)
[![GitHub
issues](https://img.shields.io/github/issues/dbca-wa/tscr.svg?style=popout)](https://github.com/dbca-wa/tscr/issues)
<!-- badges: end -->

The goal of `tscr` is to provide access to TSC data, and to provide
working examples of analysis and visualisation of TSC data to answer QA,
ecological, and management questions.

## For conservation managers and ecologists

Here we’ll list working examples of conservation management nature,
ecological, or data QA questions answered with TSC data.

Example questions (to be replaced with working examples):

  - How many birds rose in conservation listing status in WA in the past
    X years?
  - Which conservation significant plants have been recorded in DBCA
    District X?
  - What is the predicted habitat for a taxon known under taxonomic name
    X?
  - Generate a formatted PDF and a plain CSV spreadsheet of threatened
    flora and fauna names in WA.
  - Generate an interactive map of all accepted occurrences in area X.
  - List conservation documents coming up for review within the next 6
    months.
  - List occurrences flagged for review.

Help us completing this list by filing new “Data export or analysis
requests” [here](https://github.com/dbca-wa/tscr/issues/new/choose).

## For data engineers

If you wish to run any of the included working examples or create new
data analyses, install this package and configure `tscr` to TSC’s API
with your own credentials as outlined below.

Use cases:

  - Create a bulk data export into spreadsheet or GIS formats.
  - Build your own analyses and reports to answer conservation
    management, ecological, or QA questions.
  - ETL of bulk data from third parties (data returns from consultants)
    into TSC.

### Installation and Setup

You can install `tscr` from [GitHub](https://github.com/dbca-wa/tscr/)
with:

``` r
# install.packages("devtools")
remotes::install_github("dbca-wa/tscr", 
                        dependencies = TRUE, 
                        upgrade = "always",
                        build_vignettes = TRUE)
```

To set up `tscr`, run `usethis::edit_r_environ()`, add your TSC API
Token, then restart your R session.

``` r
TSC_API_TOKEN="Token xxx"
```

Read vignette “Setup” (online
[here](https://dbca-wa.github.io/tscr/articles/setup.html)) to learn
more about the configuration of `tscr`:

``` r
vignette("setup", package = "tscr")
```

## For package maintainers and contributors

### Contribute

Found a bug in `tscr`, need a new `tscr` feature, or need a working
example to generate a data product from TSC? Let us know
[here](https://github.com/dbca-wa/tscr/issues/new/choose)\!

Want to chat about TSC? Join the [“TSC” group on DBCA’s
Teams](https://teams.microsoft.com/_#/conversations/General?threadId=19:20412eea61c949e59460ece939a128cd@thread.tacv2&ctx=channel)\!
(You’ll need a DBCA account to access this group.)

### Release

Tasks that can be run for each release are shown below.

``` r
usethis::use_version(which = "dev") # patch, dev, minor, major

# Rebuild included package data, used for tests and vignettes
source(here::here("data-raw/make_data.R"))

# Tests shall pass
devtools::test()

styler::style_pkg()
lintr:::addin_lint_package() # some lint errors are OK
devtools::document(roclets = c("rd", "collate", "namespace", "vignette"))
spelling::spell_check_package()
spelling::spell_check_files("README.Rmd", lang = "en_AU")
spelling::update_wordlist()
codemetar::write_codemeta("tscr")
if (fs::file_info("README.md")$modification_time <
  fs::file_info("README.Rmd")$modification_time) {
  rmarkdown::render("README.Rmd", encoding = "UTF-8", clean = TRUE)
  if (fs::file_exists("README.html")) fs::file_delete("README.html")
}
#
# Checks
goodpractice::goodpractice(quiet = FALSE)
devtools::check(cran = FALSE, remote = TRUE, incoming = TRUE)
#
# Add new feature to news if user-facing
usethis::edit_file("NEWS.md")

# Commit and push
```
