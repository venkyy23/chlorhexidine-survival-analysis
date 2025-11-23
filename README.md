

<p align="center">

  <!-- FULL BADGE COLLECTION -->
  <img src="https://img.shields.io/badge/Field-Clinical%20Data%20Science-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Domain-Survival%20Analysis-orange?style=for-the-badge">
  <img src="https://img.shields.io/badge/Method-Kaplan--Meier%20%7C%20Cox%20PH-green?style=for-the-badge">
  <img src="https://img.shields.io/badge/Data-Clinical%20Trial%20(VAP%20Study)-purple?style=for-the-badge">
  <img src="https://img.shields.io/badge/Source-Randomized%20Controlled%20Trial-red?style=for-the-badge">
  <img src="https://img.shields.io/badge/Python-3.10+-yellow?style=for-the-badge&logo=python">
  <img src="https://img.shields.io/badge/Libraries-lifelines%2C%20pandas%2C%20matplotlib-brightgreen?style=for-the-badge">
  <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge">

</p>

<hr>



# **Survival Analysis of Chlorhexidine Trial Outcomes Using Python** 🧪📈

This project is based on a real clinical trial case study titled  
**“Effectiveness of Oral Hygiene with Chlorhexidine Mouthwash with 0.12 percent and 0.2 percent Concentration on Incidence of VAP”**  
published in *Annals of International Medical and Dental Research, Vol (7), Issue (3) 2021*.  
The complete article is included in this repository as **Effectiveness of Oral Hygiene with Chlorhexidine Mouthwash.pdf**.  
(If needed locally use: `/data/Effectiveness of Oral Hygiene with Chlorhexidine Mouthwash.pdf`)

This repository reproduces and interprets **time-to-VAP (Ventilator-Associated Pneumonia)** outcomes using classical Survival Analysis methods in Python. All results, tables, and plots are generated via the **Chlorhexidine_Trials.ipynb**.

---

## **1️⃣ Project Title**  
**Survival Analysis of Chlorhexidine Trial Outcomes Using Python**

---

## **2️⃣ Project Summary** ✍️

This project analyses patient-level data from a randomized controlled trial comparing **0.12% vs 0.20% chlorhexidine** mouthwash for preventing Ventilator-Associated Pneumonia (VAP) in mechanically ventilated ICU patients.

- **Outcome:** time (days) until VAP (event = 1) with censoring for discharge, death, or LAMA (event = 0).  
- **Why survival analysis:** follow-up times vary and many patients are censored, so time-to-event methods are required.  
- **Learning outcomes:** Kaplan–Meier estimation, Log-Rank test, Cox Proportional Hazards modelling, Schoenfeld residuals, and clinical interpretation.

---

## **3️⃣ Dataset Description** 📚

- **Source:** Hospital-based randomized controlled trial, 140 patients randomized to two arms (0.12% vs 0.20% chlorhexidine).  
- **Working data:** cleaned dataset derived from `Raw Data form Chlorhexidine Trial.xlsx`.

### **Core variables**
| Column | Description | Type |
| ------ | ----------- | ---- |
| Age | Age in years | Continuous |
| Gender | Male / Female | Categorical |
| TrialArm_num | 1 = 0.12%, 2 = 0.20% | Categorical |
| APACHEII | Severity score | Continuous |
| TLC_D1 | Day-1 leukocyte count | Continuous |
| time | Time to VAP / censor | Continuous |
| event | 1 = VAP, 0 = NO VAP | Binary |

---

## **4️⃣ Problem Statement** ❓

Key clinical questions this project answers:

1. Does chlorhexidine 0.20% reduce the hazard of developing VAP compared to 0.12%?  
2. Is VAP-free survival different between the two treatment arms?  
3. Do baseline predictors — Age, APACHE II, TLC Day 1, Gender — influence time to VAP?  
4. Do survival curves differ by arm when tested with the Log-Rank test?  
5. What is the clinical interpretation of hazard ratios from a Cox PH model?

---

## **5️⃣ Objectives** 🎯

1. Data cleaning and preprocessing  
2. Exploratory Data Analysis (EDA) and baseline summary statistics  
3. Estimate survival curves (Kaplan–Meier) overall and by arm  
4. Compare groups using the Log-Rank test  
5. Fit a Cox Proportional Hazards model and report hazard ratios  
6. Check proportional hazards assumptions (Schoenfeld residuals)  
7. Produce clear visualizations and clinical interpretation

---

## **6️⃣ Methodology** 🛠️

### **6.1 Data Preparation**
- Column names cleaned (e.g., `APACHE II Score` → `APACHEII`, `TLC Day 1` → `TLC_D1`).  
- Encoded `TrialArm` and `Gender` to numeric/dummies for modelling.  
- Ensured `time` and `event` are numeric and suitable for survival objects.  
- **Model variables used:** `time`, `event`, `Age`, `APACHEII`, `TLC_D1`, `TrialArm_num`, `Gender_binary`.

**Handling missing values**  
- Missing values in `APACHEII` and `TLC_D1` were imputed using the **median** of each column.  
- Median was chosen because it is robust to outliers and preserves central tendency for skewed clinical measures.

---

### **6.2 Exploratory Data Analysis (EDA)** 🔍

The exploratory steps performed in the analysis include:

**Baseline summary**
- Total N = 106 (after dataset filtering), Events (VAP) = 10  
- Mean Age ≈ 47.6 (SD 17.3)  
- APACHEII mean ≈ 16.9 (SD 6.5)  
- TLC Day1 mean ≈ 15,216 (SD 7,270)  
- Arm1 n = 61, Arm2 n = 45  
- Male n = 93, Female n = 13  
- Mean follow-up time ≈ 5.7 days

**Visual exploration**
- **1. Age Distribution (Histogram)** – wide adult range, no extreme clustering.  
- **2. APACHE II Score Distribution (Histogram)** – most patients between 10–20.  
- **3. TLC Day 1 (Boxplot)** – elevated white cell counts with some high-value outliers, central tendency consistent with ICU inflammatory response.  

**Life tables**
- Life tables computed overall and per-arm show that early time points have few events; events appear mostly between days 5–10.  
- Arm 1 shows slightly higher cumulative hazard vs Arm 2.

**Event / Censoring structure**
- The dataset contains many censored observations (discharge, LAMA, death), which is expected for ICU treatment episodes.

**Survival visual checks**
- Kaplan–Meier curves (overall and by arm) with confidence intervals were plotted as part of EDA.

Purpose: confirm data structure, variable distributions, and readiness for survival modelling.

---

### **6.3 Survival Modelling**
Applied methods:
- Overall Kaplan–Meier survival curve  
- KM curves stratified by treatment arm  
- Log-Rank test (Arm 1 vs Arm 2)  
- Cox Proportional Hazards model (multivariable)  
- Schoenfeld residuals and PH assumption checks

---

## **7️⃣ Python Implementation Structure** 💻

.
├── data/
│   ├── Chlorhexidine Trials.xlsx
│   ├── Data_Dictionary.png
│   ├── Effectiveness of Oral Hygiene with Chlorhexidine Mouthwash.pdf
│   ├── Raw Data from Chlorhexidine Trial.xlsx
│
├── results/
│   ├── km_overall.png
│   ├── km_by_arm.png
│   ├── cox_summary.png
│   ├── cox_ph.png
│   ├── ph_age.png
│   ├── ph_apache2.png
│   ├── ph_gender.png
│   ├── ph_TLCD1.png
│   ├── ph_Trial_arm.png
│
├── Chlorhexidine_Trials.ipynb
├── LICENSE
├── README.md


## **8️⃣ Key Visualizations** 📊

- Life tables (overall and per-arm)  
- Overall Kaplan–Meier survival curve  
- Kaplan–Meier curves by treatment arm (with CI)  
- Schoenfeld residual plots for PH checks  
- Forest-style table of hazard ratios  

---

## **9️⃣ Results & Interpretation** 🧾

### **1. Kaplan–Meier Survival (Overall)**  
<div align="center">
  <img src="results/km_overall.png" width="600" alt="Overall KM">
</div>

Overall VAP-free survival remains high (≈ >90%) over the observed follow-up (≤10 days).

---

### **2. Kaplan–Meier by Trial Arm**  
<div align="center">
  <img src="results/km_by_arm.png" width="600" alt="KM by arm">
</div>

- Arm 1 (0.12%): more drops in the curve (7 VAP events)  
- Arm 2 (0.20%): flatter curve (2 VAP events)  
- Visual trend favors 0.20%.

---

### **3. Log-Rank Test**  
- **p = 0.94** — no statistical difference in time-to-VAP between arms.  
- Timing of events is similar despite numerical differences.

---

### **4. Cox Proportional Hazards (Multivariable)**  
<div align="center">
  <img src="results/cox_summary.png" width="600" alt="Cox summary">
</div>

- **TrialArm HR = 0.97 (p = 0.97)** → no detectable hazard difference between concentrations.  
- All predictors show HR ≈ 1 and are not statistically significant.  
- Concordance = 0.59.

---

### **5. Proportional Hazards Assumption**  
<div align="center">
  <img src="results/cox_ph.png" width="600" alt="PH checks">
</div>

- **All p > 0.05** → PH assumption holds for all variables.

---

## **🔟 Discussion** 💬

Both chlorhexidine concentrations maintained high VAP-free survival during observation.  
Although the 0.20% arm had fewer VAP events, the time-to-event patterns between groups were very similar, leading to no statistically significant difference.  
Cox modelling confirmed that baseline variables — age, APACHE II, TLC Day 1, gender, and trial arm — did not meaningfully change the hazard of VAP onset.  

These results align with clinical knowledge that chlorhexidine is effective in maintaining oral hygiene and reducing VAP risk, with both concentrations offering comparable protection within the short follow-up period.

---

## **1️⃣1️⃣ Conclusion** ✅

- Both chlorhexidine strengths support good VAP-free survival.  
- The 0.20% arm showed fewer raw events, but survival analysis did not show significant time-to-event differences.  
- No baseline predictor emerged as a meaningful driver of hazard.  
- Survival modelling provided structured insights into hazard behaviour and patient trajectories.

---

## **1️⃣2️⃣ Future Work** 🔭

- Add time-varying covariates.  
- Explore parametric survival models (Weibull, Exponential).  
- Apply machine-learning survival models (Random Survival Forests, DeepSurv).  
- Validate with external ICU datasets (e.g., MIMIC).  
- Consider competing risks models (VAP vs death).

---

### **Contact / Citation**
- Original trial paper: *Nagesh Vyas, Priya Mathur, Shailesh Jhawar, Akash Prabhune, Pradeep Vimal (2021).*  
- Notebook: `Chlorhexidine_Trials.ipynb`  
- Case study PDF path: `/data/Effectiveness of Oral Hygiene with Chlorhexidine Mouthwash.pdf`

---

**End of README**

