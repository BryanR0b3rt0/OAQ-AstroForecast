# Fase 1: Characterization Report

## OAQ-AstroForecast

### Meteorological Characterization of the Observatorio Astronómico de Quito Monitoring Network

**Project:** OAQ-AstroForecast
**Version:** 1.0
**Period analyzed:** January 2026 – April 2026
**Author:** Bryan Tipán
**Date:** June 2026

---

# 1. Introduction

The first phase of the OAQ-AstroForecast project focuses on the characterization and validation of the meteorological observations acquired by the Ambient Weather stations installed at the Observatorio Astronómico de Quito (OAQ).

The long-term objective of the project is the development of an operational forecasting system capable of estimating astronomical observability conditions using local meteorological measurements, satellite observations, numerical weather prediction products, and machine learning techniques.

Before incorporating external datasets and predictive models, it is necessary to evaluate the quality, consistency, and representativeness of the available meteorological observations.

This report summarizes the quality control procedures, exploratory analysis, and initial scientific findings obtained from the first four months of observations recorded during 2026.

---

# 2. Dataset Description

The analyzed dataset consists of meteorological observations collected by two Ambient Weather stations operating at the Observatorio Astronómico de Quito.

The dataset covers the period from January 1, 2026 to April 29, 2026 with a nominal temporal resolution of 5 minutes.

## Available Variables

* Air temperature (°F)
* Relative humidity (%)
* Wind speed (mph)
* UV index
* Solar radiation
* Daily accumulated precipitation
* Timestamp (UTC)
* Station identifier (MAC address)

## Stations

Two meteorological stations were identified:

| Station ID        | Proposed Location |
| ----------------- | ----------------- |
| F4:CF:A2:E2:BC:AD | Garden            |
| F4:CF:A2:E2:C0:8B | Roof              |

The identification of the stations was inferred through the statistical behavior of the measurements and later confirmed through the exploratory analysis.

---

# 3. Data Quality Assessment

## Dataset Size

The complete dataset contains:

* 61,697 observations
* 8 variables
* Two independent meteorological stations

## Missing Values

The dataset exhibits an exceptionally low missing-data rate.

| Variable        | Missing (%) |
| --------------- | ----------- |
| Temperature     | 0.188       |
| Humidity        | 0.188       |
| Wind Speed      | 0.188       |
| UV Index        | 0.193       |
| Solar Radiation | 0.193       |
| Timestamp       | 0.000       |
| Station ID      | 0.000       |
| Rainfall        | 0.000       |

The overall percentage of missing values remains below 0.2%, indicating a highly reliable monitoring system.

## Duplicate Records

No duplicate observations were detected.

Total duplicated rows:

0

## Temporal Consistency

The expected sampling interval was 5 minutes (300 seconds).

Analysis of temporal differences revealed that more than 99% of observations were recorded at the nominal interval.

Minor deviations of 4, 6, 10, and 15 minutes were detected but represent a negligible fraction of the dataset.

These results indicate excellent temporal consistency and continuity.

---

# 4. Station Identification

Although the physical labels of the stations were not initially available, the exploratory analysis allowed the identification of their probable locations.

## Station BC:AD

Characteristics:

* Higher relative humidity
* Lower wind speeds
* Larger thermal amplitude
* Reduced atmospheric exposure

These characteristics are consistent with a sensor located inside a garden environment surrounded by vegetation and local obstacles.

## Station C0:8B

Characteristics:

* Significantly higher wind speeds
* Lower average humidity
* Greater atmospheric exposure
* More representative of free-air conditions

These characteristics strongly suggest a rooftop installation.

Consequently, the following interpretation is adopted throughout the project:

| Station | Location |
| ------- | -------- |
| BC:AD   | Garden   |
| C0:8B   | Roof     |

Confidence level: Very High.

---

# 5. Daily Meteorological Cycles

## Temperature

Both stations exhibit a well-defined diurnal cycle.

The minimum temperatures occur between 05:00 and 06:00 local time, while maximum temperatures occur around noon.

An interesting thermal inversion between stations was observed:

* During nighttime: Roof temperatures are slightly higher than garden temperatures.
* During daytime: Garden temperatures become higher than roof temperatures.

This behavior suggests distinct thermal responses between both environments.

## Relative Humidity

Both stations exhibit an inverse relationship with temperature.

Relative humidity reaches maximum values during the night and early morning, frequently exceeding 90%.

Minimum humidity values occur around midday when temperatures reach their maximum.

The garden station consistently records higher humidity values than the rooftop station.

## Wind Speed

Wind speed represents the variable with the strongest contrast between stations.

The rooftop station consistently records significantly higher wind speeds throughout the day.

Maximum wind speeds occur between 12:00 and 16:00 local time.

The garden station frequently records near-zero wind conditions, indicating strong shielding effects from surrounding structures and vegetation.

---

# 6. Nighttime Conditions

Since the final objective of the project is astronomical forecasting, a dedicated analysis was performed for the nighttime interval between 18:00 and 06:00 local time.

## Temperature

Nighttime temperatures remain relatively stable at both locations.

Average values:

* Garden: 53.45 °F
* Roof: 53.84 °F

The rooftop station remains slightly warmer during the night.

## Relative Humidity

Nighttime humidity levels are extremely high.

Average values:

* Garden: 93.79 %
* Roof: 90.54 %

Median humidity exceeds 93% at both locations.

These conditions suggest a high probability of condensation events, dew formation, and low-level cloud development.

## Wind Speed

Nighttime wind behavior differs dramatically between stations.

Average values:

* Garden: 0.06 mph
* Roof: 1.57 mph

The rooftop station records approximately twenty-six times higher wind speeds than the garden station.

This result confirms that the rooftop station better represents the atmospheric conditions relevant for astronomical observations.

---

# 7. Microclimatic Differences

One of the most important findings of this phase is the existence of a measurable microclimate between the roof and garden environments.

Three differential variables were identified:

* Temperature difference
* Humidity difference
* Wind speed difference

The most remarkable result is the temperature difference cycle:

* Positive during nighttime
* Negative during daytime

This sign inversion suggests distinct thermal dynamics between both locations and may become an important predictor in future machine learning models.

Similarly, humidity and wind differences exhibit systematic and physically consistent behavior throughout the day.

---

# 8. Main Findings

The following conclusions summarize the results obtained during Phase 1:

1. The dataset presents excellent quality with less than 0.2% missing data.
2. No duplicated observations were detected.
3. Temporal sampling is highly consistent with the nominal 5-minute resolution.
4. Two distinct microclimatic environments were identified within the observatory.
5. The rooftop station better represents atmospheric conditions relevant for astronomical observations.
6. Nighttime humidity frequently exceeds 90%, highlighting the importance of condensation-related variables.
7. Temperature, humidity, and wind differences between stations reveal physically meaningful local atmospheric dynamics.
8. Several candidate predictive variables were identified for future forecasting models.

---

# 9. Conclusions and Next Steps

Phase 1 successfully validated the quality and scientific usability of the meteorological dataset.

The analysis confirmed the existence of meaningful microclimatic differences between the roof and garden environments and identified the rooftop station as the most representative source of information for astronomical forecasting purposes.

The next phase of the project will focus on the acquisition and integration of satellite-derived cloud cover products from GOES, which will serve as the primary target variable for forecasting astronomical observability.

The outputs generated during this phase provide a solid foundation for subsequent machine learning development and future scientific publication.
