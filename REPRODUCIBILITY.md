# Reproducibility

## Reproduction model

The public repository now exposes **one canonical installation surface: `requirements.txt`**. It covers the compact/support-reliability diagnostics, the PyBullet UAV diagnostics, the test suite, and the supplemental continuous-control benchmark.

The registered experiments were originally executed in more than one dependency snapshot. Consolidating the install surface does **not** rewrite that historical fact. The exact material version records needed to interpret the executed runs are preserved below as provenance, while new users can install the complete public artifact from a single file.

The consolidated versions were selected only where the declared dependency ranges overlap. In particular, the pinned `gym-pybullet-drones` source requires Python `^3.10`, NumPy `^2.2`, SciPy `^1.15`, Matplotlib `^3.10`, PyBullet `^3.2.7`, Gymnasium `^1.2`, Stable-Baselines3 `^2.8`, and pytest `^9.0`; the unified pins remain inside those compatible ranges.

## Unified installation

Python 3.12 or newer is recommended for an all-in-one environment. The package metadata supports Python 3.10 or newer.

```bash
python -m venv .venv
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

`requirements.txt` installs the local package in editable mode and includes the core, test, UAV, and continuous-control dependencies. No second requirements file and no separate `pip install -e .` command are required.

Quick verification:

```bash
python scripts/reproduce_all.py --quick
python -m pytest
```

The quick path is a smoke/integrity check and is not a substitute for rerunning all registered training families. Its artifact audit is registry-driven: it checks E01-E28 source coverage, all 16 claim-evidence entries, public artifact paths, existing result audits, continuous-control S1/S2 counts and labels, protocol/environment hashes, and the public/private boundary.

Full Environment A reproduction (E01-E28) is:

```bash
python scripts/reproduce_all.py --full
```

The supplemental continuous-control grid is:

```bash
python scripts/reproduce_all.py --continuous-control
```

Existing-output audit without retraining is:

```bash
python scripts/reproduce_all.py --audit-only
```

Non-training release preflight is:

```bash
python scripts/reproduce_all.py --preflight
```

The preflight compiles `src/`, `scripts/`, and `tests/` using a temporary bytecode cache, runs the full pytest suite, exercises protocol/hash regression tests, and runs the artifact audit. It does not rerun training or regenerate registered scientific results.

Public deterministic derived assets that are not emitted directly by experiment aggregators can be regenerated with:

```bash
python scripts/generate_tables.py
python scripts/generate_figures.py
```

Both commands are constrained to the public `tables/` and `figures/` trees. They do not create or copy manuscript-facing files.

## Historical execution records

These records document the dependency snapshots used by the already-registered executions. They are provenance records, not additional installation files.

### Core/discrete snapshot

The earlier core snapshot used:

```text
gymnasium==1.3.0
matplotlib==3.10.8
minigrid==3.1.0
numpy==2.4.3
pandas==3.0.1
PyYAML==6.0.3
scipy==1.17.1
torch==2.11.0
```

### PyBullet UAV snapshot

The recorded UAV snapshot used the pinned drone source commit `9bc12bc583fa3b28807b2f90a8cadf09fb06e1ff` and the following direct pins:

```text
gym-pybullet-drones @ git+https://github.com/utiasDSL/gym-pybullet-drones.git@9bc12bc583fa3b28807b2f90a8cadf09fb06e1ff
gymnasium==1.2.3
matplotlib==3.10.9
minigrid==3.1.0
numpy==2.4.6
pandas==3.0.3
pybullet==3.2.7
PyYAML==6.0.3
pytest==9.0.3
scipy==1.17.1
torch==2.12.0
```

Python 3.12 was the recommended interpreter for this recorded PyBullet stack.

### Continuous-control execution snapshot

The registered continuous-control runtime records Python 3.13.x, CPU execution with one environment per process, three independent worker processes, two Torch intra-op threads per worker, and one inter-op thread per worker.

The original 32-package `pip freeze` snapshot was:

```text
absl-py==2.5.0
cloudpickle==3.1.2
etils==1.14.0
Farama-Notifications==0.0.6
filelock==3.32.2
fsspec==2026.7.0
glfw==2.10.2
gymnasium==1.3.0
ImageIO==2.37.4
Jinja2==3.1.6
MarkupSafe==3.0.3
mpmath==1.3.0
mujoco==3.10.0
networkx==3.6.1
numpy==2.5.1
packaging==26.3
pandas==3.0.5
pillow==12.3.0
psutil==7.2.2
PyOpenGL==3.1.10
python-dateutil==2.9.0.post0
PyYAML==6.0.3
sb3_contrib==2.9.0
scipy==1.18.0
setuptools==83.0.0
six==1.17.0
stable_baselines3==2.9.0
sympy==1.14.0
torch==2.13.0
typing_extensions==4.16.0
tzdata==2026.3
zipp==4.1.0
```

The SHA-256 of that historical freeze text was:

```text
1dd2fef815ad702e0cff2f6ae2edaa16d76bad039b58210bba15a92c236f6d95
```

The frozen scientific specification remains `configs/continuous_control/CONTINUOUS_CONTROL_PROTOCOL.yaml`, with its hash manifest in `configs/continuous_control/CONTINUOUS_CONTROL_PROTOCOL_SHA256.txt`.

Registered example:

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

The registered grid consists of SAC, CrossQ, and TQC; HalfCheetah-v5 and Walker2d-v5; five training seeds per algorithm–environment pair; 100,000 nominal training interactions per agent; and deterministic evaluation under nominal, observation-delay, actuation-authority, and exploratory combined shifts.

Aggregation:

```bash
python scripts/aggregate_continuous_control.py
```

The aggregate audit expects 30 complete trained runs, 3,600 final episode rows, 600 checkpoint episode rows, eight S1 controller contrasts, and twelve S2 support contrasts.

### Continuous-control release packaging

The clean public release retains the frozen protocol and release-facing analysis outputs (`condition_summary.csv`, seed/checkpoint summaries, return AUC, S1/S2 contrasts, and `audit.json`). Per-agent training work directories and serialized model checkpoints are execution caches rather than claim-bearing release artifacts; they are retained in the private historical archive and are intentionally excluded from GitHub/Zenodo. They can be regenerated from the frozen protocol with `python scripts/reproduce_all.py --continuous-control`. This packaging change does not alter any reported numerical result.

## Public artifact boundary

This repository reproduces the computational evidence only. Manuscript compilation, supplementary compilation, bibliography processing, reviewer-response generation, and journal-submission packaging are outside this public artifact and are maintained in the separate private project area.

Scripts whose sole purpose is to generate private manuscript-facing assets must not be treated as required public reproduction steps.

## Determinism and statistical reproduction

Random seeds control environment resets, action selection, replay sampling, model initialization, and registered evaluation resets where applicable. Exact bitwise identity across operating systems, processors, BLAS libraries, CUDA/CPU implementations, or dependency builds is not guaranteed. Statistical reproduction of historical runs should use the recorded package versions above, protocol files, and seed sets. New full-repository installations should use the canonical `requirements.txt`.

## Support diagnostics

The artifact distinguishes exact-key support, approximate/tolerance support, kernel affinity, replay-neighborhood support, branch-use rates, and fallback rates. These measures have different estimands and must not be interchanged.

Continuous-control support is computed from final nominal replay observations with deterministic subsampling, per-dimension standardization, k=5 cKDTree neighborhoods, and a radius defined by the 95th percentile of fifth non-self-neighbor distances. Reward, failure labels, and shifted observations do not enter the radius calibration.

## Audit order for a frozen public snapshot

A public frozen snapshot should pass, in order:

1. `python scripts/reproduce_all.py --preflight` for compile, pytest, protocol/environment-lock, registry, result-audit, and public-boundary checks;
2. optional `python scripts/reproduce_all.py --quick` smoke reproduction;
3. final repository SHA-256 manifest generation from the frozen public tree;
4. `python scripts/reproduce_all.py --preflight --require-manifest` for the frozen-release preflight.

Before step 3, `audit_artifact.py` operates in pre-publication mode and does not require `MANIFEST.sha256`. After the final manifest is generated, `--require-manifest` changes the audit to frozen-release mode and verifies SHA-256 values **and exact public-file coverage**: omitted, duplicate, unexpected, missing, or mismatched entries fail the audit. No manifest from an earlier tree should be reused after file moves, deletions, metadata rewrites, dependency-manifest consolidation, or regenerated computational artifacts.
