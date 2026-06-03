# 02_GOES_Cloud_Dataset

## Overview

This notebook implements the complete satellite processing pipeline used to extract cloud-related atmospheric features from GOES-18 imagery over the Quito Astronomical Observatory (OAQ).

The objective of this stage is to transform raw GOES-18 ABI observations into a compact set of physically meaningful predictors describing cloud cover, cloud structure, and atmospheric conditions above the observatory.

This notebook corresponds to **Phase 2A: GOES Satellite Characterization** of the OAQ-AstroForecast project.

---

## Scientific Objective

The main objective is to determine which GOES-derived variables are most suitable for representing sky conditions relevant to astronomical observations.

Unlike ground-based weather stations, satellite imagery provides direct information about:

* Cloud coverage
* Cloud-top temperature
* Spatial cloud heterogeneity
* Atmospheric moisture structure

These variables are expected to improve short-term cloud forecasting performance when combined with local meteorological measurements.

---

## Input Data

### Satellite Platform

GOES-18 (Geostationary Operational Environmental Satellite)

### Product

ABI-L2-MCMIPF

Advanced Baseline Imager (ABI)

Level 2 Cloud and Moisture Imagery

Full Disk Mode

### Temporal Resolution

10 minutes

### Spatial Resolution

Approximately 2 km for infrared channels.

---

## Observatory Location

### Quito Astronomical Observatory (OAQ)

Latitude:

-0.2150°

Longitude:

-78.5025°

Altitude:

≈ 2820 m above sea level

---

## Methodology

### 1. Georeferencing

The observatory coordinates are converted from geographic coordinates (latitude and longitude) into GOES fixed-grid coordinates.

The corresponding satellite pixel is identified using the geostationary projection parameters contained within the NetCDF product.

### 2. Spatial Sampling Window

A square window centered on the observatory is extracted from each satellite scene.

Several window sizes were evaluated:

* 3 × 3 pixels
* 5 × 5 pixels
* 7 × 7 pixels
* 9 × 9 pixels

After exploratory analysis, a **5 × 5 window** was selected as the best compromise between:

* Local representativeness
* Noise reduction
* Computational efficiency

### 3. Infrared Channel Selection

Three infrared channels were selected:

| Channel | Wavelength | Description           |
| ------- | ---------- | --------------------- |
| C13     | 10.3 µm    | Clean infrared window |
| C15     | 12.3 µm    | Longwave infrared     |
| C16     | 13.3 µm    | CO₂ absorption band   |

These channels are commonly used for cloud detection and cloud-top characterization.

### 4. Statistical Feature Extraction

For each channel and each scene, the following statistics are computed over the 5 × 5 window:

* Mean
* Standard deviation
* Minimum
* Maximum

The standard deviation is interpreted as a measure of spatial cloud heterogeneity.

### 5. Brightness Temperature Differences (BTD)

Additional spectral features are computed:

BTD13-15

BTD13-16

BTD15-16

where:

BTD(i-j) = Channel_i − Channel_j

These differences are commonly used in cloud classification and atmospheric moisture analysis.

---

## Extracted Features

### Thermal Features

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

### Spectral Features

* btd_13_15
* btd_13_16
* btd_15_16

### Metadata

* timestamp

---

## Exploratory Analysis

A complete 24-hour GOES time series was processed for 10 May 2026.

### Data Summary

Downloaded scenes:

144

Valid scenes:

134

Corrupted scenes:

6

### Correlation Analysis

Strong redundancy was observed among:

* cmi_c13_mean
* cmi_c15_mean
* cmi_c16_mean

Similarly, high correlations were found among:

* cmi_c13_std
* cmi_c15_std
* cmi_c16_std

However, the brightness temperature differences exhibited distinct temporal behavior and provided complementary atmospheric information.

### Temporal Analysis

Time-series analysis revealed:

* Rapid cloud evolution events
* Strong variations in brightness temperature
* Episodes of increased spatial heterogeneity
* Distinct behavior of BTD variables relative to thermal channels

---

## Preliminary Feature Selection

Based on the exploratory analysis, the following variables were identified as the most promising candidates:

* cmi_c13_mean
* cmi_c13_std
* btd_13_15
* btd_13_16

These variables capture:

* Cloud coverage
* Cloud spatial structure
* Spectral atmospheric properties

while minimizing redundancy.

---

## Output Products

### Processed GOES Dataset

Location:

data/processed/

Example:

goes_features_2026_05_10.csv

This dataset contains one row per GOES scene and serves as the primary satellite feature repository for subsequent stages.

---

## Limitations

This notebook focuses exclusively on satellite feature extraction and characterization.

No comparison with ground observations is performed here.

Validation against local meteorological measurements is conducted separately in:

03_GOES_Ambient_Validation.ipynb

---

## Next Stage

The next phase evaluates the physical consistency of GOES-derived variables by comparing them with measurements obtained from the Ambient Weather station located at the observatory.

Notebook:

03_GOES_Ambient_Validation.ipynb

Objective:

GOES Satellite Features → Ground-Based Atmospheric Validation

---

## Project Context

OAQ-AstroForecast

Machine Learning-Based Cloud Forecasting for Astronomical Observations at the Quito Astronomical Observatory.
