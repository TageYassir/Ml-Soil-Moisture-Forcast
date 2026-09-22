# 🌱 Regional Hybrid Stacking for One-Day Soil Moisture Forecasting

> **Regional Hybrid Stacking with XGBoost and Deep Attention for One-Day Soil Moisture Forecasting**

A machine learning research project investigating **one-day-ahead soil moisture forecasting** using regional station clustering and hybrid machine learning architectures.

The study focuses on reducing spatial heterogeneity in large-scale environmental datasets and evaluating whether regional modeling can improve short-term soil moisture prediction.

---

# 👨‍💻 Author

|                                                            |                                                                                                                                                                                                              |
| :--------------------------------------------------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <img src="https://github.com/TageYassir.png" width="120"/> | **Yassir Tagemouati** <br><br> Big Data Engineering Student (Cycle d’Ingénieur) <br> Azure Data Engineering • Data Warehousing • Analytics <br><br> **GitHub:** [@TageYassir](https://github.com/TageYassir) |

---

## 📌 Research Overview

Soil moisture is an important variable in the land-atmosphere system and plays a major role in applications such as:

* 🌾 Precision agriculture
* 💧 Irrigation planning
* 🌵 Agricultural drought monitoring
* 🌱 Crop productivity management
* 🌦️ Environmental forecasting

This research investigates the prediction of **Volumetric Water Content (VWC) at 5 cm depth** for the following day using historical meteorological and soil measurements.

The main research hypothesis is that **grouping stations with similar hydro-meteorological behavior can reduce spatial noise and improve forecasting performance**.

The forecasting task uses a **10-day historical window to predict the following 24-hour soil moisture state**.

---

# 🎯 Research Objectives

The study focuses on three main objectives:

1. Investigate the limitations of global machine learning models on heterogeneous geographical datasets.
2. Identify groups of weather stations with statistically similar hydro-meteorological behavior.
3. Evaluate hybrid machine learning architectures for one-day-ahead soil moisture forecasting.

The experimental study compares several standalone and hybrid approaches, with particular attention to the combination of **XGBoost and deep sequential models**.

---

# 🌎 Dataset

The primary data source is the **U.S. Climate Reference Network (USCRN)** provided by NOAA.

An automated Python-based scraping process was developed to collect high-resolution daily meteorological and soil-state observations from stations across the United States.

### Main variables

The dataset includes measurements related to:

* Mean temperature
* Solar radiation
* Precipitation
* Relative humidity
* Soil moisture
* Soil temperature
* Soil moisture at multiple depths

The main prediction target is:

```text
SOIL MOISTURE — 5 cm
```

---

# 🧹 Data Preprocessing & Feature Engineering

The raw dataset required several preprocessing stages before modeling.

### Missing Values

The value `-9999` was used as a missing-data placeholder and was removed during preprocessing.

### Data Standardization

The original space-separated data format was converted into CSV format, with a state identifier added to support regional analysis.

### Feature Reduction

The original dataset contained:

```text
23 variables
```

The feature space was reduced to:

```text
12 variables
```

using correlation analysis and feature-selection considerations.

### Correlation Analysis

The analysis identified substantial relationships between several meteorological and soil variables.

For example, mean temperature showed a correlation greater than **90%** with daily maximum and minimum temperature, leading to the removal of those redundant variables.

The analysis also identified strong vertical coupling between soil moisture measurements at different depths. Correlations between adjacent soil layers exceeded **0.95**, with significant relationships extending toward the 100 cm layer.

This motivated the use of **5 cm soil moisture as the primary forecasting target**.

---

# 🗺️ Regional Homogeneous Modeling

A central component of the research is the transition from a broad **global modeling approach** toward **regional homogeneous modeling**.

Station-level soil moisture distributions were analyzed using their mean and variability to identify stations exhibiting statistically similar behavior.

Five stations were selected for the regional experiment:

| WBANNO |
| ------ |
| 53877  |
| 23904  |
| 63857  |
| 94996  |
| 53182  |

The resulting regional dataset contained approximately:

```text
22,000 observations
```

This selection was designed to provide a more consistent hydro-climatic signal and reduce the spatial noise present in the broader dataset.

---

# ⏱️ Forecasting Framework

The forecasting problem is defined as:

```text
Historical Data
     │
     ▼
Previous 10 Days
     │
     ▼
Feature Engineering
     │
     ▼
Regional Modeling
     │
     ▼
One-Day-Ahead Prediction
     │
     ▼
5 cm Soil Moisture
```

### Forecast configuration

| Parameter         | Configuration        |
| ----------------- | -------------------- |
| Historical window | 10 days              |
| Forecast horizon  | 24 hours             |
| Target            | VWC at 5 cm          |
| Regional stations | 5                    |
| Regional dataset  | ≈22,000 observations |
| Original features | 23                   |
| Selected features | 12                   |

---

# 🧠 Experimental Design

Six machine learning architectures were evaluated:

* **LSTM**
* **XGBoost**
* **Transformer**
* **XGB + CNN**
* **XGB + Transformer**
* **XGB + GRU**

The experiments were designed to compare standalone deep-learning and tree-based approaches against hybrid architectures that combine **tabular lagged features with sequential representations**.

For the XGBoost-based experiments, lagged variables were created using:

```text
1-day lag
3-day lag
7-day lag
10-day lag
```

resulting in **48 lagged meteorological and soil features**.

---

# 📊 Experimental Results

The reported results for the regional forecasting experiment are:

| Model             |         R² |        MAE |       RMSE |
| ----------------- | ---------: | ---------: | ---------: |
| LSTM              |     0.7904 |     0.0268 |     0.0045 |
| XGBoost           |     0.9432 |     0.0099 |     0.0261 |
| Transformer       |     0.7138 |     0.0317 |     0.0082 |
| **XGB + CNN**     | **0.9491** | **0.0099** | **0.0084** |
| XGB + Transformer |     0.9475 |     0.0105 |     0.0080 |
| XGB + GRU         |     0.9473 |     0.0105 |     0.0069 |

The table reproduces the performance metrics reported in the research paper.

---

# 📈 Key Experimental Findings

### Regional Modeling

The initial persistence/global experiment produced a reported performance level of approximately:

```text
R² = 0.11
```

After transitioning to the regional homogeneous modeling strategy, the evaluated advanced architectures achieved R² values above **0.94**.

### Hybrid Architectures

The reported experiments show that the hybrid approaches achieved R² values in the range of approximately:

```text
0.9473 — 0.9491
```

while the standalone deep-learning architectures produced lower R² values in the reported experiment.

### Feature Importance

The XGBoost feature-importance analysis identified **previous-day soil moisture** as an important driver of the next-day prediction.

Mean temperature and solar radiation were also identified among the important meteorological variables.

---

# 🔬 Research Workflow

```text
                 NOAA / USCRN
                      │
                      ▼
             Automated Data Collection
                      │
                      ▼
              Data Preprocessing
                      │
                      ▼
             Feature Engineering
                      │
                      ▼
            Correlation Analysis
                      │
                      ▼
          Geographic / Spatial Analysis
                      │
                      ▼
           Homogeneous Station Selection
                      │
                      ▼
             Regional Dataset
                      │
                      ▼
              10-Day Lookback
                      │
                      ▼
             Model Experiments
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
   Standalone       Hybrid       Ensemble
     Models         Models       Evaluation
        │             │             │
        └─────────────┼─────────────┘
                      ▼
             Performance Analysis
                      │
                      ▼
           One-Day Soil Moisture
                 Forecast
```

---

# 📏 Evaluation Metrics

The experiments use three main evaluation metrics.

### R² — Coefficient of Determination

Measures how much of the variance in the target variable is explained by the model.

### MAE — Mean Absolute Error

Measures the average absolute difference between predicted and observed values.

### RMSE — Root Mean Squared Error

Measures prediction error while placing greater weight on larger errors.

---

# 🌾 Potential Application

The one-day forecasting framework is intended to support data-driven agricultural decision-making.

Accurate prediction of tomorrow's soil moisture can provide information for:

* Precision irrigation
* Water resource management
* Agricultural drought assessment
* Crop management
* Irrigation scheduling

The research identifies one-day-ahead forecasting as a potential foundation for optimizing irrigation decisions and reducing unnecessary water use.

---

# 🔬 Research Contributions

The study brings together several components:

### Regional Modeling

Station clustering is used to address spatial heterogeneity in large-scale environmental datasets.

### Soil Profile Correlation

The strong statistical relationship between different soil depths supports the use of surface moisture as a representative forecasting variable.

### Hybrid Learning

The experiments investigate the combination of gradient-boosted trees with sequential deep-learning architectures.

### Temporal Forecasting

A 10-day historical window is used to predict soil moisture for the following day.

### Environmental Data Engineering

The research includes automated data collection, preprocessing, feature reduction, spatial analysis, model experimentation, and performance evaluation.

---

# 📚 Data Sources

The research uses and references several environmental data resources:

* **NOAA National Centers for Environmental Information — U.S. Climate Reference Network (USCRN)**
* **ESA Climate Change Initiative Soil Moisture**
* **International Soil Moisture Network (ISMN)**

The **USCRN** is the primary data source used in the experiments.

---

# 🔮 Future Research

Future extensions identified by the study include:

* 🌿 Integration of dynamic vegetation indices
* 🪨 Integration of high-resolution soil texture data
* 🌎 Evaluation across additional climatic zones
* 🔍 Improved model interpretability
* 📊 Investigation of broader model generalization

These extensions could help evaluate whether the regional modeling strategy remains effective across more diverse environmental conditions.

---

# 📄 Research Paper

**Regional Hybrid Stacking with XGBoost and Deep Attention for One-Day Soil Moisture Forecasting**

**Author:** Yassir Tagemouati
**Date:** May 27, 2026

The complete research paper contains the detailed methodology, mathematical formulations, experimental analysis, figures, and references used in this project.
