# Rock Fracture by Ice Segregation

[![Degree](https://img.shields.io/badge/Degree-MSc_Data_Science-blue.svg)](https://www.sussex.ac.uk)
[![Institution](https://img.shields.io/badge/University-University_of_Sussex-red.svg)](https://www.sussex.ac.uk)
[![Model](https://img.shields.io/badge/Model-Walder--Hallet_(1985)-green.svg)](#numerical-modelling)

This repository contains the data cleaning pipelines, exploratory data analysis, and numerical model implementations for my MSc Data Science dissertation at the University of Sussex. The project evaluates the applicability of the **Walder–Hallet (1985)** rock fracture model to real-world experimental data collected during freeze-thaw cycles by **Murton et al. (unpublished)**, building upon previous numerical work by **Abram Haas (2021)**.

---

## 📌 Project Overview

Rock fracture due to **ice segregation** (the growth of ice lenses fed by water migration through cryosuction) is a primary driver of bedrock instability in cold regions and permafrost zones. As global temperatures shift due to climate change, understanding and predicting freeze-thaw bedrock degradation is crucial for infrastructure stability.

The goal of this research is to:
1. **Precondition & Clean** large-scale experimental time-series data from freeze-thaw laboratory experiments.
2. **Re-implement** the time-dependent Walder–Hallet mathematical framework in **Python** using differential equation solvers (`scipy.integrate.solve_ivp` with RK23 integration).
3. **Compare** numerical predictions of ice volume added ($V_S$), crack propagation ($c$), and aperture growth ($w$) against physical measurements (heave, settlement, and macro-crack observations).

---

## 🔬 Experimental Setup & Datasets

The experimental data comes from multi-cycle freeze-thaw tests conducted by J. B. Murton et al. on 5 rock types (sandstones and limestones) under two distinct freezing regimes:

| Regime | Description | Rock Types / Identifiers |
| :--- | :--- | :--- |
| **Unidirectional (U1–U5)** | Top-down freezing simulating seasonal frost in non-permafrost regions. | Sandstones (U1, U2), Limestones/Chalks (U3, U4, U5). |
| **Bidirectional (B1–B4)** | Permafrost setup with active layer thawing (ALT) cycles over a frozen base. | Sandstones (B1, B2), Limestones/Chalks (B3, B4). |

### Tracked Variables
- **Temperatures ($T_c$):** Platinium resistor (Pt100) thermistor time-series at multiple block depths.
- **Displacement / Heave:** LVDT sensors capturing vertical expansion/settlement.
- **Pressure & Water Content:** Transducers and capacitance probe measurements.
- **Macro-cracking:** Visual crack mapping and physical measurement across lateral increments during thaw periods.

---

## ⚙️ Methodology & Data Pipeline
Raw Excel Spreadsheets
└──> Manual Preconditioning (Excel)
└──> Automated Python Wrangling & Timestamp Alignment (Pandas)
└──> Exploratory Data Analysis & Correlation Heatmaps
└──> Numerical Model (SciPy ODE Integration: RK23 / Euler)

1. **Data Preconditioning:**
   - Separated nested tables and multi-block sheets into discrete CSVs.
   - Standardized timestamps across sensor arrays using a $\pm 30$-minute alignment tolerance.
   - Handled missing values (NaNs $< 0.4\%$) and removed misaligned timestamps ($< 3\%$).

2. **Numerical Modelling:**
   - Modeled stress intensity factor ($K_I$), ice pressure ($p_i$), crack extension rate ($\frac{dc}{dt}$), and aperture growth ($\frac{dw}{dt}$).
   - Calculated volume rate of ice added per unit area ($V_S$) using the Clausius-Clapeyron relation and flow resistance ($R_f$) formulations.

---

## 📊 Key Findings

- **Unidirectional Match:** The model successfully predicts the location and timing of peak macro-cracking under unidirectional (top-down) freezing conditions, matching physical observations in blocks like **U3** (Totternhoe Clunch) and **U5** (Tuffeau).
- **Bidirectional Limitation:** Predictions under bidirectional permafrost conditions struggled to match physical observations, primarily due to non-horizontal/interacting fractures breaking single-crack model assumptions, and numerical instabilities in calculated flow resistance ($R_f$) near $0^\circ\text{C}$.
- **Data Correlations:** Thermal time-series EDA showed a distinct drop in temperature correlation across depths corresponding to areas of high fracture density, reflecting altered thermal conductivity caused by ice lens formation.

---

## 🛠️ Repository Structure
.
├── data/
│   ├── raw/                 # Original Excel data sheets (Chambre B, Caisson)
│   └── processed/           # Standardized per-block CSV time-series
├── src/
│   ├── data_cleaning.py     # Pandas wrangling and timestamp alignment
│   ├── eda_plots.py         # Histograms, time-series plots, correlation matrices
│   └── walder_hallet_model.py # ODE integration (solve_ivp) for crack growth
├── notebooks/
│   └── eda_and_modeling.ipynb
├── README.md
└── requirements.txt

---

## 📦 Dependencies

- Python 3.9+
- `pandas`
- `numpy`
- `scipy`
- `matplotlib` / `seaborn`

---

## 📑 References & Acknowledgements

- **Walder, J., & Hallet, B. (1985).** *A theoretical model of the fracture of rock during freezing.* Geological Society of America Bulletin.
- **Murton, J. B., Peterson, R., & Ozouf, J.-C. (Unpublished).** *Physical and numerical modelling of rock fracture by ice segregation in limestones and sandstones.*
- **Haas, A. (2021).** *Applicability of the Walder-Hallet frost fracture model to laboratory cyclic uni- and bi-direction freeze-thaw of limestone and sandstone.* MSc Thesis, University of Alaska Fairbanks.
---

## ⚙️ Methodology & Data Pipeline
