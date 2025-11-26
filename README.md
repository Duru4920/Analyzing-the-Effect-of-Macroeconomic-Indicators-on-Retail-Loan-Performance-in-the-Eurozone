# Analyzing-the-Effect-of-Macroeconomic-Indicators-on-Retail-Loan-Performance-in-the-Eurozone
# 📊 Macroeconomic Determinants of NPL Ratios in the Eurozone (Quarterly Panel)

This repository contains the code, data workflow, and econometric analysis behind my project:

> **“Analyzing the Effect of Macroeconomic Indicators on Retail Loan Performance in the Eurozone”**

The goal is to understand how **macroeconomic variables** such as unemployment, GDP growth, inflation, and short-term interest rates affect **Non-Performing Loan (NPL) ratios** across Eurozone countries using a **quarterly panel**.

---

## 🌍 Project Overview

- **Region:** Eurozone (19 countries)  
- **Frequency:** Quarterly  
- **Period (effective NPL coverage):** ~2006–2024 (unbalanced panel)  
- **Dependent variable:** NPL ratio (% of total loans)  
- **Main regressors:**
  - GDP growth (%)
  - Unemployment rate (%)
  - Inflation YoY (%)
  - Euribor 3M (%)

---

## 📂 Data Sources

- **NPL ratios:** World Bank – Global Financial Development Database  
- **Macro variables (GDP, Unemployment, HICP):** ECB / Eurostat  
- **Money market rates:** Euribor 3M – ECB

All raw data were merged and transformed into a single panel dataset:

- `macro_panel_updated.csv` – original merged panel  
- `macro_clean_final.csv` – cleaned panel used for econometric analysis

> 🔎 The cleaning process:
> - Standardized country codes and time indices  
> - Constructed a quarterly `time_id = year * 4 + quarter`  
> - Dropped invalid / missing NPL rows  
> - Fixed the inflation measure by using the correct `inflation_yoy` column and removing a corrupted `hicp_yoy` column.

---

## 🧮 Methods & Econometric Strategy

The analysis is performed in **Python (Colab)** using `pandas`, `statsmodels`, and `linearmodels`.

### 1. Descriptive Statistics

- Summary statistics for:
  - NPL ratio
  - GDP growth
  - Unemployment
  - Inflation YoY
  - Euribor 3M  
- Pearson correlation matrix to check basic relationships and multicollinearity.

### 2. Panel Regression Models

Baseline specification:

\[
NPL_{it} = \beta_1 GDP_{it} +
           \beta_2 Unemployment_{it} +
           \beta_3 Inflation_{it} +
           \beta_4 Euribor3M_{it} + \alpha_i + \varepsilon_{it}
\]

Where:
- \( \alpha_i \) = **country fixed effects** (time-invariant heterogeneity)
- \( i \) = country, \( t \) = quarter

Models estimated:

- **Fixed Effects (FE)** – country FE, clustered SEs by country  
- **Random Effects (RE)** – with constant  
- **Hausman test** – to compare FE vs RE

---

## 💡 Key Findings (Summary)

- **Unemployment** is the most important driver of NPL ratios.  
  - A 1 percentage point rise in unemployment is associated with roughly **+1.3 pp higher NPL ratio** in the FE model.
- **GDP growth** has the expected negative sign but is not always statistically significant once FE are included.
- **Inflation YoY** has a weak and mostly insignificant effect in the FE specification.
- **Euribor 3M** appears with a **negative coefficient**, consistent with the idea that policy rates tend to be higher in good times when NPLs are lower (credit cycle effect), rather than a direct causal impact.
- An F-test strongly rejects a pooled OLS model in favour of a panel model with country effects.

---

## 🧾 Repository Structure 

```bash
.
├── data/
│   ├── macro_panel_updated.csv      # Original merged panel
│   └── macro_clean_final.csv        # Cleaned analysis-ready panel
├── notebooks/
│   └── npl_eurozone_analysis.ipynb  # Main Colab / Jupyter notebook
├── src/
│   └── panel_analysis.py            # (Optional) Python script version of key analysis steps
├── latex/
│   ├── regression_table.tex         # LaTeX table of FE/RE results
│   └── descriptive_stats.tex        # LaTeX descriptive statistics
└── README.md
