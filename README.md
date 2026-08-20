# 🎬 AMC Revenue Forecasting Tracker: Box-Office Proxy & WLS Modeling

![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat&logo=python&logoColor=white)
![Statsmodels](https://img.shields.io/badge/Statsmodels-WLS-blue)
![Validation](https://img.shields.io/badge/LOOCV_R²-0.93-brightgreen)

## 📌 Executive Summary
This repository presents an empirical financial forecasting model designed to predict **AMC Entertainment’s Q2 2026 quarterly revenue** prior to official earnings announcements. 

To overcome severe historical noise (e.g., post-pandemic recovery, streaming windows) and low-sample constraints ($N=16$), this project leverages web-scraped **Box Office Mojo proxy data** combined with macroeconomic indicators (CPI) within a **Time-Weighted Weighted Least Squares (WLS)** framework.

### 🎯 Key Results & Forecast
* **Q2 2026 Forecasted Revenue:** **$1.54B**
* **Model Fit ($R^2$):** **0.96**
* **Out-of-Sample Validation (LOOCV $R^2$):** **0.93** (Demonstrates robust generalization capability without overfitting)

---

## 🛠 Methodology & Analytical Pipeline

### 1. Feature Engineering & Web Scraping
* **Box-Office Ingestion:** Scraped daily domestic and global box-office grosses from Box Office Mojo and aligned them with AMC’s fiscal quarters.
* **Proxy Construction:** Engineered domain-specific proxy metrics, notably `Average_Gross_Per_Theater`, to capture theater-level productivity.
* **Macro Integration:** Combined macro-economic sentiment using `CPI_Quarter` to control for broader inflationary pressures.

### 2. Model Optimization & Feature Diagnostics (RFE & Model Diet)
* **Iterative Feature Selection:** Evaluated feature sets using Recursive Feature Elimination (RFE).
* **Noise Reduction (Model Diet):** Eliminated statistically non-significant variables (e.g., `BlockBuster_Dominance_Ratio` with $p = 0.115$ and counter-intuitive negative coefficient) to prevent variance inflation.
* **Final Feature Set:** `Average_Gross_Per_Theater` ($p < 0.001$), `Movie_Count_Quarter` ($p = 0.002$), and `CPI_Quarter` ($p < 0.001$).

### 3. Time-Weighted WLS & LOOCV
* **Time-Variant Weighting:** Implemented geometrically decaying weights to dynamically prioritize recent post-pandemic market dynamics over stale historical trends.
* **LOOCV Validation:** Applied Leave-One-Out Cross Validation across 16 rotations, securing a **93% validation $R^2$**, proving high stability under low-sample environments ($N=16$).

---

## 🔍 Model Limitations & Blind Spots

* **Omitted Variable Bias:** Unquantifiable external factors (e.g., weather anomalies, OTT streaming exclusives, and major sporting events) remain uncaptured in box-office proxies.
* **Sample Size Sensitivity ($N=16$):** Extreme historical outliers can exert disproportionate leverage on regression coefficients.
* **Auxiliary Revenue Blind Spots:** Focuses primarily on ticket sales proxy, under-representing high-margin concession spending (food & beverage per-capita).

---

## 🚀 Operational Scalability (Future Trackers)

1. **Automated Data Pipeline:** Real-time ingestion of weekly box-office grosses for continuous mid-quarter revenue tracking.
2. **Early Warning System:** Automated signal generation comparing real-time forecasts against Wall Street consensus 3–4 weeks ahead of earnings calls.
3. **Alternative Data Integration:** Fusing per-capita concession spending metrics to build an end-to-end cinema industry forecasting suite.

---

## 📁 Repository Structure

```text
├── data/                  # Scraped Box Office Mojo & CPI datasets
├── notebooks/             # Jupyter Notebooks (Data Scraping, Feature Engineering, WLS Model)
├── reports/               # Full 20-Page Comprehensive Write-up (PDF)
├── src/                   # Python scripts for data processing and modeling
└── README.md              # Project Overview
