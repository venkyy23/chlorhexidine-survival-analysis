

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
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge">


</p>

<hr>



# **Survival Analysis of Chlorhexidine Trial Outcomes Using Python** 🧪📈

This project is based on a real clinical trial case study titled  
**“Effectiveness of Oral Hygiene with Chlorhexidine Mouthwash with 0.12% and 0.2% Concentration on Incidence of VAP”**  
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
- Event rate = 9.4 percent (10 of 106 patients)

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

📁 Project Structure

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
│   ├── cox_forest.png
│   ├── nelson_overall.png
│   ├── nelson_by_arm.png
│
├── Chlorhexidine_Trials.ipynb
├── LICENSE
└── README.md


## **8️⃣ Key Visualizations** 📊

- Life tables (overall and per-arm)  
- Overall Kaplan–Meier survival curve  
- Kaplan–Meier curves by treatment arm (with CI)  
- Schoenfeld residual plots for PH checks  
- Forest-style table of hazard ratios
- 

---

## **9️⃣ Results & Interpretation** 🧾

### **1. Kaplan–Meier Survival (Overall)**  
<div align="center">
  <img src="results/km_overall.png" width="600" alt="Overall KM">
</div>

- The Kaplan Meier curve shows high VAP free survival, starting at 1.00 and remaining above 0.90 through most of the 10 day follow up period.
- Small drops occur around days 3 to 6, reflecting only 10 total VAP events out of 106 patients.
- The final survival probability at day 10 is approximately 0.78, indicating that most patients remained VAP free.
- The narrow shaded confidence band early on and wider band later reflect increasing uncertainty as fewer patients remain under observation.

---

### **2. Kaplan–Meier by Trial Arm**  
<div align="center">
  <img src="results/km_by_arm.png" width="600" alt="KM by arm">
</div>

- Both arms maintain high VAP free survival, remaining above 0.90 through most of the 10 day follow up.
- Arm 1 ends with a survival probability around 0.78, while Arm 2 remains slightly higher at around 0.92, though both curves overlap heavily.
- Only 10 total events occurred, causing wide confidence bands and limiting precision in comparing arms.
- The overall log rank p value of 0.94 indicates no statistically meaningful difference in VAP free survival between the two concentrations.

---

### **3. Log-Rank Test**  
- **p = 0.94** — no statistical difference in time-to-VAP between arms.  
- Timing of events is similar despite numerical differences.

---

### **4. Cox Proportional Hazards (Multivariable)**  

<div align="center">
  <img src="results/cox_summary.png" width="600" alt="Cox summary">
</div>

- The **Cox Proportional Hazards** reports the fitted model results, including hazard ratios, confidence intervals, and p-values. 
- All predictors (Age, APACHE II, TLC Day 1, Gender, TrialArm) show HR values close to 1 and p > 0.45, indicating no significant effect on VAP hazard.
- *TrialArm_num HR = 0.97* (95 percent CI 0.24 to 3.89, p = 0.97) indicates no measurable difference in hazard between the two interventions.  
- Only 10 events among 106 patients lead to wide confidence intervals, reducing precision of estimates.
- Model concordance is 0.59, suggesting weak predictive ability due to the low event rate.
  
---

### **5. Proportional Hazards Assumption**  
<div align="center">
  <img src="results/cox_ph.png" width="600" alt="PH checks">
</div>

- The **Proportional Hazards Assumption** evaluates whether the proportional hazards assumption holds. All variables have p > 0.05, meaning their effects remain constant over time and the model is valid.  
- the Cox Summary describes *what the model found*, while the PH Test confirms *whether those results can be trusted*.
- All covariates have KM p > 0.26 and rank p > 0.09, indicating no violation of the proportional hazards assumption.
- APACHEII (p = 0.91), Age (p = 0.97), TLC_D1 (p = 0.44), and Gender (p = 0.26) show stable hazard patterns over time.
- TrialArm_num shows the lowest p value (KM p = 0.05, rank p = 0.09) but still does not cross the significance threshold.
- Since all PH tests are > 0.05, the Cox model assumptions are satisfied.
- The PH test confirms that the model’s estimated effects are valid and time-independent.

---

### **6. Forest plot for Cox proportional hazards model**  
<div align="center">
  <img src="results/cox_forest.png" width="600" alt="Cox forest">
</div>

- The **Cox proportional hazards model** shows no clear or precisely estimated treatment effect. 
- The hazard ratio for the trial arm is close to 1 with a confidence interval crossing 1, indicating no detectable change in hazard.
- Other covariates, especially Gender, display wide confidence intervals, reflecting high uncertainty and limited event information.
- Overall, the model suggests small effect sizes and imprecise estimates due to low event counts.

---
### **7. Nelson Aalen cumulative hazard plots**  
<div align="center">
  <img src="results/nelson_overall.png" width="600" alt="Nelson Overall">
</div>

<div align="center">
  <img src="results/nelson_by_arm.png" width="600" alt="Nelson By Arm">
</div>

- **Nelson Aalen cumulative hazard plots** reveal that events are few and occur in clusters.
- The curves rise in small, sudden steps, showing that only a few VAP events occurred and they were clustered at specific days.
- Both treatment arms have almost identical cumulative hazard levels across the timeline.
- Neither arm shows a consistently higher or lower hazard pattern.
- The overall interpretation is that event rates are low and the two arms behave similarly in terms of accumulated risk.
  
---

## **🔟 Discussion** 💬

Both chlorhexidine concentrations maintained high VAP-free survival during observation.  
Although the 0.20% arm had fewer VAP events, the time-to-event patterns between groups were very similar, leading to no statistically significant difference.  
Cox modelling confirmed that baseline variables — age, APACHE II, TLC Day 1, gender, and trial arm — did not meaningfully change the hazard of VAP onset.  

These results align with clinical knowledge that chlorhexidine is effective in maintaining oral hygiene and reducing VAP risk, with both concentrations offering comparable protection within the short follow-up period.

---

## **1️⃣1️⃣ Conclusion** ✅

- Both chlorhexidine concentrations maintained high VAP free survival across the 10 day follow up.
- The 0.20 percent arm showed fewer raw VAP events, but Kaplan Meier curves and the log rank test (p = 0.94) demonstrated no meaningful difference in time to VAP between groups.
- Cox modelling confirmed that baseline predictors including age, APACHE II, TLC Day 1, gender and trial arm had hazard ratios near 1 with no significant effects.
- The overall pattern indicates low event rates and comparable risk profiles for both concentrations within the short observation window.

---

## **1️⃣2️⃣ Future Work** 🔭

- Add time-varying covariates.  
- Explore parametric survival models (Weibull, Exponential).  
- Apply machine-learning survival models (Random Survival Forests, DeepSurv).  
- Validate with external ICU datasets (e.g., MIMIC).  
- Consider competing risks models (VAP vs death).

---

### **Citation**
- Original trial paper: *Nagesh Vyas, Priya Mathur, Shailesh Jhawar, Akash Prabhune, Pradeep Vimal (2021).*  
- Notebook: `Chlorhexidine_Trials.ipynb`  
- Case study PDF path: `/data/Effectiveness of Oral Hygiene with Chlorhexidine Mouthwash.pdf`

---

### **License**
- This project is licensed under the MIT License.
- You are free to use, modify, distribute, and build upon this work, provided that the original license is included with any copies or substantial portions of the software.
- View Full License: [MIT License](LICENSE)
<br>
**End of README**

