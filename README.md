# 🏥 Modeling Doctor Complaints with Zero-Inflated Negative Binomial Regression

This project was completed as part of the [**STAT2402: Analysis of Observations**](https://handbooks.uwa.edu.au/unitdetails?code=STAT2402) assignment at the University of Western Australia. The goal was to model the number of complaints received by emergency doctors based on their workload and demographics, using appropriate count regression techniques.

> **Note:** This README provides a summary of the project and key findings.
> For full analysis and conclusions, see `Analysis Report.pdf`.
> All analysis code can be found in `script.Rmd`.

## 📊 Project Overview

- **Objective**:
  To investigate which factors influence the number of complaints received by emergency doctors, and to build a statistical model that best describes the relationship.

- **Dataset (`compdat.txt`)**:
  Contains information for 94 emergency department doctors:

  - `complaints`: number of complaints received in the past year (response)
  - `visits`: number of patient visits
  - `residency`: whether the doctor is in residency training (`Yes` / `No`)
  - `gender`: gender of the doctor (`Male` / `Female`)
  - `revenue`: hourly income in dollars
  - `hours`: total work hours in a year

## 🔍 Methodology

- **Model Choice Justification**:

  - Started with a **Poisson model**, but tests revealed **overdispersion** and **zero-inflation**.
  - Tried **quasi-Poisson** and **Negative Binomial** models, which improved fit but did not fully account for excess zeros.
  - Final model selected was a **Zero-Inflated Negative Binomial (ZINB)** model, ideal for count data with overdispersion and excess zeros.

- **Modeling Process**:

  - Stepwise model simplification using `stepAIC()`
  - Explored meaningful interaction terms (e.g., `gender × residency`, `visits × revenue`)
  - Evaluated model fit using AIC, residual diagnostics, and rootograms

## 🧾 Final Model

The final model was a **Zero-Inflated Negative Binomial (ZINB)** with:

- **Count Model**:

  $$
  \log(\lambda) = 1.243 + 0.0017 \cdot \text{visits} + 0.3298 \cdot \text{gender}_{\text{Female}} - 0.0100 \cdot \text{revenue} - 0.0016 \cdot \text{hours} + 0.6332 \cdot (\text{Male} : \text{No}) - 0.5002 \cdot (\text{Female} : \text{No})
  $$

- **Zero-Inflation Model**:

  $$
  \log\left(\frac{p}{1 - p}\right) = -0.0545 + 1.6000 \cdot \text{visits} + 1.6330 \cdot \text{revenue} - 2.4640 \cdot \text{hours} - 0.0058 \cdot (\text{visits} \cdot \text{revenue}) + 0.0091 \cdot (\text{revenue} \cdot \text{hours})
  $$

## 📈 Results Summary

- **Significant Predictors**:

  - **More visits** ⟶ more complaints
  - **Higher revenue** and **longer hours** ⟶ fewer complaints
  - **Male non-residents** ⟶ higher complaint rates
  - **Interaction effects** captured complex relationships, improving model fit

- **Model Diagnostics**:

  - Plots of residuals show no major biases
  - ZINB model successfully captured overdispersion and zero-inflation

## ⚠️ Limitations

- Small sample size (n = 94) may limit statistical power
- No direct measure of patient complexity or case severity
- Dataset limited to one hospital; may not generalize to other settings
- Potential reporting bias in complaints data

## 📁 Folder Structure

```
ZINB/
│
├── Analysis Report.pdf         #
├── Finals2.Rmd                 # RMarkdown script with all code, model outputs, and plots
├── compdat.txt                 # Dataset with doctor demographics and complaint counts
├── README.md                   # Project summary and documentation (this file)
├── diagrams/
│   ├── boxplot.png             # Boxplots comparing gender and residency with income/hours
│   ├── geombar.png             # Bar plot: complaints by residency and gender
│   └── zinb.png                # Residual diagnostics for the ZINB model
```
