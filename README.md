# Multi-Task vs. Independent DistilBERT for Automated CVSS v3.1 Base Metric Prediction

## Overview

This project investigates whether multi-task learning provides an advantage over independently trained models for automatically predicting the eight CVSS v3.1 Base Metrics from CVE vulnerability descriptions.

The study compares two approaches using DistilBERT:

1. **Multi-Task DistilBERT** – one shared DistilBERT encoder with eight separate classification heads for the CVSS Base Metrics.
2. **Independent DistilBERT** – eight separately trained DistilBERT models, with one model dedicated to each CVSS Base Metric.

Both approaches use the same dataset, preprocessing procedure, training configuration, and train/validation/test split to provide a fair comparison.

## Main Results

The independent DistilBERT approach achieves slightly better overall CVSS Base Metric prediction performance:

| Metric | Multi-Task | Independent |
|---|---:|---:|
| Accuracy | 84.92% | **85.21%** |
| Precision | 81.37% | **83.28%** |
| Recall | 75.53% | **76.28%** |
| F1 | 77.60% | **78.15%** |
