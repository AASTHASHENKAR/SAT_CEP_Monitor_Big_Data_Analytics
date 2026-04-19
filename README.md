# SAT_CEP_Monitor_Big_Data_Analytics
# 🛰️ SAT-CEP-Monitor: Air Pollution Event Detector
### Big Data Analytics (BDA) Mini Project

> **Based on:** *SAT-CEP-Monitor: An air quality monitoring software architecture combining complex event processing with satellite remote sensing*

---

## 📌 Problem Statement

Air pollution monitoring faces two major limitations:

- **Satellite data** → Wide coverage but cannot process events in Near Real-Time (NRT)
- **Ground sensors** → Real-time but sparse coverage, cannot reach remote/inaccessible areas

This project implements a **PySpark-based software architecture** that combines **Complex Event Processing (CEP)** with **satellite remote sensing data** to detect air pollution events in Near Real-Time — directly inspired by the SAT-CEP-Monitor paper.

---

## 🏗️ Architecture

```
Satellite Remote Sensing Data (AOD, NO2, SO2, CO, O3)
      ↓
[Data Ingestion Layer]     ← PySpark / Simulated Data Streams
      ↓
[Preprocessing Layer]      ← Cleaning, Validation, Feature Engineering
      ↓
[Big Data Processing]      ← PySpark RDD + DataFrame Transformations
      ↓
[CEP Engine]               ← 8 Complex Event Processing Rules
      ↓
[Pollution Event Detector] ← Threshold + Multi-Parameter Pattern Alerts
      ↓
[Analytics & Reporting]    ← AQI Computation, Trend Analysis, Rankings
```

---

## 🗺️ Regions Covered

Modeled on the exact regions used in the SAT-CEP-Monitor paper:

| Country | Cities |
|---------|--------|
| 🇲🇦 Morocco | Casablanca, Rabat, Marrakech, Fes |
| 🇪🇸 Spain | Madrid, Barcelona, Seville, Valencia |

---

## 🛰️ Satellite Sensors Simulated

| Sensor | Agency |
|--------|--------|
| MODIS-Terra | NASA |
| MODIS-Aqua | NASA |
| Sentinel-5P | ESA |
| TROPOMI | ESA / Copernicus |
| VIIRS | NOAA / NASA |

---

## 📊 Atmospheric Parameters

| Parameter | Unit | Description |
|-----------|------|-------------|
| **AOD** | — | Aerosol Optical Depth — measures how much particles block sunlight |
| **NO₂** | µmol/m² | Nitrogen Dioxide — trace gas from combustion |
| **SO₂** | µmol/m² | Sulfur Dioxide — industrial / volcanic emissions |
| **CO** | mol/m² | Carbon Monoxide — combustion indicator |
| **O₃** | Dobson Units | Ozone concentration |
| **PM2.5** | µg/m³ | Fine particles — derived from AOD using physical models |
| **PM10** | µg/m³ | Coarse particles — derived from AOD |

> 💡 **Key concept:** Satellites do not directly measure PM2.5. They measure AOD (how aerosols interact with sunlight), which is then converted to PM2.5 using physical and statistical models.

---

## 🔴 CEP Engine — 8 Rules

| # | Rule | Condition | Threshold |
|---|------|-----------|-----------|
| 1 | AOD Spike | AOD ≥ 0.6 | High aerosol load |
| 2 | NO₂ Elevated | NO₂ ≥ 100 µmol/m² | Trace gas alert |
| 3 | SO₂ Industrial | SO₂ ≥ 40 µmol/m² | Industrial / volcanic source |
| 4 | CO Combustion | CO ≥ 0.07 mol/m² | Combustion event |
| 5 | PM2.5 Moderate | PM2.5 ≥ 35.4 µg/m³ | WHO guideline breach |
| 6 | PM2.5 Unhealthy | PM2.5 ≥ 55.4 µg/m³ | EU limit breach |
| 7 | **Compound Event** | AOD + NO₂ + PM2.5 all exceed thresholds | Multi-pollutant pattern |
| 8 | Severe Alert | AQI ≥ 150 | Unhealthy for all groups |

### Severity Classification

| Rules Fired | Severity Level |
|-------------|---------------|
| 0 | 🟢 NORMAL |
| 1 | 🟡 LOW_ALERT |
| 2 | 🟠 MODERATE_ALERT |
| 3 | 🔴 HIGH_ALERT |
| 4+ | 🚨 CRITICAL_EVENT |

---

## 📁 Project Structure

```
sat-cep-monitor/
│
├── sat_cep_monitor.py          # Main PySpark script (run locally)
├── SAT_CEP_Monitor_BDA.ipynb   # Google Colab notebook (19 cells)
└── README.md                   # This file
```

---

## ⚙️ Steps Implemented

| Step | Description | PySpark Feature Used |
|------|-------------|----------------------|
| 1 | Data Ingestion | `createDataFrame`, Schema |
| 2 | Preprocessing & Validation | `dropna`, `filter` |
| 3 | AQI Computation (US EPA) | `withColumn`, `when` |
| 4 | CEP Engine (8 Rules) | Conditional columns, aggregation |
| 5 | Event Detection Results | `groupBy`, `count` |
| 6 | CEP Rule Breakdown | Per-flag activation count |
| 7 | Regional Analysis | `groupBy`, `agg`, `avg`, `max` |
| 8 | Top Critical Events | `orderBy`, `limit` |
| 9 | Temporal Trend Analysis | `hour()`, hourly `groupBy` |
| 10 | **RDD Processing** | `map`, `reduceByKey`, `filter`, `flatMap` |
| 11 | **Window Functions** | `Window`, rolling avg, sustained detection |
| 12 | Sensor Performance | Multi-satellite comparison |
| 13 | Ground Station Validation | `join`, MAPE computation |

---

## 🚀 How to Run

### Option 1 — Google Colab (Recommended, Zero Setup)

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Click **File → Upload notebook**
3. Upload `SAT_CEP_Monitor_BDA.ipynb`
4. Click **Runtime → Run all**

The first cell installs PySpark automatically.

---

### Option 2 — Run Locally

**Prerequisites:**
- Python 3.x
- Java 8 or 11 (required by Spark)

**Install PySpark:**
```bash
pip install pyspark
```

**Run:**
```bash
python sat_cep_monitor.py
```

**Windows (if you see Python path errors):**
```cmd
set PYSPARK_PYTHON=python
set PYSPARK_DRIVER_PYTHON=python
python sat_cep_monitor.py
```

---

## 📈 Sample Output

```
=================================================================
   SAT-CEP-Monitor: Air Pollution Event Detector (PySpark)
=================================================================

  Total Readings Processed     : 500
  Total Pollution Events (CEP) : 83
    ↳ Critical Events          : 83
    ↳ Compound Multi-Pollutant : 83

  Global Average AQI           : 48.45
  Most Polluted Region         : Madrid
  Cleanest Region              : Rabat
```

---

## 🧠 Key Concepts (For Viva)

**How do satellites detect pollution?**
> Satellites measure how sunlight is altered by gases and particles in the atmosphere. Pollutants absorb specific wavelengths (NO₂ absorbs blue light, O₃ absorbs UV) and scatter light in predictable ways. These observations are converted into pollution indicators using radiative transfer equations and physical models.

**What is AOD?**
> Aerosol Optical Depth (AOD) measures how much aerosol particles in the atmosphere block sunlight. High AOD → polluted/dusty air. Low AOD → clean air. PM2.5 is estimated from AOD combined with weather data and ML models.

**Why is big data processing needed?**
> Satellite data is large-scale, heterogeneous, and arrives continuously as streams. Traditional systems cannot process such volumes in Near Real-Time. PySpark's distributed processing solves this.

**What is CEP?**
> Complex Event Processing (CEP) analyzes data streams in real-time to detect patterns across multiple parameters simultaneously. Instead of looking at single readings, CEP fires when combinations of conditions are met — e.g., AOD spike + NO₂ elevation + PM2.5 exceedance at the same time.

---

## 📚 Reference

> SAT-CEP-Monitor: An air quality monitoring software architecture combining complex event processing with satellite remote sensing. *(Morocco and Spain use case)*

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![PySpark](https://img.shields.io/badge/PySpark-3.5-orange?logo=apachespark)
![Colab](https://img.shields.io/badge/Google_Colab-Ready-yellow?logo=googlecolab)
![License](https://img.shields.io/badge/License-MIT-green)
