# Pipeline overview

This document provides a high-level description of the notebook workflow implemented in `CNV_MMRF_COMMPASS_Fixed_documented_for_git.ipynb`.

## 1. Objective

The notebook processes CNV segment data from the MMRF CoMMpass cohort and produces cohort-level and patient-level outputs suitable for:

- descriptive genomics
- recurrent CNV profiling
- clinical association analyses
- overall survival modeling
- figure and supplementary-table generation

## 2. Core inputs

### Required
- CNV segment files obtained from the GDC `MMRF-COMMPASS` project
- `clinical.tsv`
- `follow_up.tsv`

### Optional
- `family_history.tsv`
- `exposure.tsv`
- `pathology_detail.tsv`
- hg38 cytoband annotation table (`cytoBand_hg38.tsv`)

## 3. Directory layout

Each execution creates a structured run folder:

```text
outputs/run_<RUN_ID>/
├── raw/        # downloaded source files
├── processed/  # harmonized and filtered tables
├── results/    # analytical outputs
└── logs/       # provenance and run parameters
```

## 4. Workflow stages

### Stage 0 — Setup
Installs or validates the required Python packages and imports the libraries used throughout the notebook.

### Stage 1 — Configuration and provenance
Defines:
- run identifier
- output directories
- GDC endpoints
- CNV classification thresholds
- genomic bin size
- reproducibility seed

A JSON record of these settings is saved to `logs/run_params.json`.

### Stage 2 — Utility layer
Defines helper functions for:
- dataframe auditing
- hash calculation
- safe column resolution
- lightweight text exports

### Stage 3 — GDC file discovery
Queries the GDC files API for open-access CNV files associated with `MMRF-COMMPASS`, then extracts file metadata and participant identifiers.

### Stage 4 — Download and integrity validation
Downloads the selected CNV files and checks MD5 hashes when available.

### Stage 5 — Metadata export
Builds a clean metadata table that links files to GDC IDs and participant identifiers.

### Stage 6 — CNV loading and normalization
Parses all downloaded CNV segment files into a canonical schema with harmonized coordinates and standardized chromosome labels.

### Stage 7 — CNV classification
Maps `Segment_Mean` to biologically interpretable CNV states:
- homozygous deletion
- single-copy loss
- neutral
- single-copy gain
- high-level amplification
- uncertain / margin

### Stage 8 — High-confidence CNV subset
Removes neutral and uncertain segments to generate the high-confidence somatic CNV set used in downstream analyses.

### Stage 8.5 — Exact segment recurrence
Quantifies recurrence using exact `chr:start-end` segment identity. This is strict but breakpoint-sensitive.

### Stage 9 — Cytoband recurrence
Intersects CNVs with cytobands to create a more stable recurrence summary across patients.

### Stage 10 — Fixed-bin recurrence
Aggregates CNVs across fixed 1 Mb bins to support robust cohort-level recurrence summaries and patient-region feature matrices.

### Stage 11 — Breakpoint features
Builds patient-level breakpoint burden and distribution metrics.

### Stage 12 — Hotspot features
Builds hotspot-style genomic burden features derived from breakpoint density or concentrated overlap patterns.

### Stage 12B — Breakpoint landscape
Exports cohort-level breakpoint summaries by chromosome, cytoband, genomic bin, and exact position.

### Stage 13 — Clinical / survival dataset
Finds and harmonizes clinical and follow-up tables, merges optional auxiliary clinical tables, and constructs a patient-level survival-ready dataframe without forcing imputation.

### Stage 14 — Feature merge
Combines CNV-derived features with survival variables to build the modeling dataset.

### Stage 15A — Sex-difference tests
Runs Mann–Whitney comparisons and FDR correction for selected quantitative features.

### Stage 15 — Cox models
Fits survival models for engineered CNV features, using age and sex covariates when available.

### Stage 15B — Kaplan–Meier plots
Builds survival curves for the top-ranked quantitative features.

### Stage 16 — Optional clustering
Performs deterministic unsupervised clustering using global CNV-derived features.

### Stage 17 — Survival by cluster
Evaluates overall survival across clusters when cluster labels are available.

### Stage 18–21B — Recurrent CNV survival/staging module
Selects top recurrent CNVs using fixed-bin and exact-segment representations, then evaluates:
- survival associations
- Kaplan–Meier separation
- stage-stratified patterns
- optional contingency tests

### Stage 22 — Publication checklist
Runs sanity checks to support review before sharing or manuscript drafting.

### Stage 23 — Optional predictive modeling
Creates predictive datasets and runs exploratory models for:
- survival risk
- ISS stage classification

### Final stages — Inventory and backup
Exports a detailed run inventory and creates a zip archive of the working results.

## 5. Recommended interpretation

For biological interpretation, prefer the **cytoband** and **fixed-bin** recurrence outputs over exact-segment recurrence, unless the specific goal is strict breakpoint-level reproducibility.

## 6. Intended use

This pipeline is appropriate for:
- exploratory genomic characterization
- hypothesis generation
- reproducible supplementary material creation
- figure/table preparation for manuscripts

Additional validation is recommended before using optional predictive modules for strong inferential claims.
