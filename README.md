# Predicting Poverty and Income Using Satellite Data

This project explores whether **satellite-derived features and night-time lights can predict local economic conditions in developing countries**, including income and multidimensional deprivation.

The analysis combines georeferenced household survey data with satellite-derived MOSAIKS features, night-time light indicators, income estimates, and Multidimensional Poverty Index (MPI) data. Machine-learning models are used to assess how well remotely sensed information can explain variation in economic well-being both across and within countries.

The project was developed as part of exploratory research on using alternative and geospatial data sources for human development and poverty measurement.

## Research Questions

The analysis explores several related questions:

1. How well can satellite-derived features predict local income and multidimensional deprivation?
2. Does combining satellite imagery with night-time lights improve predictive performance?
3. How much variation in economic conditions can satellite data explain **within countries**, after removing average differences between countries?
4. Does predictive performance differ between **urban and rural areas**?
5. How does model performance vary across countries?

## Data

The project integrates several sources of spatial and socioeconomic information at the DHS cluster level.

### DHS-linked geographic observations

DHS cluster identifiers (`DHSID`) provide the common geographic unit used to combine the different datasets.

### MOSAIKS satellite features

The analysis uses high-dimensional satellite-image features generated using the **MOSAIKS** representation.

Approximately 4,000 satellite-derived features are used to characterize the physical and built environment surrounding survey locations.

These features allow satellite imagery to be incorporated into statistical models without manually defining individual land-cover or infrastructure indicators.

### Night-time lights

Night-time light features are included as an alternative remotely sensed measure of local economic activity.

The project uses DMSP night-light features aggregated around DHS locations.

### Income

Cluster-level income estimates are constructed by aggregating individual income measures to DHS locations.

Income is log-transformed for the modelling analysis.

### Multidimensional Poverty

The analysis incorporates the **weighted deprivation score** derived from MPI data.

In parts of the analysis, the underlying MPI indicators are also processed, including deprivations related to:

- child mortality
- nutrition
- school attendance
- education
- electricity
- drinking water
- sanitation
- housing
- cooking fuel
- assets

### Urban–Rural Classification

DHS/MPI geographic information is used to distinguish between urban and rural survey locations, allowing model performance to be evaluated separately across these contexts.

## Data Integration

The different datasets are linked using the DHS geographic identifier.

## Modelling Strategy

The project uses **Ridge regression with cross-validation** to predict economic outcomes from high-dimensional satellite features.

Ridge regression is particularly useful in this setting because the MOSAIKS representation contains thousands of potentially correlated predictors.

Hyperparameters are selected using cross-validation over a range of regularisation parameters.

Model performance is evaluated using out-of-sample **R²**.

## Outcomes

The primary outcomes modelled are:

### Multidimensional deprivation

The MPI weighted deprivation score is used to represent the intensity of deprivation experienced by households around each survey location.

### Income

Cluster-level income estimates are log-transformed and used as an alternative measure of economic well-being.

Together, these outcomes allow the project to compare the ability of satellite information to predict both **monetary and multidimensional measures of living standards**.

## Model Comparisons

Three sets of remotely sensed predictors are compared.

### 1. MOSAIKS

Uses approximately 4,000 satellite-image features.

`Economic outcome ~ MOSAIKS features`

### 2. Night-time lights

Uses night-time light indicators as predictors.

`Economic outcome ~ Night-time light features`

### 3. MOSAIKS + Night-time lights

Combines both sources of remotely sensed information.

`Economic outcome ~ MOSAIKS features + Night-time lights`

Comparing these models helps assess whether high-dimensional satellite imagery contains information about living standards beyond conventional night-time-light measures.

## Within-Country Analysis

A major component of the project examines whether satellite data can explain **variation within countries**, rather than simply distinguishing richer countries from poorer countries.

Variables are demeaned within country:

`X_ic - mean(X_c)`

where observations are measured relative to the corresponding country's average.

This removes average between-country differences and allows the models to focus on geographic variation in economic conditions **within the same country**.

The within-country models are estimated separately using:

- MOSAIKS features
- night-time lights
- MOSAIKS + night-time lights

for both multidimensional deprivation and income.

## Country-Specific Models

The project also estimates separate predictive models for individual countries.

For each country:

1. observations are selected from the integrated dataset;
2. data are divided into training and testing samples;
3. Ridge regression models are trained using cross-validation;
4. predictions are generated for the test sample;
5. out-of-sample R² is calculated.

This makes it possible to examine how the relationship between remotely sensed information and economic well-being varies across national contexts.

## Urban–Rural Analysis

A second notebook extends the within-country analysis by separating DHS locations into **urban and rural areas**.

Models are estimated independently for rural and urban observations using:

- night-time lights
- MOSAIKS features
- MOSAIKS + night-time lights

Features are standardized before estimation, and Ridge regression models are tuned using cross-validation.

This analysis explores whether remotely sensed data capture poverty and economic conditions differently in densely built urban environments compared with rural areas.

## Analytical Workflow

## Tools

The analysis is implemented in **Python** using:

- `pandas` and `numpy` for data processing
- `scikit-learn` for machine learning and model evaluation
- `geopandas` for geospatial data
- `matplotlib` for visualization
- `joblib` for parallel model estimation

Key modelling tools include:

- Ridge regression
- RidgeCV
- repeated K-fold cross-validation
- train/test validation
- feature standardization
- out-of-sample R² evaluation

## Repository Structure

The repository currently contains two main analytical notebooks.

### `1. Within and country specific models.ipynb`

Develops the main predictive models and compares:

- MOSAIKS
- night-time lights
- MOSAIKS + night-time lights

The notebook estimates both pooled within-country models and separate country-specific models.

### `2. Within country analysis-urban-rural.ipynb`

Extends the analysis by separating observations into urban and rural locations and comparing predictive performance across the two contexts.

