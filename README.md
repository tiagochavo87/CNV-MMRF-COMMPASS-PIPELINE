# CNV Pipeline — MMRF CoMMpass (GDC)

A GitHub-ready Jupyter notebook pipeline to download, normalize, QC, and analyze CNV segments from the **MMRF CoMMpass** project via the **NCI Genomic Data Commons (GDC)** API.

## Repository contents

- `CNV_MMRF_COMMPASS_Pipeline_GitHub_Documented.ipynb`  
  Documented notebook (may include saved outputs).

- `CNV_MMRF_COMMPASS_Pipeline_GitHub_Documented_NO_OUTPUTS.ipynb`  
  Same notebook, but with outputs stripped (recommended for version control).

- `requirements.txt`  
  Best-effort dependency list for local execution.

- `docs/PIPELINE_OVERVIEW.md`  
  A short, high-level description of the pipeline stages and outputs.

## Quickstart (local)

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab
```

Open the notebook and run it from top to bottom.

## Quickstart (Google Colab)

Upload the notebook to Colab and run top-to-bottom. The pipeline is written to be Colab-friendly.

## Outputs

All artifacts are written under:

- `outputs/run_<RUN_ID>/raw/`
- `outputs/run_<RUN_ID>/processed/`
- `outputs/run_<RUN_ID>/results/`
- `outputs/run_<RUN_ID>/logs/`

The notebook ends with a list of the main deliverables.

## Notes

- Exact-breakpoint recurrence (`SegmentID = chr:start-end`) is included for comparability, but breakpoint-variation can underestimate biological recurrence.
- Cytoband overlap and fixed 1Mb bins provide more stable recurrence summaries.

## License

MIT (see `LICENSE`).
