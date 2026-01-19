# UIDAI-Data-Hackathon
**Unlocking Societal Trends in Aadhaar Enrolment and Updates (UIDAI Hackathon 2026)**  
A data-driven analytics project that identifies meaningful patterns, trends, hotspots, anomalies, and forecasting indicators from Aadhaar enrolment and update activity datasets.

---

## 📌 Overview
Aadhaar enrolment and update operations generate large-scale activity data across time and geography. This project analyzes UIDAI-provided Aadhaar datasets to:

- understand **activity trends** over time
- identify **high-demand hotspots** (state / district / PIN code level)
- compute **Update Pressure Ratio** to quantify update intensity
- perform **anomaly detection** on activity spikes/drops
- generate **short-horizon forecasts** for future service load

The project is designed to run efficiently on limited compute resources (Google Colab / laptop) using **chunk-wise reading** and **stratified sampling** of large CSV files.

---

## 🎯 Problem Statement
**Identify meaningful patterns, trends, anomalies, or predictive indicators from Aadhaar enrolment and update datasets, and translate them into actionable insights to support informed decision-making and system improvements.**

---

## 📂 Repository Contents

UIDAI-Data-Hackathon/
│── README.md
│── aadhaar_hackathon.ipynb # Complete end-to-end notebook (Colab compatible)


---

## 🗂️ Dataset Details
This project uses UIDAI Aadhaar activity datasets:

- **Enrolment Dataset** (`enrolment.csv`)
- **Demographic Update Dataset** (`demographic.csv`)
- **Biometric Update Dataset** (`biometric.csv`)

All datasets share the same schema:

| Column         | Description                             |
|----------------|-----------------------------------------|
| `date`         | Date of activity                        |
| `state`        | State/UT name                           |
| `district`     | District name                           |
| `pincode`      | PIN code                                |
| `bio_age_5_17` | Activity count for age group 5–17       |
| `bio_age_17_`  | Activity count for age group 17+        |

<img width="1005" height="244" alt="image" src="https://github.com/user-attachments/assets/a3f8c60e-9933-44bb-970a-d45da405ced1" />


### Standardisation (Feature Engineering)
For analysis consistency:
- `bio_age_5_17` → `age_5_17`
- `bio_age_17_` → `age_17_plus`
- `total_activity` = `age_5_17 + age_17_plus`

---

## 🧠 Methodology
### 1) Big Data Handling
Since the raw datasets may contain **500k+ rows**, the project implements:

- **Chunk-wise CSV reading** (`chunksize=50,000`)
- First pass: compute **top activity states**
- Second pass: **sample a fixed number of rows** from top states
- Optional: reduce to most frequent PIN codes to preserve hotspot signals

This preserves meaningful patterns while preventing memory overflow.

### 2) Exploratory Data Analysis (EDA)
- temporal trends (daily/monthly totals)
- enrolment vs updates comparison
- age group participation patterns
- district and PIN code hotspots

### 3) Advanced Analytics
- **Update Pressure Ratio** (state level)
  \[
  UpdatePressure = \frac{DemographicUpdates + BiometricUpdates}{Enrolments}
  \]
- **Anomaly Detection** using `IsolationForest` on time-series totals
- **Improved Forecasting** (outlier handling + smoothing + log transform regression) to avoid unrealistic negative forecasts

---

## 📊 Key Visualizations Produced
The notebook generates and saves charts (PNG) such as:

- Aadhaar activity trends over time (by record type)
- <img width="2047" height="1093" alt="monthly_trend_by_type" src="https://github.com/user-attachments/assets/e12f89b0-2def-446f-91f7-07c62cd25a97" />

- Top states / top districts comparison
- <img width="2047" height="1247" alt="top_states_activity" src="https://github.com/user-attachments/assets/a08d6084-982c-4042-95d2-0d31b495e8e7" />

- PIN code hotspot ranking

 Update Pressure Ratio across states
- <img width="2400" height="1200" alt="update_pressure_states" src="https://github.com/user-attachments/assets/2dbcd25d-738f-48db-9a90-873d9ea9385d" />

- Adult share (17+) patterns across states
- Anomaly detection plot
- <img width="2008" height="1054" alt="anomaly_detection" src="https://github.com/user-attachments/assets/6bba5fe7-44d0-451d-97ea-275f42750feb" />

- Improved forecast plot for next 7 days

---

## ✅ Key Findings (Highlights)
- Aadhaar activity is **highly concentrated** in specific states and districts.
- **PIN code hotspots** reveal micro-regions with disproportionate service demand.
- <img width="527" height="201" alt="Screenshot 2026-01-16 233359" src="https://github.com/user-attachments/assets/8e3dba62-282c-46f1-95c1-dfc58f2210ff" />

- Several states show **high update pressure**, indicating significant identity maintenance workload beyond new enrolments.
- **Anomaly days** were detected (spikes/drops) that may align with operational surges, campaigns, or system-level disruptions.
- Forecasting demonstrates how Aadhaar load can be **predicted short-term** for staffing and scheduling support.

---

## 💡 Recommendations (Administrative Applicability)
- Prioritize staffing and infrastructure in hotspot districts and PIN codes.
- Deploy temporary/mobile service units in high-demand regions.
- Use update pressure to plan resources for recurring Aadhaar maintenance workload.
- Integrate anomaly alerts into monitoring systems for early operational response.

---

## ⚙️ How to Run (Google Colab)
1. Open the notebook: `aadhaar_hackathon.ipynb`
2. Mount Google Drive
3. Place CSV files inside Drive folder:

