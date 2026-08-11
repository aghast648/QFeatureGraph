# Project Scope

## Purpose

QFeatureGraph investigates whether an activation-similarity graph adds useful structure to
importance-based channel pruning in a deliberately small convolutional neural network (CNN).
The proof-of-concept is designed to be understandable, exhaustively verifiable, and
reproducible before it is expanded.

## Research question

For a fixed number of retained first-layer CNN channels, can a selection objective that
balances ablation importance against pairwise activation redundancy retain predictive
performance better or more consistently than importance-only selection?

## Version 1 scope

- Fashion-MNIST and one fixed `1 -> 8 -> 16` CNN.
- The eight post-ReLU `conv1` channels as graph nodes.
- Positive Pearson correlation as pairwise redundancy.
- Single-channel masking loss change as importance.
- Fixed-cardinality selection with `k = 4`.
- Importance-only, exact graph/QUBO, and ideal depth-1 QAOA selectors.
- Masked, structurally pruned, and fine-tuned evaluations.
- Three training seeds for stability evidence.

## Explicit nonclaims

- QAOA integration is an engineering comparison, not evidence of quantum advantage.
- Correlation indicates possible redundancy; it does not establish causation.
- Exact enumeration is the reference solver because eight binary variables are tractable.
- Version 1 results do not generalize automatically to other models, layers, or datasets.
- Runtime improvements are reported only when measurement conditions are controlled.

## Deferred work

QPU execution, noisy simulation, deeper QAOA, multiple architectures, multiple datasets,
learned graph construction, unstructured pruning, hardware deployment, publication claims,
and automated hyperparameter searches are outside version 1.

## Completion criteria

The PoC is complete when the canonical configuration runs end to end, all critical
equivalence and bit-order tests pass, exact and QAOA selections are decoded correctly,
structural pruning produces the expected parameter reduction, required baselines are
reported across the declared seeds, and a fresh environment can reproduce the curated
results from documented commands.
