<p align="center">

  <!-- FULL BADGE COLLECTION -->
  <img src="https://img.shields.io/badge/Field-Clinical%20Data%20Science-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Domain-Survival%20Analysis-orange?style=for-the-badge">
  <img src="https://img.shields.io/badge/Method-Kaplan--Meier%20%7C%20Cox%20PH-green?style=for-the-badge">
  <img src="https://img.shields.io/badge/Data-Clinical%20Trial%20(VAP%20Study)-purple?style=for-the-badge">
  <img src="https://img.shields.io/badge/Source-Randomized%20Controlled%20Trial-red?style=for-the-badge">
  <img src="https://img.shields.io/badge/Python-3.10+-yellow?style=for-the-badge&logo=python">
  <img src="https://img.shields.io/badge/Notebook-Jupyter-orange?style=for-the-badge&logo=jupyter">
  <img src="https://img.shields.io/badge/Libraries-lifelines%2C%20pandas%2C%20matplotlib-brightgreen?style=for-the-badge">
  <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge">

</p>

<hr>

# **Survival Analysis of Chlorhexidine Trial Outcomes Using Python** 🧪📈

This project is based on a real clinical trial titled  
**“Effectiveness of Oral Hygiene with Chlorhexidine Mouthwash with 0.12 percent and 0.2 percent Concentration on Incidence of VAP”**  
published in *Annals of International Medical and Dental Research (2021)*.  
The complete article is included as  
**/data/Effectiveness of Oral Hygiene with Chlorhexidine Mouthwash.pdf**.

This repository analyses **time-to-VAP (Ventilator-Associated Pneumonia)** using classical Survival Analysis methods in Python. All results, tables, and plots are generated via **Chlorhexidine_Trials.ipynb**.

---

## **1️⃣ Project Title**  
**Survival Analysis of Chlorhexidine Trial Outcomes Using Python**

---

## **2️⃣ Project Summary** ✍️

This project evaluates **0.12% vs 0.20% chlorhexidine** in preventing VAP in ventilated ICU patients.

- **Outcome:** Days until VAP (event = 1) or censoring (0).  
- **Need for survival analysis:** Follow-up varies and censoring is high.  
- **Methods used:** Kaplan–Meier curves, Log-Rank test, Cox PH model, Schoenfeld residual checks.

---

## **3️⃣ Dataset Description** 📚

- **Source:** Randomized controlled trial (140 patients).  
- **Working dataset:** Cleaned file `Raw Data from Chlorhexidine Trial.xlsx`.

### **Core Variables**
| Column | Description | Type |
| ------ | ----------- | ---- |
| Age | Years | Continuous |
| Gender | Male / Female | Categorical |
| TrialArm_num | 1 = 0.12%, 2 = 0.20% | Categorical |
| APACHEII | Severity score | Continuous |
| TLC_D1 | Day-1 total leukocyte count | Continuous |
| time | Time to VAP / censor | Continuous |
| event | 1 = VAP, 0 = NO VAP | Binary |

---

## **4️⃣ Problem Statement** ❓

1. Does 0.20% chlorhexidine reduce hazard of VAP compared to 0.12%?  
2. Are VAP-free survival curves significantly different between arms?  
3. Do Age, APACHE II, TLC Day 1, or Gender influence time-to-VAP?  
4. What does the Log-Rank test show?  
5. What do Cox PH hazard ratios indicate clinically?

---

## **5️⃣ Objectives** 🎯

1. Clean and preprocess data  
2. Perform EDA and baseline summaries  
3. Estimate KM curves (overall and by arm)  
4. Conduct Log-Rank comparison  
5. Fit Cox PH model  
6. Test PH assumptions  
7. Generate clear visualizations and interpretations  

---

## **6️⃣ Methodology** 🛠️

### **6.1 Data Preparation**

- Cleaned column names for compatibility.  
- Encoded TrialArm and Gender numerically.  
- Ensured `time` and `event` are numeric.  
- **Model variables:** `time`, `event`, `Age`, `APACHEII`, `TLC_D1`, `TrialArm_num`, `Gender_binary`.

**Handling Missing Values**  
- Median imputation for APACHEII and TLC_D1 (robust to skewed clinical values).

---

### **6.2 Exploratory Data Analysis (EDA)** 🔍

**Baseline Summary**
- N = 106, Events = 10  
- Mean Age ≈ 47.6  
- Mean APACHEII ≈ 16.9  
- Mean TLC Day1 ≈ 15,216  
- Arm1 = 61, Arm2 = 45  
- Male = 93, Female = 13  
- Average follow-up ≈ 5.7 days  

**Visual Exploration**
- **Age Histogram:** Wide adult distribution.  
- **APACHE II Histogram:** Most between 10–20.  
- **TLC Day1 Boxplot:** Elevated counts typical for ICU.

**Life Tables**
- Early time points show almost no events.  
- Most VAP events occur between days 5–10.

**Event / Censoring**
- Heavy censoring due to discharge, LAMA, or death.

**Survival Visual Checks**
- KM curves (overall + by arm) confirm readiness for modelling.

---

### **6.3 Survival Modelling**
- KM curves (overall and stratified)  
- Log-Rank test  
- Cox PH multivariable model  
- PH assumption testing (Schoenfeld residuals)

---

## **7️⃣ Python Implementation Structure** 💻

.
├── data/
│ ├── Chlorhexidine Trials.xlsx
│ ├── Data_Dictionary.png
│ ├── Effectiveness of Oral Hygiene with Chlorhexidine Mouthwash.pdf
│ ├── Raw Data from Chlorhexidine Trial.xlsx
│
├── results/
│ ├── cox_ph.png
│ ├── cox_summary.png
│ ├── km_by_arm.png
│ ├── km_overall.png
│ ├── ph_TLCD1.png
│ ├── ph_Trial_arm.png
│ ├── ph_age.png
│ ├── ph_apache2.png
│ ├── ph_gender.png
│
├── Chlorhexidine_Trials.ipynb
├── LICENSE
├── README.md

---


---

## **8️⃣ Key Visualizations** 📊

- KM curves (overall and by arm)  
- Life tables  
- Schoenfeld residual plots  
- Cox model summary  
- Forest-style HR table  

---

## **9️⃣ Results & Interpretation** 🧾

### **1. Kaplan–Meier Survival (Overall)**  
<div align="center">
  <img src="results/km_overall.png" width="600">
</div>

VAP-free survival remains above 90% during the observation window, reflecting low event count and early censoring.

---

### **2. Kaplan–Meier by Trial Arm**  
<div align="center">
  <img src="results/km_by_arm.png" width="600">
</div>

- Arm 1: more drops in survival  
- Arm 2: fewer events  
- Visual trend favors 0.20%, but statistical testing required.

---

### **3. Log-Rank Test**  
- **p = 0.94** → No statistically significant difference in survival curves.

---

### **4. Cox Proportional Hazards Model**  
<div align="center">
  <img src="results/cox_summary.png" width="600">
</div>

- TrialArm HR = **0.97**, p = **0.97** → No measurable hazard difference  
- Other predictors also insignificant (HR ≈ 1.0)  
- Concordance ≈ 0.59  

---

### **5. PH Assumption**  
<div align="center">
  <img src="results/cox_ph.png" width="600">
</div>

- All covariates satisfy PH assumption (p > 0.05).

---

### **Summary Table (Selected)**  
- Survival @ Day 5: Arm1 ≈ 0.919 | Arm2 ≈ 0.902  
- Events (filtered dataset): Arm1 = 6 | Arm2 = 4  
- TrialArm HR ≈ 0.97  

---

## **🔟 Discussion** 💬

Both chlorhexidine concentrations maintained high VAP-free survival. Although the 0.20% arm showed fewer events, the survival curves and hazard timelines were similar, which is reflected by the non-significant log-rank and Cox results.

The Cox model yielded HRs close to one for all predictors, and PH checks confirmed time-independent effects. These findings align with clinical expectations—chlorhexidine supports oral hygiene, and both concentrations performed similarly in this dataset.

Survival analysis added structure by evaluating timing, hazards, and group comparisons beyond simple event counts.

---

## **1️⃣1️⃣ Conclusion** ✅

- Both chlorhexidine strengths show high VAP-free survival.  
- No statistically significant difference between 0.12% and 0.20% in time-to-VAP.  
- No strong predictors of VAP identified.  
- Survival modelling effectively supports clinical interpretation.

---

## **1️⃣2️⃣ Future Work** 🔭

- Include time-varying covariates  
- Fit parametric survival models  
- Apply machine-learning survival methods  
- Validate on external ICU datasets  
- Explore competing-risk analysis  

---

### Contact / Citation  
- Original study: *Nagesh Vyas, Priya Mathur, Shailesh Jhawar, Akash Prabhune, Pradeep Vimal (2021).*  
- Notebook: `Chlorhexidine_Trials.ipynb`  
- PDF: `/data/Effectiveness of Oral Hygiene with Chlorhexidine Mouthwash.pdf`

---

**End of README**
