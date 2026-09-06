# Multi-Task vs. Independent DistilBERT for Automated CVSS v3.1 Base Metric Prediction

## Overview

This project investigates whether multi-task learning provides an advantage over independently trained models for automatically predicting the eight CVSS v3.1 Base Metrics from CVE vulnerability descriptions.

The study compares two approaches using DistilBERT:

1. **Multi-Task DistilBERT** – one shared DistilBERT encoder with eight separate classification heads for the CVSS Base Metrics.
2. **Independent DistilBERT** – eight separately trained DistilBERT models, with one model dedicated to each CVSS Base Metric.

Both approaches use the same dataset, preprocessing procedure, training configuration, and train/validation/test split to provide a fair comparison.

## Research Questions

The study addresses three research questions:

- **RQ1:** Does multi-task DistilBERT improve the prediction performance of the eight CVSS v3.1 Base Metrics compared with independently trained DistilBERT models?
- **RQ2:** Does the choice between multi-task and independent prediction affect the accuracy of the resulting CVSS severity-level classification?
- **RQ3:** How accurately can the complete CVSS v3.1 Base Vector be reconstructed from the predicted Base Metrics?

## Dataset and Preprocessing

The study uses CVE vulnerability descriptions and their corresponding CVSS v3.1 Base Vectors.

The preprocessing pipeline:

- Selects CVSS v3.1 vulnerabilities
- Cleans CVE descriptions and removes potential leakage information such as CVE IDs and IP addresses
- Retains descriptions containing **20–125 words**
- Randomly samples **20,000 CVEs** using a fixed random seed
- Parses each CVSS Base Vector into the eight Base Metrics:
  - Attack Vector (AV)
  - Attack Complexity (AC)
  - Privileges Required (PR)
  - User Interaction (UI)
  - Scope (S)
  - Confidentiality (C)
  - Integrity (I)
  - Availability (A)
- Splits the data into **80% training, 10% validation, and 10% test sets**

The same data split and preprocessing procedure are used for both approaches.

## Experimental Approaches

### 1. Multi-Task DistilBERT

A single DistilBERT encoder processes each CVE description. Its shared representation is passed to eight classification heads, one for each CVSS Base Metric.

The losses from the eight prediction tasks are averaged to form a joint training loss.

### 2. Independent DistilBERT

Eight separate DistilBERT sequence classification models are trained independently. Each model predicts only one CVSS Base Metric.

The same training configuration is used across the eight models.

## Evaluation

The approaches are evaluated at three levels.

### CVSS Base Metric Prediction

Each of the eight CVSS metrics is evaluated using:

- Accuracy
- Macro-Precision
- Macro-Recall
- Macro-F1

Confusion matrices are also used to analyze class-level prediction errors.

### CVSS Severity Classification

The eight predicted Base Metrics are combined into a complete CVSS v3.1 Base Vector. The corresponding CVSS Base Score and severity level are then calculated.

The four severity categories are:

- Low
- Medium
- High
- Critical

Severity classification is evaluated using Accuracy, Macro-Precision, Macro-Recall, and Macro-F1, together with class-level analysis.

### CVSS Base Vector Reconstruction

The predicted Base Metrics are combined to reconstruct the complete CVSS v3.1 Base Vector.

The reconstruction is evaluated using:

- **Exact Vector Accuracy** – all eight Base Metrics must be correct
- **Average Metrics Correct** – average number of correctly predicted Base Metrics per vulnerability
- **CVSS Base Score MAE** – average absolute error between predicted and ground-truth CVSS Base Scores

## Main Results

The independent DistilBERT approach achieves slightly better overall CVSS Base Metric prediction performance:

| Metric | Multi-Task | Independent |
|---|---:|---:|
| Accuracy | 84.92% | **85.21%** |
| Precision | 81.37% | **83.28%** |
| Recall | 75.53% | **76.28%** |
| F1 | 77.60% | **78.15%** |

For severity classification, Independent DistilBERT also performs slightly better overall.

For complete vector reconstruction, the two approaches perform very similarly. Independent DistilBERT has a small advantage in exact vector accuracy and average metrics correctly predicted, while Multi-Task DistilBERT achieves slightly lower CVSS Base Score MAE.

## Project Structure

The main Jupyter notebook contains the complete workflow:

1. Dataset exploration
2. Data cleaning and preprocessing
3. CVSS vector parsing
4. Dataset splitting
5. Multi-Task DistilBERT training
6. Independent DistilBERT training
7. CVSS Base Metric evaluation
8. Severity classification
9. CVSS Base Vector reconstruction
10. Comparison and analysis of the three research questions

## Requirements

The notebook uses Python and libraries including:

- PyTorch
- Hugging Face Transformers
- Hugging Face Datasets
- scikit-learn
- pandas
- NumPy
- matplotlib
- seaborn
- cvsslib

## How to Run

Open the Jupyter Notebook:

`Trends in NLP Poster Presentation.ipynb`

and execute the cells sequentially.

The notebook is designed to run in a Python/Jupyter environment with the required dependencies installed.

## Summary

This study provides a controlled comparison between multi-task and independent DistilBERT approaches for automated CVSS v3.1 prediction. Rather than evaluating only individual CVSS metrics, the study also investigates whether differences in metric prediction translate into differences in **vulnerability severity classification** and **complete CVSS vector reconstruction**.
