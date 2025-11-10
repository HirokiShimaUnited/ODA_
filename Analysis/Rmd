ODA Analysis & WDI Dashboard —
Comprehensive Report
Python Version — Modern Data Science Theme (Dark header)

Abstract
This report presents an automated pipeline for analyzing Official Development Assistance (ODA) data
from OECD CRS, World Bank WDI indicators, and population/income datasets. The analysis aggregates
ODA disbursements, reshapes multi-year datasets, merges sources, and computes lags and z-scores for
cross-country comparisons. Key outputs include education-focused ODA trends and visualizations. The
pipeline is fully reproducible in R and designed for academic or policy research.
Methodology & Pipeline Steps
The pipeline follows multiple steps: data loading and cleaning, reshaping WDI & population data,
aggregating CRS disbursements (total and education-specific), merging datasets, computing lagged
variables and z-scores, visualizing relationships, and exporting processed outputs. The implementation
uses tidyverse-style processing in R; full RMarkdown source is included below for reproducibility.
Sample Visualization (Simulated)
Education ODA vs Literacy — simulated scatter to illustrate the expected plot.
---
title: "ODA Analysis Pipeline: A Comprehensive Study of Official Development Assistance"
author: H.S
date: "`r Sys.Date()`"
output:
pdf_document:
toc: true
latex_engine: xelatex
keep_tex: false
html_document:
toc: true
toc_float: true
theme: cosmo
highlight: tango
self_contained: true
abstract: |
This report presents an automated pipeline for analyzing Official Development Assistance
(ODA) data from OECD CRS, World Bank WDI indicators, and population/income datasets. The
analysis aggregates ODA disbursements, reshapes multi-year datasets, merges sources, and
computes lags and z-scores for cross-country comparisons. Key outputs include
education-focused ODA trends and visualizations. The pipeline is fully reproducible in R
and designed for academic or policy research.
---
```{r setup, include=FALSE}
# Global options for knitting
knitr::opts_chunk$set(echo = TRUE, warning = FALSE, message = FALSE, fig.align = "center")
```
Introduction
Official Development Assistance (ODA) plays a critical role in global development, with
data from sources like the OECD Creditor Reporting System (CRS), World Bank World
Development Indicators (WDI),
and population/income datasets providing insights into aid flows, economic indicators, and
demographic trends. This report outlines a data pipeline that:
- Aggregates ODA disbursements by country and year, focusing on total and
education-specific aid.
- Reshapes and cleans multi-year datasets from WDI and population sources.
- Merges datasets for integrated analysis.
- Computes lagged variables and z-scores for temporal and comparative analysis.
- Generates visualizations, such as education ODA versus literacy rates.
The pipeline is implemented in R using the tidyverse ecosystem for efficiency and
reproducibility. Results are exported as a processed CSV for further use.
Data Sources and Description
The analysis draws from three primary datasets:
- CRS Disbursements (crs_disbursements.csv): OECD data on ODA flows, including recipient
countries, sectors (e.g., education), and monetary values. Columns are dynamically handled
to accommodate variations.
- WDI Indicators (wdi_indicators.csv): World Bank data with country-level indicators
(e.g., literacy rates) across years. Data is pivoted from wide to long format.
- Population/Income Data (pop_income.csv): World Bank data on populati
on and income groups, similarly pivoted.
All datasets are cleaned using janitor for consistent naming. Missing values are handled
via na.rm = TRUE in aggregations.
Methodology
The pipeline follows a nine-step process, implemented in R. Code chunks below execute each
step, with outputs displayed where relevant.
Step 1-2: Load and Clean Data
```{r load_clean}
# Load libraries
if (!require("tidyverse")) install.packages("tidyverse", repos =
"https://cloud.r-project.org")
if (!require("janitor")) install.packages("janitor", repos =
"https://cloud.r-project.org")
if (!require("zoo")) install.packages("zoo", repos = "https://cloud.r-project.org")
library(tidyverse)
library(janitor)
library(zoo)
# Read and clean data
crs <- read_csv("crs_disbursements.csv", show_col_types = FALSE) %>% clean_names()
wdi <- read_csv("wdi_indicators.csv", show_col_types = FALSE) %>% clean_names()
pop <- read_csv("pop_income.csv", show_col_types = FALSE) %>% clean_names()
cat("n Data loaded and names cleaned.\n")
```
Step 3: Reshape WDI Data
WDI data is pivoted to long format, extracting years and values.
```{r reshape_wdi}
wdi_year_cols <- names(wdi)[str_detect(names(wdi), "^x\\d{4}_yr\\d{4}$")]
wdi
_long <- wdi %>%
pivot_longer(cols = all_of(wdi_year_cols), names_to = "year_raw", values_to = "value") %>%
mutate(year = as.integer(str_extract(year_raw, "\\d{4}")),
country = coalesce(country_name, country_code)) %>%
select(country, country_code, series_name, year, value)
wdi_clean <- wdi_long %>%
group_by(country, year, series_name) %>%
summarise(value = mean(as.numeric(value), na.rm = TRUE), .groups = "drop") %>%
pivot_wider(names_from = series_name, values_from = value)
cat("n WDI reshaped successfully.\n")
```
Step 4: Reshape Population/Income Data
Similar pivoting for population data, with safety checks.
```{r reshape_pop}
pop_year_cols <- names(pop)[str_detect(names(pop), "^x\\d{4}_yr\\d{4}$")]
if (length(pop_year_cols) == 0) {
stop("No year columns found in pop data.")
}
pop_long <- pop %>%
pivot_longer(cols = all_of(pop_year_cols), names_to = "year_raw", values_to = "value") %>%
mutate(year = as.integer(str_extract(year_raw, "\\d{4}")),
country = coalesce(country_name_x, country_code)) %>%
select(country, country_code, income_group, year, value)
pop_clean <- pop_long %>%
group_by(country, year) %>%
summarise(population = me
an(as.numeric(value), na.rm = TRUE),
income_group = first(na.omit(income_group)), .groups = "drop")
cat("n Population/Income data reshaped.\n")
```
Step 5: Aggregate CRS (ODA) Data
Dynamically handles column variations for sector and value aggregation.
```{r aggregate_crs}
crs <- crs %>%
mutate(
sector = if ("sector" %in% names(.)) sector else
if ("purpose_name" %in% names(.)) purpose_name else
if ("purpose" %in% names(.)) purpose else NA_character_,
oda_value = if ("obs_value" %in% names(.)) obs_value else
if ("observation_value" %in% names(.)) observation_value else
if ("amount" %in% names(.)) amount else
if ("value" %in% names(.)) value else NA_real_
)
if (all(is.na(crs$oda_value))) {
warning("No valid ODA values found.")
}
country_col <- if ("recipient" %in% names(crs)) "recipient" else
if ("recipient_name" %in% names(crs)) "recipient_name" else NULL
year_col <- if ("time_period" %in% names(crs)) "time_period" else
if ("year" %in% names(crs)) "year" else NULL
if (is.null(country_col) || is.null(year_col)) {
stop("Required columns missing
in crs.")
}
oda_total <- crs %>%
group_by(country = .data[[country_col]], year = as.integer(.data[[year_col]])) %>%
summarise(oda_total = sum(as.numeric(oda_value), na.rm = TRUE), .groups = "drop")
oda_edu <- crs %>%
mutate(sector_lower = tolower(sector)) %>%
filter(str_detect(sector_lower, "education")) %>%
group_by(country = .data[[country_col]], year = as.integer(.data[[year_col]])) %>%
summarise(oda_education = sum(as.numeric(oda_value), na.rm = TRUE), .groups = "drop")
oda_summary <- oda_total %>%
left_join(oda_edu, by = c("country", "year")) %>%
mutate(oda_education = replace_na(oda_education, 0))
cat("n CRS (ODA) aggregated.\n")
```
Step 6-7: Merge and Add Variables
Datasets are merged, and lags/z-scores are computed.
```{r merge_compute}
merged <- oda_summary %>%
left_join(wdi_clean, by = c("country", "year")) %>%
left_join(pop_clean, by = c("country", "year"))
merged <- merged %>%
arrange(country, year) %>%
group_by(country) %>%
mutate(oda_total_lag1 = lag(oda_total, 1),
oda_total_lag2 = lag(oda_total, 2),
oda_total_lag3 = lag(oda_total, 3)) %>%
ungroup() %>%
group_by(income_group, year) %>%
mutate(z_oda_total = ifel
se(sd(oda_total, na.rm = TRUE) > 0,
(oda_total - mean(oda_total, na.rm = TRUE)) / sd(oda_total, na.rm = TRUE), NA)) %>%
ungroup()
cat("n Datasets merged; lags and z-scores added.\n")
```
Step 8-9: Visualization and Export
Generates a plot and exports the dataset.
```{r viz_export}
lit_col <- names(merged)[str_detect(names(merged), "literacy")]
if (length(lit_col) > 0) {
p <- ggplot(merged, aes(x = .data[[lit_col[1]]], y = oda_education)) +
geom_point(alpha = 0.5, color = "steelblue") +
geom_smooth(method = "lm", se = FALSE, color = "red") +
labs(title = paste("Education ODA vs", lit_col[1]), x = lit_col[1], y = "Education ODA
(USD)") +
theme_minimal()
print(p)
} else {
cat("nn Literacy column not found — skipping plot.\n")
}
write_csv(merged, "processed.csv")
cat("n processed.csv exported.\n")
```
Results
The pipeline successfully processed the data, producing a merged dataset with r
nrow(merged) observations across r n_distinct(merged$country) countries. Key summaries:
- ODA Totals: Mean total ODA per country-year: r round(mean(merged$oda_total, na.rm =
TRUE), 2) USD.
- Education Focus: r sum(merged$oda_education > 0, na.
rm = TRUE) country-years with education ODA.
- Visualization: The scatter plot (if literacy data is available) shows a potential
positive correlation between literacy and education aid.
A sample of the processed data:
```{r sample_head}
head(merged) %>% knitr::kable(caption = "Sample of Processed ODA Data")
```
Conclusion
This pipeline provides a robust, reproducible framework for ODA analysis, integrating
multiple data sources and enabling temporal and comparative insights. Future extensions
could include additional indicators or machine learning models. The code and data are
available for replication.
References
OECD CRS Database. (Accessed via OECD website).
World Bank WDI. (worldbank.org).
Wickham, H., et al. (2019). Welcome to the Tidyverse. Journal of Open Source Software.
Conclusion
This pipeline provides a robust, reproducible framework for ODA analysis, integrating multiple data
sources and enabling temporal and comparative insights. Future extensions could include additional
indicators, more advanced visualizations, and machine learning models for predictive analysis.
References
OECD CRS Database. (Accessed via OECD website).
World Bank WDI. (worldbank.org).
Wickham, H., et al. (2019). Welcome to the Tidyverse. Journal of Open Source Software.
