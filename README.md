# Supply Chain Risk & Insurance Premiums

An empirical research project that quantifies the relationship between global supply chain disruptions and insurance industry premiums, with a focus on manufacturing-exposed lines. This was developed as the final project for **IEOR 4572** at Columbia University.

---

## Overview

This project investigates whether supply chain stress indices can explain and predict movements in property & casualty (P&C) insurance premiums. Using a combination of regression analysis, event studies, and vector autoregression (VAR), the project demonstrates that supply chain pressure has statistically significant effects on insurance underwriting, particularly for manufacturing-exposed segments.

### Core Research Questions

1. Do higher supply chain stress levels correlate with higher insurance premiums?
2. Are manufacturing-exposed insurance lines (SMEDEX) more sensitive than the aggregate market (STOT)?
3. What is the dynamic causal path — do supply chain shocks lead insurance premium growth over time?
4. Are there heterogeneous effects across specific disruption events (COVID-19, Trade War, Suez blockage, Russia-Ukraine War)?

---

## Repository Structure

```
SupplyChainRisk/
├── Data/                          # Raw input data
│   ├── gscpi_data.xls/xlsx        # NY Fed Global Supply Chain Pressure Index
│   ├── Stress index tables.xlsx   # World Bank Global Supply Chain Stress Index
│   ├── swissre_insurance.csv      # Swiss Re insurance premiums & loss data
│   ├── MSCI_Auto.xls              # MSCI Automobile sector index
│   ├── MSCI_Materials.xls         # MSCI Materials sector index
│   ├── BUSINV.csv                 # FRED Business Inventories
│   ├── DGORDER.csv                # FRED Durable Goods Orders
│   ├── IPMAN.csv                  # FRED Industrial Production: Manufacturing
│   └── PPIACO.csv                 # FRED Producer Price Index
├── Plots/                         # Generated visualizations
│   ├── GSCPI.png                  # Global Supply Chain Pressure Index time series
│   ├── GSCSI.png                  # World Bank Stress Index time series
│   ├── gscpi_growth.png           # GSCPI growth rates
│   ├── STOT.png                   # Total non-life premium analysis
│   ├── SMEDEX.png                 # Manufacturing-exposed premium analysis
│   ├── corr.png                   # Correlation matrix
│   ├── premium_abnormal_stot.png  # Abnormal premium growth (STOT) around events
│   └── premium_abnormal_smedex.png# Abnormal premium growth (SMEDEX) around events
├── notebook1.ipynb                # Phase 1: Data Exploration
├── notebook2.ipynb                # Phase 2: Regression Analysis
├── notebook3.ipynb                # Phase 3: Event Study
├── notebook4.ipynb                # Phase 4: VAR & Impulse Response Analysis
└── IEOR4572_FinalProjectReport.pdf# Full research report
```

---

## Data Sources

| Variable | Source | Frequency | Description |
|----------|--------|-----------|-------------|
| GSCPI | [NY Fed](https://www.newyorkfed.org/research/policy/gscpi) | Monthly | Global Supply Chain Pressure Index |
| GSCSI | World Bank | Monthly | Global Supply Chain Stress Index |
| STOT | Swiss Re | Annual | Total non-life insurance real premium growth (US) |
| SMEDEX | Swiss Re | Annual | Manufacturing-exposed commercial insurance premiums |
| MSCI_Auto | MSCI | Monthly | Automobile sector equity index |
| MSCI_Materials | MSCI | Monthly | Materials sector equity index |
| IPMAN | [FRED](https://fred.stlouisfed.org/) | Monthly | Industrial Production: Manufacturing |
| PPIACO | FRED | Monthly | Producer Price Index (All Commodities) |
| DGORDER | FRED | Monthly | Durable Goods Orders |
| BUSINV | FRED | Monthly | Business Inventories |

**Coverage:** Insurance data spans 1991–2035 (actuals + forecasts) across 40+ countries; GSCPI available from 1998 onwards.

---

## Methodology

### Phase 1 — Data Exploration (`notebook1.ipynb`)
- Load and clean all data sources
- Standardize date formats and align time series
- Merge stress indices and insurance data using approximate joins (30-day tolerance)
- Profile Swiss Re data by country, business line, and year

### Phase 2 — Regression Analysis (`notebook2.ipynb`)
- Annualize monthly indicators (mean and max)
- Merge stress indices, macro indicators, sector indices, and insurance data at annual frequency
- Estimate multivariate OLS models:

  ```
  PremiumRealGrowth(t) = α + β₁·GSCPI(t-1) + β₂·GSCSI(t-1)
                           + β₃·IPMAN_YoY(t) + β₄·PPI_YoY(t) + ε(t)
  ```

- Compute 12-month rolling correlations between stress indices and insurance metrics

### Phase 3 — Event Study (`notebook3.ipynb`)
- Annotate time series with major supply chain disruptions:
  - US-China Trade War (2018)
  - COVID-19 Pandemic (2020)
  - Suez Canal Blockage (2021)
  - Russia-Ukraine War (2022)
- Compute abnormal premium growth deviations around each event window
- Difference-in-differences analysis: **Treated = SMEDEX** (manufacturing-exposed) vs **Control = STOT** (aggregate)
- Rolling correlation of GSCPI with MSCI sector returns

### Phase 4 — VAR & Impulse Response Analysis (`notebook4.ipynb`)
- Estimate a bivariate Vector Autoregression (VAR):

  ```
  GSCPI(t)   = α₁ + Σ β₁ᵢ GSCPI(t-i)   + Σ γ₁ᵢ Premium(t-i) + ε₁(t)
  Premium(t) = α₂ + Σ β₂ᵢ GSCPI(t-i)   + Σ γ₂ᵢ Premium(t-i) + ε₂(t)
  ```

- Generate orthogonalized impulse response functions (IRF)
- Conduct Granger causality testing
- Perform forecast error variance decomposition

---

## Key Findings

- Supply chain stress indices have **statistically significant predictive power** for P&C insurance premium growth.
- **Manufacturing-exposed lines (SMEDEX) show greater sensitivity** to supply chain shocks than the aggregate market (STOT).
- Major disruption events cause measurable abnormal premium adjustments, especially for SMEDEX.
- The VAR impulse responses confirm that supply chain shocks propagate to insurance premiums over several quarters.

---

## Requirements

The notebooks use standard Python scientific computing and finance libraries:

```
pandas
numpy
matplotlib
seaborn
statsmodels
scipy
openpyxl
xlrd
```

Install with:

```bash
pip install pandas numpy matplotlib seaborn statsmodels scipy openpyxl xlrd
```

---

## Usage

Run the notebooks in order:

```bash
jupyter notebook notebook1.ipynb   # Data exploration
jupyter notebook notebook2.ipynb   # Regression analysis
jupyter notebook notebook3.ipynb   # Event study
jupyter notebook notebook4.ipynb   # VAR / impulse response
```

---

## Report

The full write-up, including literature review, detailed results, and policy implications, is available in [`IEOR4572_FinalProjectReport.pdf`](IEOR4572_FinalProjectReport.pdf).

---

## Course

**IEOR 4572** — Columbia University
