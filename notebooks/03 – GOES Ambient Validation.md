# 03 – GOES Ambient Validation

## Overview

This notebook performs the first physical validation of the GOES-18 satellite features extracted in the previous phase of the project. The objective is to determine whether the selected infrared cloud indicators contain meaningful information about atmospheric conditions observed from the ground at the Quito Astronomical Observatory (OAQ).

To achieve this, GOES-derived cloud features are compared against measurements from the Ambient Weather station located at the observatory. Surface solar radiation is used as an independent proxy for cloudiness because cloud cover directly modulates the amount of solar energy reaching the surface.

This validation serves as a critical intermediate step before incorporating additional atmospheric data sources such as GFS forecasts and ERA5 reanalysis products.

---

## Objectives

The main objectives of this notebook are:

1. Synchronize GOES satellite observations with ground-based observations from the Ambient Weather station.
2. Validate whether the selected GOES features are physically related to observed sky conditions.
3. Quantify the relationship between satellite-derived cloud indicators and surface solar radiation.
4. Evaluate linear and non-linear relationships.
5. Identify the most informative GOES features for future forecasting models.
6. Determine whether GOES features can discriminate between clear-sky and cloudy conditions.

---

## Input Datasets

### GOES Dataset

Generated in:

```text
02_GOES_Cloud_Dataset.ipynb
```

File:

```text
data/processed/goes_features_2026_05_10.csv
```

Contains the cloud features extracted from a 5×5 pixel window centered on the observatory.

Variables:

* cmi_c13_mean
* cmi_c13_std
* cmi_c13_min
* cmi_c13_max
* cmi_c15_mean
* cmi_c15_std
* cmi_c15_min
* cmi_c15_max
* cmi_c16_mean
* cmi_c16_std
* cmi_c16_min
* cmi_c16_max
* btd_13_15
* btd_13_16
* btd_15_16
* timestamp

---

### Ambient Weather Dataset

Generated in:

```text
01_EDA_Station_Validation.ipynb
```

File:

```text
data/processed/jardin_2026_05_10_10min.csv
```

Contains 10-minute observations from the selected reference station:

```text
Station: Jardín
```

Variables:

* time_10min
* solar_radiation

---

## Methodology

### 1. Temporal Alignment

GOES observation timestamps were converted from UTC to Ecuador local time (UTC-5).

All timestamps were rounded to 10-minute intervals to match the temporal resolution of the Ambient Weather station.

A temporal inner join was performed using:

```python
pd.merge(...)
```

Resulting synchronized dataset:

```text
GOES records      : 134
Ambient records   : 144
Merged records    : 114
```

---

### 2. Daytime Filtering

Only daytime observations were retained:

```python
solar_radiation > 20 W/m²
```

Resulting dataset:

```text
66 daytime observations
```

This filtering removes nighttime conditions where solar radiation is physically uninformative for cloud validation.

---

### 3. Correlation Analysis

Pearson correlation coefficients were calculated between:

```text
Solar Radiation
```

and:

```text
cmi_c13_mean
cmi_c13_std
btd_13_15
btd_13_16
```

Spearman rank correlations were also computed to evaluate possible non-linear monotonic relationships.

---

### 4. Robustness Tests

Additional analyses included:

* Spearman correlation
* Logarithmic transformation
* Square-root transformation
* Outlier removal
* Recalculation of correlation metrics

These tests were performed to determine whether the observed relationships were:

* driven by outliers,
* strongly non-linear,
* sensitive to scale transformations.

---

### 5. Non-Linear Validation

A LOWESS (Locally Weighted Scatterplot Smoothing) model was fitted between:

```text
BTD13-15
```

and

```text
Solar Radiation
```

The LOWESS model was compared against a simple linear regression.

The objective was to determine whether satellite-cloud relationships are better represented by non-linear models.

---

### 6. Clear-Sky vs Cloudy-Sky Classification

Observations were divided into two groups:

```text
Clear Sky:
Solar Radiation > 500 W/m²

Cloudy Sky:
Solar Radiation ≤ 500 W/m²
```

The distribution of BTD13-15 values was compared between both groups using:

* Welch's t-test
* Cohen's d effect size

---

## Results

### Pearson Correlations

| Variable | Pearson r |
| -------- | --------- |
| C13 Mean | 0.479     |
| C13 Std  | 0.039     |
| BTD13-15 | 0.603     |
| BTD13-16 | 0.546     |

BTD13-15 exhibited the strongest linear relationship with solar radiation.

---

### Spearman Correlations

| Variable | Spearman ρ |
| -------- | ---------- |
| C13 Mean | 0.361      |
| C13 Std  | 0.143      |
| BTD13-15 | 0.530      |
| BTD13-16 | 0.420      |

Spearman coefficients were generally lower than Pearson coefficients, indicating that strong hidden monotonic non-linear relationships were not present.

---

### Outlier Robustness

Removing the two highest solar radiation observations produced only minor changes in the correlation coefficients.

This demonstrates that the observed relationships are not driven by a small number of extreme observations.

---

### Linear Regression

For:

```text
BTD13-15 vs Solar Radiation
```

Results:

```text
r       = 0.603
R²      = 0.364
p-value = 8.35 × 10⁻⁸
```

The relationship is statistically significant and physically meaningful.

---

### LOWESS Analysis

Results:

```text
R² Linear  = 0.364
R² LOWESS  = 0.574
```

The non-parametric model substantially improved the explained variance.

This indicates that the relationship between BTD13-15 and solar radiation is strongly non-linear.

---

### Clear vs Cloudy Conditions

Welch's t-test:

```text
t = 5.148
p = 8.95 × 10⁻⁶
```

The difference between both groups is highly significant.

---

### Effect Size

Cohen's d:

```text
d = 1.336
```

Interpretation:

```text
Very Large Effect
```

This indicates strong separation between clear-sky and cloudy-sky atmospheric states.

---

## Main Findings

### Confirmed Features

The following variables demonstrated physical consistency with independent ground-based observations:

* BTD13-15
* BTD13-16
* C13 Mean

---

### Strongest Feature

BTD13-15 emerged as the most informative GOES-derived variable.

Evidence includes:

* Highest Pearson correlation
* Significant Spearman correlation
* Robustness to outliers
* Strong non-linear relationship
* Significant clear/cloudy discrimination
* Large effect size

---

### Weakest Feature

C13 Standard Deviation showed negligible correlation with solar radiation and limited evidence of direct physical relevance in this validation stage.

It remains available for future multivariate analysis but is not considered a primary cloud indicator.

---

## Conclusions

This notebook provides the first independent validation of the GOES-derived cloud features used in OAQ-AstroForecast.

The results demonstrate that several satellite-derived infrared indicators contain meaningful information about atmospheric conditions observed from the ground.

In particular, BTD13-15 consistently emerged as the strongest cloud-related predictor and successfully discriminated between clear and cloudy sky conditions.

These findings justify retaining the validated GOES features for future integration with:

* Ambient Weather observations
* GFS numerical weather forecasts
* ERA5 atmospheric reanalysis

and for their subsequent use in machine learning models for astronomical cloud forecasting.

---

## Next Phase

```text
Phase 3
GFS Characterization
```

Future work will focus on incorporating numerical weather prediction variables and evaluating their complementary predictive value relative to GOES observations.
