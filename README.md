---
title: "README"
author: "Sunee Rakotonindriana"
date: "2026-03-27"
output: html_document
---

```
COMPAS Recidivism Analysis — Project Overview

```

# COMPAS Recidivism Analysis - Full Python Implementation

This project replicates the full analytical workflow used in ProPublica’s COMPAS investigation, translated from R into Python. The goal is to evaluate whether the COMPAS risk‑assessment algorithm exhibits racial disparities in how it predicts recidivism. The analysis follows a structured pipeline:
- Load and clean the original COMPAS dataset
- Reproduce all preprocessing steps used in the R script
- Conduct exploratory data analysis to understand demographic and score distributions
- Fit a logistic regression model predicting high COMPAS scores
- Evaluate model performance using confusion matrices and error rates
- Compare false positive and false negative rates across racial groups
- Quantify disparities relative to Caucasian defendants
The final output mirrors the analytical insights of the R workflow while demonstrating how to implement the same methodology in Python.


```
Python Libraries used

```

# Python Library

The project relies on a set of core scientific‑computing and data‑analysis libraries:
- pandas — data cleaning, filtering, transformation, and tabulation
- numpy — numerical operations and vectorized calculations
- statsmodels — logistic regression modeling and statistical summaries
- matplotlib — visualization of distributions and score patterns
- seaborn — enhanced statistical plotting for EDA
- warnings — suppressing irrelevant output during model fitting


```
Reproducability

```
# Instructions for Reproducting Results

1. Set up your environment
Install the required Python packages if they are not already available:

pip install pandas numpy statsmodels matplotlib seaborn

2. Load the dataset
The analysis uses the publicly available COMPAS dataset hosted on GitHub. No local files are required.

3. Run the notebook
Each cell in the notebook corresponds to a step in the R workflow.
Running the notebook from top to bottom will reproduce all results.

4. Ensure reproducibility
The dataset is static, and the modeling procedure is deterministic. Running the notebook on any standard Python environment (Colab, Jupyter, VS Code, etc.) will yield the same outputs as long as:
- The dataset URL remains unchanged
- The Python libraries are up to date
- The notebook is executed in order without skipping cells

# Notebook Workflow Summary

## Dataset and Project Purpose
- Uses the publicly available COMPAS recidivism dataset from ProPublica:
  `https://raw.githubusercontent.com/propublica/compas-analysis/master/compas-scores-two-years.csv`
- Reproduces and extends the original COMPAS analysis in Python, including data cleaning, demographic exploration, model fitting, fairness measurement, and adversarial robustness evaluation.
- Focuses on whether COMPAS-style risk predictions exhibit racial and intersectional disparities, especially for African-American and other protected groups.

## Models Used
- **Logistic Regression**: interpretable baseline model fit with a standard scaler and one-hot encoding pipeline.
- **Gradient Boosted Trees**: a black-box tree-based model fit using `GradientBoostingClassifier` in the same preprocessing pipeline.
- Both models are trained and evaluated on the same feature set, including `priors_count`, `two_year_recid`, `race_factor`, `gender_factor`, `age_factor`, and `crime_factor`.

## Fairness Metrics Evaluated
- **False Positive Rate (FPR)** by race and race-vs-Caucasian disparity.
- **False Negative Rate (FNR)** by race.
- **Adverse Impact Ratio (AIR)** by race and intersectional race×gender subgroups.
- **Marginal Effect (ME)** relative to a reference group.
- **Standardized Mean Difference (SMD)** on predicted probabilities.
- **Slice-based evaluation** across race, gender, age, and offense type to identify subgroup performance gaps.

## Adversarial Machine Learning Components
- **PGD-style evasion audit**: tests bounded numeric perturbations and reports race-specific FPR and AIR under increasing epsilon values.
- **Label-flip poisoning**: simulates targeted poisoning on African-American or Caucasian training records and tracks AUC degradation, AIR degradation, stealth zones, and PSI detectability.
- **Membership inference analysis**: trains shadow models and attack classifiers to measure MI AUC for LR and GBT, compares MI risk to generalization gap, and evaluates confidence-gap behavior.
- **Regularization sweep**: varies logistic regression `C` values to illustrate the utility-privacy tradeoff between generalization and membership inference vulnerability.

## Key Findings and Observations
- Both models depend strongly on `priors_count`, making it a dominant risk driver.
- Logistic regression has smaller generalization gaps and more stable behavior than the gradient boosted tree.
- African-American defendants face substantially higher FPR than Caucasian defendants, consistent with known COMPAS fairness concerns.
- Intersectional analysis shows severe AIR disparities for subgroups such as `Other / Male`, highlighting issues invisible to single-attribute metrics.
- PGD evasion tends to amplify existing fairness gaps rather than reverse them, with LR showing more absolute FPR sensitivity and GBT remaining structurally biased.
- Label-flip poisoning can degrade fairness stealthily: AUC may remain stable while AIR changes significantly, and PSI drift detection can fail because only labels are altered.
- Increasing logistic regression regularization reduces membership inference risk, illustrating the privacy-performance tradeoff.

## Notes for Running the Notebook
- Install required packages before executing the notebook, including at least:
  `pandas`, `numpy`, `statsmodels`, `matplotlib`, `seaborn`, `scikit-learn`, `lime`, and `solas-ai`.
- Run `COMPAS_Analysis.ipynb` from top to bottom so preprocessing, model training, fairness metrics, and security analysis execute in sequence.
- The notebook is designed to work with the raw COMPAS CSV URL, so no local dataset file is required.
- For Colab or a fresh Python environment, use the in-notebook `%pip install` cells before running the analysis cells.
