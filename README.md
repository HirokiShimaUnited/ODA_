📊 ODA–WDI Dashboard

An Integrated Analysis of Official Development Assistance (ODA) and Human Development Outcomes (2000–2023)

🌍 Overview

This repository presents a reproducible analysis of how education and health sector ODA relate to key human development indicators such as literacy, life expectancy, and infant mortality.
The study integrates datasets from the OECD Creditor Reporting System (CRS) and the World Bank World Development Indicators (WDI) to examine aid effectiveness and cross-sectoral connections in human development.

The project includes:

Cleaned and merged datasets (final_merged_dataset.csv)

Power BI dashboard (ODA_Analysis(final).pbix)

Analytical summary (model_summary.json)

Full technical documentation (README_technical.md)

📁 Repository Structure
ODA_Analysis/
 ├─ data/
 │   ├─ raw/
 │   │   ├─ crs_disbursements.csv
 │   │   ├─ wdi_indicators.csv
 │   │   └─ pop_income.csv
 │   └─ processed/
 │       └─ final_merged_dataset.csv
 ├─ reports/
 │   ├─ model_summary.json
 │   ├─ ODA_Analysis(FINAL).pdf
 │   └─ figures/
 ├─ dashboards/
 │   └─ ODA_Analysis(final).pbix
 ├─ code/
 │   ├─ build_panel.ipynb
 │   └─ (future: 00_setup.R, 01_clean_merge.R, 02_model_plots.R)
 ├─ README.md
 └─ README_technical.md

📚 Data Sources
Source	Dataset	Description
OECD CRS (DAC2A)	Aid disbursements to Education and Health sectors	Constant USD values for comparability
World Bank WDI	Literacy, life expectancy, infant mortality, school enrollment	2000–2023 coverage
World Bank Population & Metadata	Population and income group	Used for ODA per capita and z-score normalization
🧮 Methodology Summary

Merge key: Country + Year

Filtered sectors: Education and Health

Indicators: Literacy, Life Expectancy, Infant Mortality, Enrollment

Derived variables:

ODA per Capita = total ODA ÷ population

Lag variables: Literacy (1–3 years)

Z-scores: standardization within income_group × year

|Z| ≥ 2 → “Notable” Mismatch

All processing steps were executed in Python (build_panel.ipynb) and are reproducible in R for full transparency.

📊 Dashboard Highlights

The Power BI dashboard visualizes ODA flows and outcomes across six countries:
Bangladesh, Ethiopia, India, Pakistan, Sudan, and South Sudan (2000–2023).

Key insights include:

Education ODA shows a positive association with literacy (r ≈ +0.65).

Health ODA is moderately correlated with life expectancy (r ≈ +0.58).

Some middle-income countries exhibit mismatches between ODA inflows and progress rates.

Interactive dashboard filters include:

Country, Sector, Year Range, Income Group

Lag and Z-score views (Normal vs. Notable mismatch)

🧾 Reproducibility

All workflows are transparent and reproducible.

Run build_panel.ipynb in Google Colab or Jupyter.

Output:

/data/processed/final_merged_dataset.csv

/reports/model_summary.json

Visualize results with Power BI using ODA_Analysis(final).pbix.

For academic reproducibility, R scripts (00_setup.R–02_model_plots.R) and a Quarto report will be added next.

For detailed steps, see README_technical.md
.

⚠️ Caveats

Data represent disbursements, not commitments.

Correlation ≠ causation.

CRS data availability varies by sector and country.

All values converted to constant USD for year-to-year comparability.

👤 Author

Hiroki Shima
Independent Researcher | Human Capital & Development Analytics
🔗 GitHub: HirokiShimaUnited

📘 Citation

Shima, Hiroki (2025). ODA–WDI Dashboard: Integrated analysis of Official Development Assistance and Human Development Indicators (2000–2023).
GitHub repository: https://github.com/HirokiShimaUnited/ODA
