📊 ODA–WDI Dashboard

Integrated Analysis of Official Development Assistance (ODA) and Human Development Outcomes (2000–2023)

📘 Overview

This repository presents a reproducible analysis of how education and health sector ODA relate to key human development indicators such as literacy, life expectancy, and infant mortality.
The study integrates datasets from the OECD Creditor Reporting System (CRS) and the World Bank World Development Indicators (WDI) to examine aid effectiveness and cross-sectoral connections in human development.

📦 The Project Includes

Cleaned and merged datasets: final_merged_dataset.csv

Power BI dashboard: ODA_Analysis(final).pbix

Analytical summary: model_summary.json

Full technical documentation: README_technical.md

📂 Repository Structure
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
 ├─ dashboards/
 │   └─ ODA_Analysis(final).pbix
 ├─ code/
 │   └─ build_panel.ipynb
 ├─ README.md
 └─ README_technical.md

📚 Data Sources
Source	Dataset	Description
OECD CRS (DAC2A)	ODA disbursements to Education & Health sectors	Constant USD values for comparability
World Bank WDI	Literacy, life expectancy, infant mortality, school enrollment	2000–2023 coverage
World Bank Metadata	Population & Income Group	Used for ODA per capita and z-score normalization
🧮 Methodology Summary

Merge key: Country + Year

Filtered sectors: Education and Health

Indicators: Literacy, Life Expectancy, Infant Mortality, Enrollment

Derived variables:

ODA per Capita = total ODA ÷ population

Lag variables (1–3 years)

Z-score standardization: within income_group × year

Mismatch flag: |z| ≥ 2 → “Notable”

All processing steps are reproducible using the Python notebook (build_panel.ipynb).
A fully equivalent R/Quarto workflow will be added to ensure academic reproducibility.

📈 Dashboard Highlights

The Power BI dashboard visualizes ODA and outcomes for six countries:
Bangladesh, Ethiopia, India, Pakistan, Sudan, and South Sudan (2000–2023)

🔍 Key Insights

Education ODA shows a positive correlation with literacy (r ≈ +0.65).

Health ODA correlates moderately with life expectancy (r ≈ +0.58).

Some middle-income countries exhibit mismatches between ODA inflows and progress rates.

🧭 Dashboard Filters

Country / Sector / Year Range / Income Group

Lag Selection (0–3 years)

Z-score category (Normal / Notable)

🧾 Reproducibility Workflow

Open build_panel.ipynb in Google Colab or Jupyter.

Run all cells sequentially:

Input: /data/raw/*.csv

Output: /data/processed/final_merged_dataset.csv

Visualize results using Power BI (ODA_Analysis(final).pbix)

Review methodology in README_technical.md

⚠️ Caveats

Data represent disbursements, not commitments.

Correlation ≠ causation.

CRS data completeness varies by sector and country.

All values converted to constant USD for cross-year comparability.

👤 Author

Hiroki Shima
Independent Researcher | Human Capital & Development Analytics
🔗 GitHub: HirokiShimaUnited

📘 Citation

Shima, Hiroki (2025). ODA–WDI Dashboard: Integrated Analysis of Official Development Assistance and Human Development Indicators (2000–2023).
GitHub repository: https://github.com/HirokiShimaUnited/ODA_

💡 Next Step (Planned)

Port workflow to R (tidyverse + Quarto) for open academic reproducibility

Publish analytical version on GitHub Pages / Quarto site
