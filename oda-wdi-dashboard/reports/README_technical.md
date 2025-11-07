
# ODA ↔ WDI Dashboard – Technical Notes

## Data Sources
- OECD CRS: Education & Health disbursements (constant USD)
- World Bank WDI:
  - SE.ADT.LITR.ZS – Adult literacy
  - SE.SEC.ENRR – Secondary enrollment
  - SP.DYN.IMRT.IN – Infant mortality
  - SP.DYN.LE00.IN – Life expectancy
- WDI metadata: Population & income group

## Reproducibility
Run `build_panel.ipynb` or the single Colab Python cell.  
Outputs will be in:
- `data/processed/final_merged_dataset.csv`
- `reports/model_summary.json`
- `reports/figures/oda_total_mln.png`

## Indicators & Logic
- ODA filtered to sectors **Education** + **Health**
- Per-capita ODA = total ODA ÷ population
- Lags: Literacy (1–3)
- Z-scores = within income_group × year
- |Z| ≥ 2 ⇒ “Notable” Mismatch
- Latest metrics copied per country

## Caveats
Association ≠ causation.  
CRS data = disbursements, not commitments.  
Constant USD ensures comparability across years.
