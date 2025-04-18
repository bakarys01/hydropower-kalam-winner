# First Place Solution  
**15 Apr 2025, 16:44 · 11 min read**

---

## 1. Overview and Objectives
**Purpose:** This document describes a winning solution for the *IBM SkillsBuild Hydropower Climate Optimization Challenge*. The goal is to predict daily energy consumption for multiple consumer devices in Kalam, Pakistan. The solution applies comprehensive feature engineering, strategic data segmentation, and advanced ensemble modeling to handle various seasonal and climate-related complexities.

**Objectives and Expected Outcomes:**
1. **Accurate kWh Predictions**: Achieve low RMSE for daily consumption forecasts.
2. **Seasonality and Weather Integration**: Incorporate Pakistan's unique seasonal and weather factors.
3. **Robustness**: Effectively manage zero-consumption periods and device filtering.
4. **Efficient Inference Pipeline**: Offer a systematic approach from raw data transformation to final predictions.

---

## 🔥 Summary
This repository contains the winning solution for the IBM SkillsBuild Hydropower Climate Optimisation Challenge. It provides a complete end‑to‑end pipeline—from data preprocessing and feature engineering, through model training and ensemble optimization, to inference and submission file generation.

**Key Results**  
- **Private Leaderboard RMSE:** 4.312706649  
- **Final Rank:** 🥇 1st Place  

---

## 🧹 Data Preprocessing & Cleaning
- **Device Filtering:** Excluded consumer devices and users not present in the test set to avoid leakage and noise.  
- **Aggregation:** Converted raw 5‑minute readings into daily statistics (sum, mean, std, min, max) for kWh, voltage (red/blue/yellow), current, and power factor.  
- **Offline Period Detection:** Marked weeks with zero consumption across all days as "offline" and filtered them out to focus on meaningful usage patterns.  
- **Final Dataset:** Saved as `output/filtred_online_days_enriched.csv` containing only "online" days for the users in the test set.

---

## 🌦️ Feature Engineering (Comprehensive Climate Integration)
- **Climate Data Merge:** Joined in daily aggregates of temperature, dewpoint, precipitation, snowfall, snow cover, and wind components.  
- **Cyclical Temporal Features:** Sine/cosine encodings for day-of-week, day-of-year, month, week, and day-in-season.  
- **Pakistan‑Specific Features:** Flags for public holidays and Ramadan periods; season indicators tuned to Kalam's climate.  
- **Advanced Trends:** Exponential moving averages, volatility, acceleration, and extreme‑event indicators for temperature.  
- **Heating/Cooling Metrics:** Heating degree days, cooling degree days, and temperature‑dewpoint differentials.  
- **Interactions:** Temperature × weekday interactions and weather × consumption effects.

---

## 🧪 Strategic Data Segmentation
Divided into four temporal segments to capture distinct seasonal behaviors:

| Segment | Periods                                 | Rationale                                            |
| ------- | --------------------------------------- | ---------------------------------------------------- |
| **Data1** | Aug–Sep 2024 & Oct 2023 (late summer/early fall) | High consumption → model learns peak usage patterns  |
| **Data2** | Nov–Dec 2023 & Jul 2024 (winter & mid-summer)    | Moderate consumption periods                         |
| **Data3** | All other intervals                         | Mostly zero/minimal consumption                      |
| **Data4** | Entire dataset                              | Global model for overarching patterns                |

---

## 🧠 Advanced Ensemble Modeling
1. **Seven LightGBM Configurations** per segment:
   - **Precise:** deep, conservative trees (max_depth=8).  
   - **Feature‑Selective:** aggressive feature sampling.  
   - **Robust:** outlier‑resistant (min_data_in_leaf=20).  
   - **Deep Forest:** very deep with many estimators.  
   - **Highly Regularized:** strong L1/L2 penalties.  
   - **Fast Learner:** high learning rate for rapid convergence.  
   - **Balanced:** tuned for bias‑variance tradeoff.  
2. **Cross‑Validation:** 5‑fold CV to evaluate each base model.  
3. **Bayesian Optimization:** Search optimal ensemble weights per fold instead of a meta‑model.  
4. **Multi‑Level Blending:** Combine segment‑specific ensembles with global weighting.

---

## 🔗 Dataset Access via Kaggle
Due to GitHub file size limits, raw CSV/XLSX files are **not** included here. Please download from Kaggle:

1. Visit:  
   👉 [IBM SkillsBuild Hydropower Climate Optimisation (Updated)](https://www.kaggle.com/datasets/muhammadqasimshabbir/ibmskillsbuildhydropowerclimateoptimisationupdated)
2. Click **Download All**.  
3. Unzip and place into this repo under:
   ```
   datasets/
   ├── Data/
   │   └── Data.csv
   ├── SampleSubmission.csv
   └── Climate Data/
       └── Kalam Climate Data.xlsx
   ```

---

## 📂 Repository Structure
```
.
├── datasets/                           # Kaggle data (not committed)
├── output/                             # Generated data, models, plots
├── first_place_solution.ipynb          # Full notebook with code & docs
├── final_submission.csv                # Submission file matching leaderboard
├── requirements.txt                    # Python dependencies
└── README.md                           # This documentation
```

---

## 🛠️ How to Run
1. Clone the repo  
2. Download & place the dataset as described above  
3. Create and activate a Conda/Python environment:  
   ```bash
   pip install -r requirements.txt
   ```  
4. Open `first_place_solution.ipynb` in Jupyter/Colab  
5. Run cells top to bottom—full training takes ~25–30 minutes  

---

## 📈 Performance Metrics
- **Public Leaderboard RMSE:** your public score  
- **Private Leaderboard RMSE:** 4.312706649  
- **Ensemble Improvement:** Bayesian weighting improved RMSE by ~4.2%

---

## 🕒 Run Time
- **Data preprocessing & feature engineering:** ~3–5 minutes  
- **Model training (4 segments × 7 configs × 5‑fold CV):** ~20–25 minutes  
- **Inference & submission export:** < 1 minute  

---

## ❓ Additional Notes
- **Reproducibility:** `seed_everything(42)` ensures consistent results.  
- **Logging:** Comprehensive logging for each stage (ETL, training, inference).  
- **Error Handling:** Fallback models in case of training errors; robust NaN column filtering.  

---

## 📞 Contact
Bakary Sidibé  
✉️ bakarysidibe1995@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/bakary-sidibe-256419111/)  

---

*Thank you for reviewing this solution!*