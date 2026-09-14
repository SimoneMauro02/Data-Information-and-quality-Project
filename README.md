# DIQ Project — Dataset 12

**Course:** Data and Information Quality
**Institution:** Politecnico di Milano — Scuola di Ingegneria Industriale e dell'Informazione
**Authors:** Santarossa Noah, Mauro Simone
**Advisor:** Prof. Camilla Sancricca
**Academic Year:** 2025-26

## Overview

This project applies a full data quality improvement pipeline to a real-world open dataset published by the Comune di Milano: **"Servizi alla persona — parrucchieri estetisti"**, containing information on hairdressing and beauty-care commercial activities across the city.

The dataset arrives with significant quality issues (missing values, inconsistent formatting, duplicate/fragmented records, unreliable location fields). The goal of the project is to profile, clean, and validate the dataset, transforming it from a low-quality raw export into a reliable, analysis-ready resource, while documenting every decision against standard data quality principles (TDQM cycle, functional dependencies, closed-world assumption, etc.).

## Dataset

- **Source:** Comune di Milano open data portal
- **Rows (raw):** 3,909
- **Rows (after cleaning):** 3,240
- **Columns:** activity type (`Tipo esercizio pa`), address (`Ubicazione`, split into `Tipo via`, `Via`, `Civico`, `Codice via`, `ZD`), primary activity (`Prevalente`), and surface areas (`Superficie altri usi`, `Superficie lavorativa`)

## Pipeline

The project follows a structured pipeline built around two macro-phases:

### 1. Data Profiling
- Single-column analysis (completeness, uniqueness, distinctness, constancy) for every attribute
- Correlation analysis between columns (e.g. `Codice via` ↔ `ZD`)
- Functional dependency discovery (`Via → Codice via`, `Via → Tipo via`, `Codice via → Via`)
- Association rule discovery (`Via → ZD`, non-strict due to streets crossing multiple zones)
- Perfect duplicate detection
- Overall data quality assessment along four dimensions: **timeliness, completeness, consistency, accuracy**

### 2. Data Cleaning
- **Normalization:** uppercase standardization, splitting `Ubicazione` into structured fields, decomposing multi-valued `Tipo esercizio pa` into four ordered columns (1NF), merging synonymous category labels
- **Error correction:** deterministic imputation via functional dependencies and association rules, statistical imputation (mode) grouped by activity category, domain-based outlier correction on `Civico` and on the surface-area fields (regulatory minima, 3σ rule)
- **Duplicate detection & data fusion:** records sharing the composite key `{Tipo via, Via, Civico}` are merged, consolidating fragmented/historical entries into a single coherent record per commercial unit

## Tools & Libraries

- **Language / environment:** Python 3.x, Google Colab
- **Libraries:** `pandas`, `numpy`, `ydata-profiling`, `json`, `matplotlib`, `seaborn`

## Results

| Aspect | Before | After |
|---|---|---|
| Rows | 3,909 | 3,240 (duplicates/fragments merged) |
| Geographical/spatial fields completeness | partial (down to ~66% for surfaces) | 100% |
| Composite key `Tipo via + Via + Civico` uniqueness | not guaranteed | 100% |
| Overall completeness | ~79% | significantly improved |

Full metrics, plots, and the reasoning behind each cleaning decision are documented in the accompanying report (`DIQ_prj.pdf`).

## Repository Structure

```
.
├── DIQ_prj.pdf          # Full project report
├── notebook/             # Colab/Jupyter notebook with the implemented pipeline
├── data/                 # Raw and cleaned dataset (if included)
└── README.md
```

## References

Key references used for methodology and validation are listed in the report's bibliography, including ISO 8000 data quality standards and the Total Data Quality Management (TDQM) framework.
