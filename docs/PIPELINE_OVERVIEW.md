# Pipeline overview (V7)

This document summarizes the stages implemented in the notebook:

`CNV_MMRF_COMMPASS_Pipeline_GitHub_Documented(_NO_OUTPUTS).ipynb`

## Inputs

Typical inputs (file names may vary depending on the release/source):

- CNV segment files (from GDC MMRF-COMMPASS, Copy Number Segment / similar)
- Clinical table (e.g., `clinical.tsv`)
- Follow-up table (e.g., `follow_up.tsv`)

> The pipeline is written to work with **publicly accessible** artifacts via GDC whenever possible.  
> Some CoMMpass data are controlled-access; ensure you comply with the data-use terms for your source.

## Stages

1. **Environment + run configuration**
   - Creates a run folder: `outputs/run_<RUN_ID>/...`
   - Captures versions, timestamps, and key parameters.

2. **Load / harmonize CNV segments**
   - Standardizes chromosome, start/end coordinates, sample identifiers
   - Generates stable segment identifiers and harmonized CNV type fields

3. **QC + basic summaries**
   - Sample counts, missingness checks, coordinate sanity
   - Per-sample CNV burden summaries (as available from segments)

4. **Recurrence summaries**
   - Exact `SegmentID` recurrence (breakpoint-sensitive)
   - Cytoband overlap recurrence (more stable)
   - Fixed-bin recurrence (default 1 Mb bins)

5. **Clinical integration**
   - Builds an OS dataset (no imputation) from clinical + follow-up
   - Links patients to CNV features (presence/absence and/or burden summaries)

6. **Association analyses**
   - Example: CNV frequency differences by ISS stage (with multiple-testing control)

7. **Survival analyses**
   - Kaplan–Meier plots for presence/absence of top recurrent CNVs
   - Cox proportional hazards models (with covariate adjustment where available)

8. **(Optional) clustering**
   - Exploratory clustering for hypothesis generation

## Key outputs (examples)

- `processed/file_metadata.tsv`
- `processed/cnv_segments.parquet`
- `results/cnv_recurrence_by_cytoband.tsv`
- `results/cnv_recurrence_by_bins_1Mb.tsv`
- `results/cox_results_*.tsv`
- `results/km_plots_*.png`

Exact file names can differ depending on run options; the notebook prints the final output inventory.
