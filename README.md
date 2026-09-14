# Support-Boundary and Relative-Reliability Diagnostics for Hybrid Memory–Neural Reinforcement Learning

[![Research article DOI](https://img.shields.io/badge/Research%20article-10.1016%2Fj.asoc.2026.116350-blue)](https://doi.org/10.1016/j.asoc.2026.116350)
[![Artifact DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21897588.svg)](https://doi.org/10.5281/zenodo.21897588)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

This repository is the independent public reproducibility artifact for an audited study of support boundaries, estimator-relative reliability, fuzzy and crisp arbitration, fallback behavior, sensorized software-in-the-loop control, and continuous-control transfer diagnostics. The Python package retains the internal project name `confidence-gated-q` for import and package compatibility.

The artifact is organized around one scientific question: under which support and estimator-reliability conditions can hybrid memory–neural control make auditable branch-selection decisions, and where do exact, approximate, fuzzy, crisp, fallback, or replay-support mechanisms fail under shift?

The artifact does **not** establish a universally superior reinforcement-learning controller, calibrated correctness confidence, hardware readiness, flight safety, or operational deployment readiness.

## Associated publication

This repository is the reproducibility artifact associated with the following peer-reviewed research article:

> **Ercan Erkalkan.** “Support-boundary and relative-reliability diagnostics for hybrid memory–neural reinforcement learning.” *Applied Soft Computing*, **204** (2027), 116350. DOI: [10.1016/j.asoc.2026.116350](https://doi.org/10.1016/j.asoc.2026.116350).

Publisher page: https://www.sciencedirect.com/science/article/pii/S1568494626017989

### Citation

If this repository or its results are used in academic work, please cite the research article and, when reproducibility materials are specifically used, the software artifact as well. The repository-level citation metadata is also available in [`CITATION.cff`](CITATION.cff).

**Research article — BibTeX**

```bibtex
@article{ERKALKAN2027116350,
  title   = {Support-boundary and relative-reliability diagnostics for hybrid memory--neural reinforcement learning},
  journal = {Applied Soft Computing},
  volume  = {204},
  pages   = {116350},
  year    = {2027},
  issn    = {1568-4946},
  doi     = {10.1016/j.asoc.2026.116350},
  url     = {https://www.sciencedirect.com/science/article/pii/S1568494626017989},
  author  = {Ercan Erkalkan}
}
```

**Reproducibility artifact**

```text
Erkalkan, E. (2026). Support-Boundary and Relative-Reliability Diagnostics for Hybrid Memory–Neural Reinforcement Learning: Reproducibility Artifact (Version 1.0.0). Zenodo. https://doi.org/10.5281/zenodo.21897588
```

## Public/private boundary

This directory is the **public reproducibility artifact only**. Manuscript, supplementary manuscript files, bibliography sources, reviewer responses, editorial correspondence, cover letters, title pages, highlights, and other journal-submission material are maintained separately under the private project area and are intentionally excluded from this repository and from the public Zenodo artifact.

## Scientific evidence families

The repository contains the following connected evidence families:

- compact tabular/DQN recurrence diagnostics and matched stronger neural comparators;
- held-out exact-support shifts and support-abstention replication;
- approximate support through tolerance-kNN and feature-distance/kernel affinity;
- application-navigation fallback and risk-adjusted comparisons;
- fuzzy arbitration, relative-reliability diagnostics, and same-input crisp falsification;
- independent reliability-shift generators and support-estimator selection;
- state-accessible and sensorized Crazyflie/PyBullet software-in-the-loop diagnostics;
- factorial, temporal/interface, safety-trace, and feasibility follow-ups for the sensorized boundary;
- a supplemental/non-confirmatory SAC–CrossQ–TQC benchmark on HalfCheetah-v5 and Walker2d-v5.

Null and negative results are retained as evidence. Continuous-control results are supplemental/non-confirmatory and are not used for universal controller ranking.

## Canonical public-artifact layout

```text
support-boundary-relative-reliability-artifact/
├── .gitignore
├── README.md
├── PROVENANCE.md
├── REPRODUCIBILITY.md
├── CITATION.cff
├── .zenodo.json
├── pyproject.toml
├── requirements.txt
├── configs/
│   ├── evidence_registry.json
│   ├── claim_evidence_index.yaml
│   ├── diagnostic_extensions/
│   └── continuous_control/
├── src/
├── scripts/
├── tests/
├── results/
│   ├── diagnostic_extensions/
│   └── continuous_control/
├── tables/
├── figures/
└── audits/
```

The manuscript tree is not part of this public layout.

## Evidence and claim control

`configs/evidence_registry.json` defines the active evidence families and evidence classes.

`configs/claim_evidence_index.yaml` maps the principal scientific claim families to stable computational evidence and inference classes. Public claim-evidence control must remain independent of private manuscript file paths.

Protocol locks, seed registries, raw/aggregated outputs, and SHA-256 manifests provide execution traceability. Evaluation is read-only where declared by the protocol.

## Key evidence boundaries

- Exact count support is informative only when recurring exact states provide relevant memory evidence.
- Approximate support softens the exact-key boundary but remains representation-dependent.
- Relative reliability is useful under the targeted stale-memory mechanism but does not generalize as a uniformly superior gate across independent shift generators.
- Same-input crisp comparisons prevent a claim that fuzzy defuzzification is necessary for the observed relative-reliability mechanism.
- Sensorized SIL exposes exact-support collapse and partial kNN coverage without learned waypoint success.
- Continuous-control replay support responds consistently to observation-delay mismatch but not to downstream actuation-authority change as a generic degradation signal.

## Installation

### Unified environment

The repository uses **one canonical dependency manifest: `requirements.txt`**. It covers the core/discrete diagnostics, test suite, PyBullet UAV diagnostics, and supplemental MuJoCo continuous-control benchmark. Python 3.12 or newer is recommended for a single all-in-one environment; the package metadata itself supports Python 3.10 or newer.

```bash
python -m venv .venv
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

`requirements.txt` also installs the local `confidence-gated-q` package in editable mode, so a separate `pip install -e .` step is not required.

The original registered executions used slightly different dependency snapshots for the UAV and continuous-control runs. Those historical versions are retained as provenance in [`REPRODUCIBILITY.md`](REPRODUCIBILITY.md), while `requirements.txt` is the single current install surface for the repository.

## Quick verification

```bash
python scripts/reproduce_all.py --quick
python -m pytest
```

The quick path runs tests, a small smoke experiment, public-only deterministic asset generation, and a registry-driven integrity audit over E01-E28, the 16-claim claim-evidence index, continuous-control S1/S2 outputs, protocol/environment hashes, result audits, and the public/private boundary. It does not recreate every computationally expensive evidence family.

To rerun the registered Environment A evidence families E01-E28, use:

```bash
python scripts/reproduce_all.py --full
```

The supplemental continuous-control grid can be rerun from the same canonical environment with:

```bash
python scripts/reproduce_all.py --continuous-control
```

To audit the existing public tree without rerunning experiments:

```bash
python scripts/reproduce_all.py --audit-only
```

Release preflight without training or result regeneration is:

```bash
python scripts/reproduce_all.py --preflight
```

This compiles `src/`, `scripts/`, and `tests/` into a temporary cache, runs the full pytest suite, verifies protocol/environment SHA-256 locks, and executes the registry-driven public-artifact audit without mutating the repository tree.

After the final `MANIFEST.sha256` is generated from the frozen public tree, require complete manifest coverage with either:

```bash
python scripts/reproduce_all.py --audit-only --require-manifest
python scripts/reproduce_all.py --preflight --require-manifest
```

The frozen-release audit rejects missing, duplicate, unexpected, hash-mismatched, **and omitted public-file** manifest entries.

## Public derived assets

Public-only deterministic assets can be regenerated with:

```bash
python scripts/generate_tables.py
python scripts/generate_figures.py
```

These scripts write only to `tables/` and `figures/`. Manuscript-facing LaTeX tables, graphical abstracts, and private paper copies are generated outside this public repository. Most experiment-specific tables and figures continue to be emitted by their corresponding `aggregate_*` scripts.

## Continuous-control execution

Example registered run:

```bash
python scripts/run_continuous_control.py \
  --lock-yaml configs/continuous_control/CONTINUOUS_CONTROL_PROTOCOL.yaml \
  --lock-manifest configs/continuous_control/CONTINUOUS_CONTROL_PROTOCOL_SHA256.txt \
  --mode supplemental \
  --algorithm SAC \
  --environment HalfCheetah-v5 \
  --seed 22000 \
  --output results/continuous_control
```

Aggregate the complete registered grid with:

```bash
python scripts/aggregate_continuous_control.py
```

## Artifact identity

- Artifact version: `1.0.0`
- ORCID: `0000-0001-9259-7112`
- Repository URL: https://github.com/ErcanErkalkan/support-boundary-relative-reliability-artifact
- GitHub release: https://github.com/ErcanErkalkan/support-boundary-relative-reliability-artifact/releases/tag/v1.0.0
- Zenodo DOI: https://doi.org/10.5281/zenodo.21897588
- Associated article DOI: https://doi.org/10.1016/j.asoc.2026.116350

No DOI or repository URL from an earlier public artifact series is reused by this artifact.

## License

MIT License. See `LICENSE`.
