# Reproducibility

## Determinism controls

Seed Python, NumPy, PyTorch CPU and accelerator generators, data-loader workers, the data
split, and the QAOA optimizer/sampler. Enable deterministic PyTorch algorithms when the
selected platform supports them. Record any operation that cannot be made deterministic.

Exact numerical reproduction is not guaranteed across hardware or dependency releases.
The repository therefore treats environment and hardware provenance as required results.

## Per-run provenance

Every run must save:

- immutable configuration snapshot and SHA-256 hash;
- Git commit and dirty-worktree flag;
- split-index hashes and all seeds;
- Python, operating system, CPU, accelerator, driver, PyTorch, and Qiskit versions;
- checkpoint hash, tensor shapes, parameter counts, and model state size;
- raw importance, normalized importance, correlation and redundancy matrices;
- QUBO coefficients, Ising coefficients and offset, and ordering metadata;
- exact rankings, ties, QAOA parameters, displayed bitstrings and decoded vectors;
- evaluation metrics, warnings, failures, and elapsed times.

## Artifact policy

Downloaded data, checkpoints, activation tensors, and raw run directories are not committed
to ordinary Git history. Curated lightweight metrics, graph tables, configuration snapshots,
and final figures belong in `results/`. A future release should add a generated lock file
after the first validated environment; `pyproject.toml` remains the sole hand-written
dependency declaration.

## Reproduction gate

A release candidate must pass the smoke pipeline from a fresh environment, run the full
critical test suite, reconstruct at least one reported result from its saved configuration,
and confirm that no official-test metrics influenced development decisions.
