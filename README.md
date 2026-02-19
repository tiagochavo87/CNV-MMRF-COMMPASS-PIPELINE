# CNV Pipeline — MMRF CoMMpass (GDC)

A GitHub-ready Jupyter notebook pipeline to **process**, **QC**, and **analyze** Copy Number Variation (CNV) segments from the **MMRF CoMMpass** cohort via the NCI **Genomic Data Commons (GDC)**.

This repository focuses on *downstream* CNV segment analytics (recurrence summaries, cytoband / fixed-bin aggregation, clinical integration, and survival/staging evaluation). The pipeline is designed to run in **Google Colab** or locally.

> Updated notebook version: **V10** (2026-02-19)

## Repository contents

- `CNV_MMRF_COMMPASS_Pipeline_GitHub_Documented.ipynb`  
  Main notebook (outputs stripped to keep diffs clean).

- `docs/PIPELINE_OVERVIEW.md`  
  High-level description of stages, inputs, and outputs.

- `requirements.txt`  
  Best-effort dependency list for local execution.

- `CITATION.cff`  
  Citation metadata for this software/repository.

- `LICENSE`  
  MIT license.

## Quickstart (local)

    python -m venv .venv
    # Linux/Mac:
    source .venv/bin/activate
    # Windows (Git Bash):
    source .venv/Scripts/activate
    # Windows (PowerShell):
    # .venv\Scripts\Activate.ps1

    pip install -r requirements.txt
    # If you don't have Jupyter installed:
    # pip install jupyterlab
    jupyter lab

Open the notebook and run it from top to bottom.

## Quickstart (Google Colab)

Upload the notebook to Colab and run top-to-bottom.  
If you store inputs in Google Drive, mount Drive and point the notebook to your `.tsv` inputs.

## Outputs

All artifacts are written under:

    outputs/run_<RUN_ID>/raw/
    outputs/run_<RUN_ID>/processed/
    outputs/run_<RUN_ID>/results/
    outputs/run_<RUN_ID>/logs/

Key intermediate deliverables commonly produced include:

- `outputs/run_<RUN_ID>/processed/file_metadata.tsv`
- `outputs/run_<RUN_ID>/processed/combined_cnvs.txt`
- `outputs/run_<RUN_ID>/processed/cnv_segments.parquet`
- `outputs/run_<RUN_ID>/processed/combined_cnvs_filtrado_somatica.txt`

The notebook ends with a concise list of deliverables and their paths.

## Notes / limitations

- **Exact-breakpoint recurrence** (`SegmentID = chr:start-end`) is included for comparability, but breakpoint variation can underestimate biological recurrence.
- **Cytoband overlap** and **fixed 1Mb bins** provide more stable recurrence summaries.
- When working with CNVs already called upstream, **purity/ploidy adjustment may be unavailable**; the pipeline is explicit about this limitation.

## License

MIT (see `LICENSE`).
