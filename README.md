# 🌱 Urban Heat & Green-Cover Impact Modeling

### Statistical Modeling of Urban Vegetation and Land Surface Temperature Anomalies

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-Machine%20Learning-F7931E?logo=scikit-learn)
![Statsmodels](https://img.shields.io/badge/Statsmodels-Statistical%20Modeling-3C5A99)
![Research](https://img.shields.io/badge/Project-Research%20Oriented-purple)
![Status](https://img.shields.io/badge/Status-Research%20%2F%20Development-yellow)

---

## 📌 Overview

**Urban Heat & Green-Cover Impact Modeling** is a research-oriented data science project that investigates the relationship between **urban vegetation** and **Land Surface Temperature (LST) anomalies**.

The project focuses not only on predicting temperature, but primarily on understanding and quantifying the statistical association between vegetation and urban heat after accounting for other measurable environmental and urban characteristics.

### Core Research Question

> **How strongly is urban vegetation associated with local surface temperature after accounting for built-environment and other environmental variables?**

The primary vegetation indicator is **NDVI (Normalized Difference Vegetation Index)**, while LST anomaly is used as the primary response variable.

---

# 🎯 Objectives

The main objectives of this project are to:

* Investigate the relationship between urban vegetation and LST anomaly.
* Quantify the association between NDVI and surface temperature.
* Control for relevant urban and environmental variables.
* Compare simple and multivariable regression models.
* Investigate possible nonlinear relationships.
* Evaluate multicollinearity among explanatory variables.
* Perform statistical and residual diagnostics.
* Examine whether residual patterns vary across cities and climate zones.
* Evaluate model robustness using grouped cross-validation.
* Provide interpretable coefficient estimates rather than relying only on prediction metrics.

---

# 🔬 Research Perspective

A conventional machine-learning project might ask:

> **Can we predict urban temperature accurately?**

This project asks a different question:

> **What is the estimated association between vegetation and temperature after accounting for other variables?**

For example, the analysis aims to produce an interpretable statement such as:

> **A 0.1-unit increase in NDVI is associated with an estimated X °C change in LST anomaly, holding the other variables in the model constant.**

The exact value of **X** is obtained from the fitted statistical model.

This makes the project particularly relevant to:

* Environmental Data Science
* Urban Climate Research
* Statistical Modeling
* Geospatial Analytics
* Sustainability Research
* Urban Planning
* Climate Adaptation

---

# 📊 Dataset

The project currently uses:

```text
global_urban_heat_island_2015_2025.csv
```

### Dataset Summary

| Property       |                  Value |
| -------------- | ---------------------: |
| Observations   |                    550 |
| Cities         |                     50 |
| Countries      |                     28 |
| Years          |              2015–2025 |
| Climate Zones  |                     15 |
| Data Structure | City-Year observations |

The dataset therefore contains repeated observations for cities across multiple years.

---

# 🧾 Variables

## Target Variable

### `lst_anomaly_c`

Land Surface Temperature anomaly measured in °C.

This is the primary dependent variable used throughout the regression analysis.

---

## Primary Predictor

### `ndvi_mean`

Mean NDVI (Normalized Difference Vegetation Index).

NDVI is used as the primary measure of urban vegetation/greenness.

The primary coefficient of interest is:

$$
\beta_{NDVI}
$$

The estimated association for a 0.1-unit increase in NDVI is:

$$
0.1 \times \beta_{NDVI}
$$

---

## Urban Surface Variable

### `impervious_pct`

Percentage of impervious surface.

This variable represents the built/impervious component of the urban environment.

---

## Alternative Vegetation Variable

### `tree_canopy_pct`

Percentage of tree canopy.

This is used as an alternative vegetation indicator for sensitivity analysis.

---

## Population Variable

### `pop_density_km2`

Population density per square kilometre.

This is included as an urban-density control.

---

## Climate Variable

### `climate_zone`

Climate classification associated with each city.

Climate-zone effects are considered in the adjusted regression analysis.

---

## Temporal Variable

### `year`

Year of observation.

The dataset covers the period:

```text
2015–2025
```

---

## Geographic Identifier

### `city`

City identifier used to distinguish repeated observations and support grouped validation and panel-oriented analysis.

---

## Additional Variable

### `heat_mortality_per_100k`

Heat-related mortality per 100,000 population.

This variable is available for potential future health-impact analysis and is not treated as a primary predictor of LST anomaly.

---

# ⚠️ Current Data Scope

The original research concept includes several additional environmental variables, but they are **not currently available in the supplied dataset**.

These include:

* Elevation
* Distance from water bodies
* Latitude
* Longitude
* Separate built-up-area measurement

Therefore, these variables are not included in the current regression specifications.

Future versions of the project can incorporate them to strengthen environmental and spatial analysis.

---

# 🧠 Methodology

The analysis follows a progressive modeling framework.

```text
                    ┌─────────────────────┐
                    │       Dataset       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Data Validation &   │
                    │ Preprocessing       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Exploratory Data    │
                    │ Analysis             │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Correlation & VIF   │
                    └──────────┬──────────┘
                               │
                               ▼
             ┌─────────────────┴─────────────────┐
             │                                   │
             ▼                                   ▼
    Simple Linear Regression          Multiple Linear Regression
             │                                   │
             └─────────────────┬─────────────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Quadratic /         │
                    │ Nonlinear Model     │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Robustness &        │
                    │ Sensitivity Tests   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Residual Diagnostics│
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Grouped Validation  │
                    └─────────────────────┘
```

---

# 📐 Statistical Models

## 1. Simple Linear Regression

The first model evaluates the unadjusted relationship between NDVI and LST anomaly.

$$
LST_i = \beta_0 + \beta_1NDVI_i + \epsilon_i
$$

This provides a baseline estimate of the relationship between vegetation and surface temperature.

---

## 2. Multiple Linear Regression

The primary adjusted model incorporates additional explanatory variables:

$$
LST_{it}
=
\beta_0
+
\beta_1NDVI_{it}
+
\beta_2Impervious_i
+
\beta_3Population_i
+
\gamma Climate_i
+
\delta Year_t
+
\epsilon_{it}
$$

The NDVI coefficient is interpreted while accounting for the other variables included in the model.

---

## 3. Quadratic Regression

The project investigates whether the NDVI relationship may be nonlinear.

The quadratic specification is:

$$
LST =
\beta_0
+
\beta_1NDVI_c
+
\beta_2NDVI_c^2
+
Controls
+
\epsilon
$$

where:

$$
NDVI_c = NDVI-\overline{NDVI}
$$

This allows the analysis to investigate whether the temperature relationship changes at different vegetation levels.

---

# 🌿 Main Effect Interpretation

The central quantity of interest is the NDVI coefficient:

$$
\beta_{NDVI}
$$

If the estimated coefficient is \(\beta\), then a 0.1-unit increase in NDVI corresponds to:

$$
\Delta LST = 0.1\beta
$$

### Example interpretation format

> A 0.1-unit increase in NDVI is associated with an estimated **X °C difference in LST anomaly**, conditional on the other variables included in the model.

The interpretation should always be accompanied by:

* Estimated coefficient
* Standard error
* 95% confidence interval
* p-value
* Model specification

---

# 🔍 Multicollinearity Analysis

The project evaluates multicollinearity using:

### Correlation Analysis

A correlation matrix is used to identify strong relationships between continuous variables.

### Variance Inflation Factor (VIF)

VIF is calculated to identify potential multicollinearity among explanatory variables.

This is particularly important because:

```text
NDVI
  ↕
Tree Canopy
  ↕
Urban Surface Characteristics
```

may exhibit substantial correlations.

High multicollinearity can increase coefficient uncertainty and make individual predictor effects difficult to interpret.

---

# 🧪 Statistical Diagnostics

The project includes several diagnostic procedures.

### Residual vs Fitted Plot

Used to investigate:

* Nonlinearity
* Heteroskedasticity
* Systematic prediction errors

### Q-Q Plot

Used to inspect residual distribution.

### Breusch-Pagan Test

Used to assess evidence of heteroskedasticity.

### Durbin-Watson Diagnostic

Used as an initial diagnostic for serial correlation.

Because the data contain repeated city observations, temporal and within-city dependence should be considered when interpreting this diagnostic.

---

# 🏙️ Panel Structure

The dataset contains repeated observations for the same cities over several years.

Therefore, observations from a single city cannot necessarily be considered completely independent.

The current analysis uses **city-clustered standard errors** for the main regression inference.

A recommended robustness extension is a:

### City + Year Fixed-Effects Model

$$
LST_{it}
=
\beta_1NDVI_{it}
+
\alpha_i
+
\gamma_t
+
\epsilon_{it}
$$

where:

* \(\alpha_i\) = city-specific fixed effects
* \(\gamma_t\) = year fixed effects

This specification focuses on changes within cities over time.

---

# 🤖 Model Validation

Prediction is treated as a secondary objective.

Because multiple observations belong to the same cities, random train/test splitting can cause information leakage.

The project therefore uses **city-grouped cross-validation**.

Conceptually:

```text
City A ─┐
City B ─┤
City C ─┤── Training
City D ─┘

City E ─── Validation
```

rather than randomly distributing observations from the same city across both datasets.

### Evaluation Metrics

* MAE
* RMSE
* R²

---

# 📊 Evaluation Metrics

## Mean Absolute Error

$$
MAE =
\frac{1}{n}
\sum_{i=1}^{n}|y_i-\hat{y}_i|
$$

---

## Root Mean Squared Error

$$
RMSE =
\sqrt{
\frac{1}{n}
\sum_{i=1}^{n}(y_i-\hat{y}_i)^2
}
$$

---

## R²

$$
R^2 =
1-\frac{SS_{res}}{SS_{tot}}
$$

---

# 📉 Residual Analysis

Residuals are calculated as:

$$
e_i=y_i-\hat{y}_i
$$

The project investigates residuals across:

* Cities
* Climate zones
* Fitted values
* Observation distributions

Large residuals can indicate locations or conditions that are not adequately represented by the current model.

---

# 🗺️ Spatial Analysis — Planned Extension

A major future component is spatial residual analysis.

However, genuine spatial statistics require geographic information such as:

* Latitude
* Longitude
* Geographic boundaries

The current dataset does not provide sufficient geographic coordinates for formal spatial autocorrelation analysis.

Future work can therefore include:

### Global Spatial Autocorrelation

* Moran's I

### Local Spatial Analysis

* Local Moran's I
* LISA

### Spatial Modeling

* Spatial lag models
* Spatial error models
* Geographically Weighted Regression

Potential workflow:

```text
Coordinates
     ↓
GeoDataFrame
     ↓
Spatial Weights
     ↓
Residual Mapping
     ↓
Moran's I
     ↓
Local Spatial Clusters
     ↓
Spatial Model
```

---

# 📁 Repository Structure

Recommended repository structure:

```text
Urban-Heat-Green-Cover-Impact-Modeling/
│
├── README.md
│
├── Urban_Heat_Green_Cover_Impact_Modeling.ipynb
│
├── global_urban_heat_island_2015_2025.csv
│
├── requirements.txt
│
├── results/
│   ├── figures/
│   ├── tables/
│   └── model_outputs/
│
└── docs/
    └── research_notes.md
```

For a minimal version:

```text
Urban-Heat-Green-Cover-Impact-Modeling/
│
├── README.md
├── Urban_Heat_Green_Cover_Impact_Modeling.ipynb
├── global_urban_heat_island_2015_2025.csv
└── requirements.txt
```

---

# 🛠️ Technology Stack

| Category                | Technology          |
| ----------------------- | ------------------- |
| Language                | Python              |
| Notebook                | Jupyter Notebook    |
| Data Processing         | Pandas              |
| Numerical Computing     | NumPy               |
| Visualization           | Matplotlib, Seaborn |
| Statistical Modeling    | Statsmodels         |
| Machine Learning        | Scikit-learn        |
| Validation              | Scikit-learn        |
| Future Spatial Analysis | GeoPandas, PySAL    |

---

# 📦 Installation

Clone the repository:

```bash
git clone <YOUR_REPOSITORY_URL>
cd Urban-Heat-Green-Cover-Impact-Modeling
```

Create a virtual environment:

```bash
python -m venv venv
```

### Windows

```bash
venv\Scripts\activate
```

### Linux / macOS

```bash
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Launch Jupyter:

```bash
jupyter notebook
```

Open:

```text
Urban_Heat_Green_Cover_Impact_Modeling.ipynb
```

---

# 📋 Requirements

Create a `requirements.txt` file containing:

```text
numpy
pandas
matplotlib
seaborn
scikit-learn
statsmodels
jupyter
```

For future spatial extensions:

```text
geopandas
libpysal
esda
contextily
```

---

# ▶️ Running the Project

1. Clone the repository.
2. Install the required dependencies.
3. Place the dataset in the repository root or update the notebook path.
4. Launch Jupyter Notebook.
5. Open the project notebook.
6. Run the notebook cells sequentially.
7. Review the generated statistical outputs and visualizations.

---

# 📌 Key Outputs

The project generates or investigates:

* Dataset summary statistics
* Missing-value analysis
* Distribution plots
* NDVI–LST relationship plots
* Correlation matrix
* VIF analysis
* Regression coefficients
* Confidence intervals
* Statistical significance
* Effect-size estimates
* Model comparison
* Residual plots
* Q-Q plots
* Heteroskedasticity diagnostics
* City-level residual summaries
* Climate-zone residual summaries
* Cross-validation performance

---

# ⚠️ Important Interpretation Note

This is an **observational statistical analysis**.

Therefore:

$$
\boxed{\text{Association} \neq \text{Causation}}
$$

The estimated NDVI coefficient should not automatically be interpreted as evidence that changing vegetation will causally change LST by the estimated amount.

Preferred terminology:

* "associated with"
* "estimated association"
* "conditional relationship"
* "adjusted relationship"

Avoid unsupported causal statements such as:

> Increasing NDVI by 0.1 causes LST to decrease by X°C.

Instead:

> A 0.1-unit increase in NDVI is associated with an estimated X°C difference in LST anomaly after adjustment for the variables included in the model.

---

# 🚧 Limitations

### 1. Observational Data

The project does not establish causal relationships.

### 2. Missing Geographic Variables

Elevation, latitude, longitude and distance from water bodies are not currently available.

### 3. Limited Urban Morphology

The dataset contains impervious surface percentage but does not provide a separate built-up-area measurement.

### 4. Potential Omitted Variables

Important factors such as:

* Humidity
* Wind
* Solar radiation
* Albedo
* Building height
* Road density
* Surface emissivity
* Anthropogenic heat

may influence urban temperature but are not comprehensively represented.

### 5. Spatial Dependence

Formal spatial autocorrelation analysis cannot be performed without suitable geographic coordinates.

### 6. Panel Dependence

Repeated observations from the same city may be correlated over time.

---

# 🔬 Future Work

The project can be expanded in several directions.

## Geographic Enrichment

Add:

* Latitude
* Longitude
* Elevation
* Distance to water bodies

## Remote-Sensing Features

Potential additions:

* EVI
* LAI
* NDWI
* NDBI
* Albedo
* Surface emissivity
* Land-cover classes

## Urban Morphology

Potential features:

* Building density
* Building height
* Road density
* Urban compactness
* Street-canyon characteristics

## Meteorological Controls

Potential additions:

* Air temperature
* Relative humidity
* Wind speed
* Precipitation
* Solar radiation

## Advanced Statistical Models

Potential extensions:

* Two-way fixed effects
* Mixed-effects models
* Hierarchical models
* Robust panel regression

## Spatial Statistics

Potential extensions:

* Moran's I
* Local Moran's I
* LISA
* Spatial lag models
* Spatial error models
* Geographically Weighted Regression

---

# 🌍 Potential Research Extension

The dataset also contains:

```text
heat_mortality_per_100k
```

This provides an opportunity for a future research extension investigating whether urban thermal conditions are associated with heat-related health outcomes.

A possible future research framework is:

```text
Urban Vegetation
       ↓
Urban Thermal Environment
       ↓
LST Anomaly
       ↓
Heat Exposure
       ↓
Potential Health Outcomes
```

This would constitute a separate research question and should be analyzed using an appropriate health-oriented statistical design.

---

# ⭐ Project Highlights

### Why this project is different

Most introductory ML projects focus on:

> **Prediction accuracy**

This project additionally focuses on:

> **Interpretability + statistical inference + environmental reasoning**

Key strengths include:

* 🌱 Vegetation-focused analysis
* 🌡️ Urban heat modeling
* 📊 Interpretable regression coefficients
* 🔬 Statistical inference
* 🏙️ City-year panel structure
* 📈 Nonlinear modeling
* 🧪 Statistical diagnostics
* 🔍 Residual investigation
* 🤖 Grouped validation
* 🗺️ Planned spatial analysis
* 📚 Research-oriented methodology

---

# 🏆 Research Contribution

The project is designed around the following analytical progression:

```text
Does vegetation correlate with temperature?
                ↓
Does the relationship remain after adjustment?
                ↓
Is the relationship nonlinear?
                ↓
Is the result robust to alternative vegetation measures?
                ↓
Does the relationship remain within cities over time?
                ↓
Where does the model fail?
                ↓
Are residuals spatially structured?
```

This progression transforms a basic regression exercise into a broader **urban environmental data-science research project**.

---

# 📑 Suggested Research Statement

> This project investigates the association between urban vegetation and Land Surface Temperature anomalies using city-year observations from 2015–2025. Multiple regression specifications are used to estimate the relationship between NDVI and LST anomaly while accounting for urban surface characteristics, population density, climate zone, and temporal variation. The analysis emphasizes interpretable effect estimates, uncertainty quantification, model diagnostics, robustness analysis, and residual investigation rather than prediction alone.

---

# 📌 Project Status

**🚧 Research / Development**

### Completed

* [x] Dataset loading
* [x] Data inspection
* [x] Data validation
* [x] Exploratory Data Analysis
* [x] Descriptive statistics
* [x] Correlation analysis
* [x] VIF analysis
* [x] Simple Linear Regression
* [x] Multiple Linear Regression
* [x] Clustered standard errors
* [x] NDVI coefficient interpretation
* [x] Tree-canopy sensitivity analysis
* [x] Quadratic regression
* [x] Model comparison
* [x] Residual diagnostics
* [x] City-level residual analysis
* [x] Climate-zone residual analysis
* [x] Grouped cross-validation

### Planned

* [ ] City + Year Fixed Effects
* [ ] Geographic coordinate enrichment
* [ ] Elevation integration
* [ ] Distance-to-water feature
* [ ] Spatial residual mapping
* [ ] Moran's I
* [ ] Local Moran's I / LISA
* [ ] Spatial econometric models
* [ ] Expanded environmental controls
* [ ] Formal research paper

---

# 👨‍💻 Author

**Soumyadeep Chattopadhyay**

Data Science • Machine Learning • Statistical Modeling • Environmental Analytics

---

# 📜 License

This project is intended for academic, educational, and research purposes.

If external datasets or third-party resources are incorporated, their original licensing and attribution requirements should be preserved.

---

# ⭐ If You Find This Project Useful

If this project is useful for research, learning, or environmental data science, consider:

* ⭐ Starring the repository
* 🍴 Forking the project
* 🐛 Reporting issues
* 💡 Suggesting improvements
* 🤝 Contributing additional analyses

---

# 🌱 Final Perspective

Urban heat is influenced by a complex interaction of vegetation, built surfaces, climate, population, morphology, and atmospheric conditions.

This project approaches the problem from an interpretable statistical perspective:

$$
\boxed{
\text{Urban Vegetation}
\rightarrow
\text{Statistical Association}
\rightarrow
\text{LST Anomaly}
\rightarrow
\text{Residual Patterns}
\rightarrow
\text{Spatial Research}
}
$$

The ultimate goal is to develop a reproducible analytical framework for understanding how urban environmental characteristics relate to surface thermal conditions while maintaining careful distinction between **prediction, association, and causation**.

---

**🌱 Urban Heat & Green-Cover Impact Modeling**
*An interpretable, research-oriented approach to urban environmental data science.*
