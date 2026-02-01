# Pipeline overview

This repository contains a GitHub-ready, documented Jupyter notebook that performs an end-to-end CNV workflow for the **MMRF CoMMpass** project through the **NCI Genomic Data Commons (GDC)** API.

## High-level stages

1. **Setup**
   - Colab/Jupyter configuration, imports, and runtime options.

2. **Configuration & provenance**
   - Defines `RUN_ID`, output folders, thresholds, and API endpoints.

3. **Data discovery & download**
   - Lists CNV files via the GDC `files` endpoint.
   - Downloads raw CNV files with integrity checks.

4. **Canonical CNV table**
   - Loads CNV segments into a single canonical table (`df_cnvs_final`).
   - Normalizes schema and performs QC.

5. **Recurrence**
   - Exact breakpoint (SegmentID), cytoband overlap, and fixed 1Mb bin overlap.

6. **Clinical & survival**
   - Builds OS outcomes without imputation.
   - Cox regression and Kaplan–Meier summaries.
   - Optional clustering and cluster-level survival.

## Outputs

Results are written to:
- `outputs/run_<RUN_ID>/processed/`
- `outputs/run_<RUN_ID>/results/`
- `outputs/run_<RUN_ID>/logs/`

The notebook documents the key files at the end (see the “End — main outputs” section).

## Reproducibility tips

- Prefer the `*_NO_OUTPUTS.ipynb` notebook for GitHub (small diffs, cleaner history).
- Consider enabling notebook output stripping in CI (e.g., `nbstripout`) if you commit executed notebooks.
