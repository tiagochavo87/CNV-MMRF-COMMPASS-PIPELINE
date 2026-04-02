# Suggested Git update note

## Main documentation improvements
- replaced the minimal notebook introduction with a fuller repository-facing overview
- inserted explanatory markdown cells before all major analytical blocks
- documented inputs, outputs, recurrence strategies, and optional modules
- added `docs/PIPELINE_OVERVIEW.md`
- added `docs/NOTEBOOK_MAP.md`

## Important consistency point to review before commit
The uploaded notebook still displayed a **V7** label in its opening markdown, while the public repository README currently advertises **V10 (2026-02-19)**. Consider aligning the displayed version string across:
- notebook title/introduction
- `README.md`
- `CITATION.cff`
- release tag / commit message
