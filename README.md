# World Energy EDA: Economic Development and Environmental Cost (2000–2024)

**Course:** Data Science & Analytics  
**Dataset:** Our World in Data — World Energy Consumption  
**Notebook:** `WorldEnergy_EDA_Final.ipynb`

---

## Research Problem

Rapid economic growth in the 21st century has been historically accompanied by rising energy demand and greenhouse gas emissions. It remains unclear whether wealthier nations are decoupling economic growth from environmental impact, or whether higher GDP continues to drive proportionally higher emissions. Understanding this relationship is critical for informing climate policy and sustainable development targets.

This study examines the relationship between **GDP, primary energy consumption, and greenhouse gas emissions** across 220 countries from **2000 to 2024**, with a focus on contrasting the behaviour of the world's highest and lowest GDP economies.

---

## Research Questions

**RQ1:** Is there a statistically significant positive relationship between a country's GDP and its primary energy consumption and greenhouse gas emissions over the period 2000–2024?

**RQ2:** Do the top 10 highest-GDP countries show a different emissions-to-energy trend compared to the bottom 10 lowest-GDP countries over the same period?

---

## Research Objectives

1. To perform systematic data quality assessment on the World Energy dataset and justify the selection of the 2000–2024 scope based on data completeness evidence.
2. To identify the strongest correlated variable triplet (GDP, energy, emissions) as the analytical scope through exploratory correlation analysis.
3. To clean, impute, and validate the scoped dataset before analysis.
4. To analyse and visualise the distribution, trend, and relationship of GDP, energy consumption, and greenhouse gas emissions across top and bottom GDP country groups.

---

## Dataset

| Property | Value |
|---|---|
| Source | [Our World in Data — Energy](https://ourworldindata.org/energy) |
| File | `WorldEnergy.csv` |
| Raw size | 23,195 rows × 130 columns |
| Coverage | 314 entities (countries + regional aggregates), 1900–2024 |
| After cleaning | 4,070 rows × 6 columns, 165 countries, 2000–2024 |

---

## Requirements

```
pandas
numpy
matplotlib
seaborn
```

Install all dependencies with:

```bash
pip install pandas numpy matplotlib seaborn
```

> **Note:** `plotly` is listed in some reference notebooks but is **not required** for this notebook. All charts use `matplotlib` and `seaborn` for reproducibility across environments.

---

## File Structure

```
project/
│
├── WorldEnergy.csv              ← Raw dataset (place in same folder as notebook)
├── WorldEnergy_EDA_Final.ipynb  ← Main analysis notebook
└── README.md                    ← This file
```

---

## How to Run

1. Place `WorldEnergy.csv` in the same folder as the notebook.
2. Open `WorldEnergy_EDA_Final.ipynb` in Jupyter Notebook or JupyterLab.
3. Update `CSV_PATH` in **Step 1** if your file is stored elsewhere:
   ```python
   CSV_PATH = r'WorldEnergy.csv'  # ← change this if needed
   ```
4. Run all cells top to bottom: **Kernel → Restart & Run All**.

---

## Notebook Structure

The notebook follows a linear EDA workflow where each step feeds directly into the next. Every step includes a **Rationale** section explaining why it is done and what decisions were made.

| Step | Title | Description |
|---|---|---|
| 1 | Import Libraries & Load Data | Load all dependencies and read the raw CSV |
| 2 | Raw Data Quality Checks | Duplicate detection, year range validation, global missingness audit |
| 3 | Classify Entities | Separate real countries (with ISO code) from regional aggregates |
| 4 | Time Period Scope — Justification | Evidence-based selection of 2000–2024 using pre- vs post-2000 missing rate comparison |
| 5 | Variable Selection via Correlation | Pearson correlation matrix to identify the GDP / energy / emissions trio as the analytical scope |
| 6 | Data Cleaning & Imputation | Sort → interpolate → fix zeros → re-interpolate → drop, in the correct sequence |
| 7 | Post-Cleaning Validation | Null check, zero/negative check, plausibility stats, country coverage histogram |
| 8 | Univariate Analysis | Distribution histograms, log-transformed distributions, decade boxplots |
| 9 | Bivariate & Relationship Analysis | Scatter plots with trend lines (log-log scale), three-way bubble chart |
| 10 | Group Comparison | Top 10 vs Bottom 10 GDP countries — boxplots, time-series trends, emissions intensity |
| 11 | Summary & Research Findings | Auto-generated findings tied back to RQ1 and RQ2, limitations, next steps |

---

## Scope Selection Rationale

### Why 2000–2024?

The time period was selected based on data completeness evidence, not arbitrary choice:

| Variable | Pre-2000 Missing | Post-2000 Missing |
|---|---|---|
| `greenhouse_gas_emissions` | 100.0% | 4.4% |
| `primary_energy_consumption` | 57.6% | 1.8% |
| `gdp` | 49.3% | 29.6% |

Analysing pre-2000 data would require imputing majority-missing columns, introducing more noise than signal. The 2000–2024 window is the largest range with acceptable completeness for all three core variables.

### Why these three variables?

Variable selection was driven by the correlation matrix — not manual choice. GDP, primary energy consumption, and greenhouse gas emissions form the most tightly correlated cluster in the dataset (all pairwise Pearson r > 0.94), directly addressing the research questions.

---

## Key Findings

**RQ1:** All three pairwise correlations exceed r = 0.94 on a log-log scale, confirming a strong positive relationship between GDP, energy consumption, and emissions at the global level across 2000–2024. No generalised decoupling is observed across the full country sample.

**RQ2:** Top 10 GDP countries emit orders-of-magnitude more in absolute terms, but their **emissions intensity** (MtCO₂e per billion USD of GDP) shows a declining trend post-2010 — evidence of early-stage decoupling among high-income economies. Bottom 10 GDP countries show lower absolute emissions but flat or rising intensity, reflecting constrained capacity for clean energy transition.

---

## Known Limitations

1. Correlation ≠ causation — GDP growth may not directly cause emissions; both may be driven by population and industrialisation stage.
2. Linear interpolation was used to fill small gaps — results for countries with sparse data (e.g. small island nations) should be interpreted cautiously.
3. Analysis is scoped to three variables — energy mix (renewables share) was excluded but would enrich RQ2.

---

## Suggested Next Steps

| Step | Method |
|---|---|
| Quantify GDP–emissions elasticity | Panel OLS regression with country fixed effects |
| Test for decoupling more rigorously | Environmental Kuznets Curve (EKC) analysis |
| Add energy mix as moderating variable | Include `renewables_share_energy` in regression |
| Forecast future emissions | Time-series model (ARIMA / Prophet) on country-level data |
