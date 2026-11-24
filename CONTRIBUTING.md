# Contributing | Contribuer

Contributions are welcome in scientific references, data dictionaries, source-to-processed variable mapping, unit tests, missingness audits, repeated-measurement analysis, grouped validation, residual analysis, and annotated extraction examples.

Rules:
1. Never overwrite or silently modify original source data.
2. Record every transformation in the transformation log.
3. Preserve unknown values as missing; do not replace them with zero without explicit source evidence.
4. Keep observed carbonation depth separate from model predictions.
5. Do not use random row splits to claim generalization across source studies.
6. Include a reproducible test or notebook cell for methodological changes.
7. Cite the source supporting scientific claims.
8. Do not commit credentials, private data, or materials whose redistribution rights are unclear.

Workflow: open an issue, identify exact input files and versions, propose a reproducible change, and report before/after results and limitations. English and French documentation are both welcome.
