# Notebook map

This file documents the logical organization of the notebook and can be used as a reviewer-facing guide for navigation.

## Section map

| Section | Purpose | Main outputs |
|---|---|---|
| 0) Setup | Install/import dependencies | in-memory environment |
| 1) Config + provenance | Define run parameters and directories | `logs/run_params.json` |
| 2) Utilities | Reusable helper functions | audit text files |
| 3) GDC listing | Query available CNV files and metadata | file discovery table |
| 4) Download | Retrieve files and verify integrity | raw downloaded CNV files |
| 5) Metadata export | Build participant-linked metadata | `processed/file_metadata.tsv` |
| 6) Load + normalization + QC | Standardize segment schema | `processed/combined_cnvs.txt`, `processed/cnv_segments.parquet` |
| 7) CNA classification | Convert `Segment_Mean` into CNV states | `processed/combined_cnvs_classified_adjusted.tsv` |
| 8) High-confidence CNVs | Remove neutral/uncertain calls | `processed/combined_cnvs_filtrado_somatica.txt` |
| 8.5) Exact recurrence | Strict `SegmentID` recurrence | exact recurrence TSVs |
| 9) Cytoband recurrence | Stable overlap-based recurrence | cytoband recurrence TSVs |
| 10) Fixed-bin recurrence | 1 Mb bin recurrence | bin recurrence TSVs |
| 11) Breakpoint features | Patient-level breakpoint metrics | `results/breakpoint_metrics_per_patient.tsv` |
| 12) Hotspot features | Patient-level hotspot metrics | `results/hotspot_metrics_per_patient.tsv` |
| 12B) Breakpoint landscape | Cohort-level breakpoint summaries | multiple breakpoint summary TSVs |
| 13) OS lockdown | Clinical/follow-up merge | `results/os_df_patient_level.tsv` |
| 14) Merge features ↔ OS | Build survival modeling dataset | `results/survival_features_merged.tsv` |
| 15A) Sex differences | Feature comparisons by sex | supplementary-style TSVs |
| 15) Cox models | Survival association for engineered features | `results/cox_results_breakpoints_hotspots.tsv` |
| 15B) KM plots | Survival curves for top metrics | KM PNGs and summary TSVs |
| 16) Clustering | Unsupervised exploratory grouping | `results/cluster_assignments.tsv` |
| 17) Survival by cluster | Cluster-level survival comparison | log-rank summary |
| 18–21B) Recurrent CNV module | Survival/staging evaluation of top CNVs | recurrent CNV Cox/KM/staging TSVs |
| 22) Checklist | Publication sanity checks | console + checks |
| 23) Prediction models | Optional exploratory ML | model summaries and exports |
| Final inventory | Export run summary | text inventory |
| Final backup | Zip workspace | backup `.zip` |

## Important implementation details

### Participant linkage
The notebook explicitly relies on GDC metadata joins to recover `cases.submitter_id` / `Participant_ID`, avoiding fragile filename-based heuristics.

### Missing data policy
The survival block avoids blindly converting missing clinical information into zeros. Models are fit only on rows with the required variables present.

### Recurrence strategy
Three recurrence schemes are implemented:
1. exact `SegmentID`
2. cytoband overlap
3. fixed 1 Mb bins

This design makes it possible to compare strict breakpoint-level summaries with more biologically stable regional summaries.

### Optional modules
Clustering and prediction are optional and should be interpreted as exploratory unless validated independently.

## Suggested use in the repository

- Keep this file in `docs/` for readers who want a quick roadmap.
- Link it from the main `README.md`.
- Mention the notebook name explicitly in release notes when a new version is committed.
