# Pediatric-Asthma-Readmission-Analysis

## Project Overview
This repository contains a survival analysis investigating factors associated with time to hospital readmission in children with asthma. The analysis examines differences between three hospitals and explores how patient characteristics (age, sex, previous admissions, etc.) influence readmission risk.

---

## Files Included

- **asthma_analysis_report.docx** – Complete analysis report with tables and graphs  
- **asthmaData.dta** – Stata dataset containing 1031 pediatric asthma patient records  
- **README.md** – This file  

---

## Data Description

The dataset includes **1031 children hospitalized for asthma**, with follow-up data on readmission outcomes.

### Variables

- **time** – Time to readmission or censoring (days)  
- **readmit** – Indicator (1 = readmitted, 0 = censored)  
- **hospital** – Hospital identifier (1, 2, or 3)  
- **age** – Patient age (years)  
- **sex** – Patient sex (boy/girl)  
- **wheezes** – Wheezing episodes count  
- **admiss** – Number of previous admissions  
- **resp** – Respiratory rate  

---

## Key Findings

### Hospital Differences
Significant variation exists between hospitals.

- **Log-rank test:** χ² = 37.36, p < 0.001  
- **Hospital 2:** markedly higher readmission risk  
  - Unadjusted analysis: **HR = 1.99**  
  - Fully adjusted model: **HR = 4.76**

### Age Interaction
The hospital effect varies with age.  
The higher risk associated with **Hospital 2 decreases as children get older**.

- **Interaction HR = 0.906**, p = 0.031

### Other Risk Factors

- **Older age:** Protective effect (HR = 0.86, p < 0.001)  
- **Male sex:** Lower risk (HR = 0.81, p = 0.018)  
- **Previous admissions:** Increased risk (HR = 1.05, p < 0.001)  
- **Wheezing and respiratory rate:** Not significant in multivariable model  

---

## Replicating the Analysis

1. Open the `.dta` file in **Stata**.
2. Run the commands listed in the Appendix of the report, or follow the workflow below.

```stata
* Load data
use "asthma_data.dta", clear

* Set survival data
stset time, failure(readmit==1)

* Question 1: Descriptive and unadjusted analysis
stsum, by(hospital)
sts graph, by(hospital) risktable
sts test hospital
stcox i.hospital
estat phtest, detail

* Question 2: Age interaction
stcox i.hospital##c.age

* Question 3: Multivariable model
stcox i.hospital i.sex age wheeze admiss resp
