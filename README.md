# ⚕️ Health Analysis of Central Asian Countries

A panel data study assessing the healthcare systems of Kazakhstan, Kyrgyzstan, Tajikistan, Turkmenistan, and Uzbekistan over the period **2000–2021**.

---

## 📌 Project Overview

This project evaluates macro-level health indicators across Central Asian countries using data from the **WHO Global Health Observatory** and the **World Bank Open Data** platform. The analysis covers 110 observations (22 per country) and applies **Fixed Effects Panel OLS regression** to test four key hypotheses about the determinants of life expectancy and long-term mortality trends.

---

## 🗂️ Table of Contents

- [Data Sources](#-data-sources)
- [Variables](#-variables)
- [Methodology](#-methodology)
- [Hypotheses & Results](#-hypotheses--results)
- [Key Findings](#-key-findings)
- [Project Structure](#-project-structure)
- [Requirements](#-requirements)
- [How to Run](#-how-to-run)

---

## 📦 Data Sources

| Source | Portal |
|--------|--------|
| World Health Organization (WHO) | [Global Health Observatory](https://www.who.int/data/gho/data/indicators) |
| World Bank | [World Bank Open Data](https://data.worldbank.org/indicator) |

**Coverage**: 5 countries × 22 years (2000–2021) = 110 observations

---

## 📊 Variables

### WHO Indicators
| Variable | Description |
|----------|-------------|
| `Life_Expectancy` | Life expectancy at birth (years) |
| `Health_Expenditure` | Current health expenditure per capita (USD) |
| `Health_Expenditure_GDP` | Health expenditure as % of GDP |
| `Doctors` | Medical doctors per 10,000 population |
| `Hospital_Beds_per10k` | Hospital beds per 10,000 population |
| `DTP3_Coverage` | DTP3 immunization coverage among 1-year-olds (%) |
| `Infant_Mortality` | Infant mortality rate (per 1,000 live births) |
| `Under5_Mortality` | Under-five mortality rate (per 1,000 live births) |
| `Maternal_Mortality` | Maternal mortality ratio (per 100,000 live births) |
| `NCD_Mortality` | Age-standardized NCD mortality rate (per 100,000) |
| `Obesity_Prevalence` | Obesity prevalence among adults, BMI ≥ 30 (%) |

### World Bank Indicators
| Variable | Description |
|----------|-------------|
| `GDP_per_Capita` | GDP per capita (current USD) |
| `GDP_Growth` | Annual GDP growth (%) |

---

## 🔬 Methodology

- **Model**: Fixed Effects Panel OLS (`PanelOLS` from `linearmodels`)
- **Effects**: Entity (country) + Time (year) fixed effects
- **Standard errors**: Clustered (robust to heteroskedasticity and autocorrelation)
- **Multicollinearity check**: Variance Inflation Factor (VIF)
- **Missing data**: Linear interpolation for Uzbekistan (Doctors); point imputation for Kazakhstan (Hospital Beds, sourced from the Bureau of National Statistics)

---

## 🧪 Hypotheses & Results

### H1 — Health Expenditure (% GDP) → Life Expectancy
> *Does higher health spending as a share of GDP improve life expectancy?*

- **R² (Within)**: 0.94 | **F-stat**: 42.77 (p < 0.001)
- **Result**: ❌ No statistically significant direct effect of health expenditure on life expectancy.  
  Mortality indicators (NCD and Infant Mortality) emerged as the dominant predictors.

---

### H2 — Obesity Prevalence → Life Expectancy
> *Is the strong positive correlation between obesity and life expectancy robust after controlling for fixed effects?*

- **F-stat**: 4.86 (p = 0.0015)
- **Result**: ✅ Positive relationship is statistically significant — consistent with the **"wealth-health-obesity paradox"**: countries with higher life expectancy also tend to show higher obesity rates, reflecting rising living standards.

---

### H3 — Medical Doctors per 10k → Life Expectancy
> *Does doctor availability consistently improve life expectancy?*

- **R² (Within)**: 0.85 | **F-stat**: 34.39 (p < 0.001)
- **Result**: ❌ Number of doctors does not show a statistically significant effect on life expectancy after controlling for other factors. Distribution and system efficiency may be confounding factors.

---

### H4 — Long-Term Mortality Trends (2000–2021)
> *Have mortality rates consistently declined over time across Central Asia?*

| Indicator | Year Coeff. | R² (Within) | Key Driver |
|-----------|-------------|-------------|------------|
| Infant Mortality | −0.86*** | 0.91 | GDP per capita |
| Under-5 Mortality | −1.02*** | 0.90 | GDP per capita |
| Maternal Mortality | −0.87*** | 0.75 | GDP per capita |
| NCD Mortality | −22.67*** | 0.83 | Time trend |

- **Result**: ✅ All four mortality indicators show **consistent and significant declines** over the two-decade period. Economic development is the primary driver for child and maternal mortality.

---

## 💡 Key Findings

- **Kazakhstan** leads the region in health infrastructure, doctor availability, and lowest child/maternal mortality.
- **Uzbekistan** excels in vaccination coverage but has the highest NCD mortality burden.
- **Tajikistan and Turkmenistan** are top performers in GDP growth, which strongly correlates with health improvements.
- **Economic development (GDP per capita)** is a more robust predictor of health outcomes than health spending levels.
- **Health expenditure as % of GDP** shows mixed and sometimes counter-intuitive effects — possibly reflecting structural inefficiencies or reverse causality.

---

## 📁 Project Structure

```
health-analysis-central-asia/
│
├── Health_Analysis.ipynb       # Main analysis notebook
├── CA_Health_data_clean.xlsx   # Cleaned and merged dataset (output)
│
├── data/                       # Raw WHO CSV files
│   ├── life_expectancy.csv
│   ├── infant_mortality.csv
│   ├── health_expenditure.csv
│   ├── health_expenditure_GDP.csv
│   ├── doctors.csv
│   ├── obesity.csv
│   ├── dtp3_coverage.csv
│   ├── under5.csv
│   ├── maternal_mortality.csv
│   ├── hospital_beds_per10k.csv
│   ├── smoking_prevalence.csv
│   └── NCD_mortality.csv
│
└── README.md
```

---

## ⚙️ Requirements

```
pandas
numpy
matplotlib
seaborn
scipy
linearmodels
statsmodels
pandas_datareader
openpyxl
```

Install all dependencies:
```bash
pip install pandas numpy matplotlib seaborn scipy linearmodels statsmodels pandas_datareader openpyxl
```

---

## ▶️ How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/YOUR_USERNAME/health-analysis-central-asia.git
   cd health-analysis-central-asia
   ```

2. Place raw WHO CSV files in the `data/` folder (or adjust file paths in the notebook).

3. Launch Jupyter Notebook:
   ```bash
   jupyter notebook Health_Analysis.ipynb
   ```

4. Run all cells sequentially. World Bank data is fetched automatically via `pandas_datareader`.

---

## 📝 Notes

- Turkmenistan's data has limited external verification due to restricted data access.
- Kazakhstan's 2021 hospital bed value was manually sourced from the [Bureau of National Statistics of Kazakhstan](https://stat.gov.kz).
- Models with negative R² (Hypothesis 2) are noted — F-statistic and coefficient significance are the preferred evaluation metrics in such cases.

---

*Data Science course project | Data Analytics module*
