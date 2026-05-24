# Seizure Detection — CHB-MIT EEG Dataset

A machine learning pipeline that classifies seizure activity from EEG recordings, distinguishing **ictal** (seizure) from **preictal** (pre-seizure) states. Built on the CHB-MIT Scalp EEG Database.

---

## Overview

Epileptic seizures produce distinctive patterns in EEG signals. This project builds an end-to-end pipeline — signal filtering, feature extraction, dimensionality reduction, and classification — to automatically detect those patterns. The goal is a model that reliably separates seizure from pre-seizure activity, a task with direct relevance to clinical seizure-warning systems.

**Pipeline at a glance:**

```
Raw EEG  →  Filtering  →  Filter Banks  →  Feature Extraction
             →  Standardization  →  PCA + Feature Selection  →  SVM  →  Prediction
```

---

## Dataset

The [CHB-MIT Scalp EEG Database](https://physionet.org/content/chbmit/) contains EEG recordings from **24 participants** across **23 electrode channels**. In the preprocessed version used here, the final column is the label:

| Label | State | Meaning |
|:-----:|:------|:--------|
| `0` | Preictal | Period preceding a seizure |
| `1` | Ictal | Active seizure |

**Download:** the preprocessed CSV is available [here](https://ieee-dataport.org/open-access/chb-mit-preprocessed-data) (IEEE DataPort). Place it in a `data/` directory before running the pipeline.

> **Note:** the original signed S3 link expires after one hour. Download directly from IEEE DataPort or PhysioNet instead.

---

## Preprocessing

1. **Filtering** — the functional frequency range for seizure detection is 2–20 Hz. Following common practice in the literature, we apply a bandpass filter with corner frequencies of **0.5 Hz and 36 Hz** to remove drift and high-frequency noise while preserving the bands of interest.
2. **Filter banks** — after the initial filter, we isolate each clinically relevant frequency band for separate analysis:

   | Band | Range (Hz) |
   |:-----|:----------:|
   | Delta | 0.5 – 4 |
   | Theta | 4 – 8 |
   | Alpha | 8 – 13 |
   | Beta | 13 – 30 |
   | Gamma | 30 – 36 |

3. **Standardization** — features are scaled to zero mean and unit variance. *(Applied after feature extraction.)*

---

## Feature Extraction

We extract features drawn from the EEG literature and established Python libraries:

- **Frequency-domain:** power spectral density, peak frequency, median frequency.
- **Time-domain (statistical):** mean, variance, skewness, kurtosis.
- **Library-based:** features from [PyEEG](https://github.com/forrestbao/pyeeg) (e.g. Hjorth parameters, spectral entropy, fractal dimension).

---

## Dimensionality Reduction & Feature Selection

The high dimensionality of the extracted feature set is reduced before classification using:

- **Principal Component Analysis (PCA)** for dimensionality reduction, combined with
- a **filter-based selection method**. Candidate methods under evaluation:
  1. Mutual information
  2. Univariate statistical tests (e.g. Wilcoxon, t-test)

> Final dimensionality is assessed empirically before fixing the selection strategy.

---

## Classification

We use a **Support Vector Machine (SVM)** — one of the most widely used classifiers in the seizure-detection literature, and computationally efficient for this feature space.

---

## Results

<!-- ===== FILL THIS IN — it's the most important section for an ML portfolio. ===== -->
<!-- Report your real numbers. Sensitivity matters most clinically (missing a seizure is worse than a false alarm). -->

| Metric | Score |
|:-------|:-----:|
| Accuracy | _e.g. 0.94_ |
| Sensitivity (Recall) | _e.g. 0.91_ |
| Specificity | _e.g. 0.95_ |
| F1-score | _e.g. 0.92_ |
| ROC-AUC | _e.g. 0.97_ |

<!-- A confusion matrix image reads really well here, e.g.: -->
<!-- ![Confusion Matrix](results/confusion_matrix.png) -->

---

## Installation

Install the core dependencies:

```bash
pip install -r requirements.txt
```

PyEEG is installed separately from source:

```bash
git clone https://github.com/forrestbao/pyeeg.git
cd pyeeg
python setup.py install
```

---

## Usage

<!-- ===== EDIT to match your actual scripts/notebooks ===== -->

```bash
# 1. Place chbmit_preprocessed_data.csv in data/
# 2. Run the pipeline
python main.py
```

---

## Repository Structure

<!-- ===== EDIT to match your real layout ===== -->

```
.
├── data/                # CHB-MIT CSV (not tracked)
├── src/
│   ├── preprocessing.py # filtering + filter banks
│   ├── features.py      # feature extraction
│   ├── reduction.py     # PCA + feature selection
│   └── classify.py      # SVM training & evaluation
├── results/             # figures, metrics
├── requirements.txt
└── README.md
```

---

## Team

| Name | Section | BN |
|:-----|:-------:|:--:|
| Ammar Al-Saeed Mohammed | 2 | 1 |
| Ahmed Sayed Elbadawy | 1 | 4 |
| Ramadan Ibrahim | 1 | 34 |
