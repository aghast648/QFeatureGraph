# Frozen PoC Specification

This document is the authoritative version 1 scientific contract. Changes that alter an
experiment require a new versioned configuration and a decision record.

## Dataset and partitions

Use the 60,000-image Fashion-MNIST training split to create deterministic, disjoint
partitions of 50,000 training, 5,000 calibration, and 5,000 validation examples. Use split
seed 42 and store the resulting indices. The official 10,000-image test split remains
unread during development and is evaluated only after the protocol is frozen.

The training partition fits model weights. The calibration partition constructs the graph
and measures channel importance. The validation partition selects permissible training and
fine-tuning decisions. The official test partition supports final reporting only.

## Baseline CNN

For 28 by 28 grayscale inputs, use:

1. `Conv2d(1, 8, kernel_size=3, padding=1)` -> ReLU -> 2 by 2 max pool.
2. `Conv2d(8, 16, kernel_size=3, padding=1)` -> ReLU -> 2 by 2 max pool.
3. Flatten 16 by 7 by 7 = 784 values.
4. `Linear(784, 64)` -> ReLU -> `Linear(64, 10)`.

Capture `conv1` activations after ReLU and before pooling. Train independently for seeds
17, 42, and 91 using the canonical configuration.

## Feature graph

For N calibration examples, capture activations with shape `[N, 8, 28, 28]`. Globally
average each spatial map to obtain `Z` with shape `[N, 8]`. Each column is one channel's
calibration response.

Compute signed Pearson correlation `rho_ij` between channel columns and define redundancy
as `R_ij = max(0, rho_ij)` for `i != j`, with a zero diagonal. A channel whose calibration
standard deviation is at or below `1e-12` is marked constant; its pairwise correlations are
set to zero and the event is reported rather than hidden.

## Channel importance

First measure baseline calibration loss. Then mask one post-ReLU `conv1` channel at a time
without changing model weights and measure

`delta_L_i = L_masked_i - L_baseline`.

Store every raw value, including negative values. For the QUBO only, use

`I_i = max(0, delta_L_i) / max_j(max(0, delta_L_j))`.

If all positive parts are zero, set all `I_i` to zero and record that importance provides
no discriminating signal.

## Fixed-cardinality selection

Let `x_i = 1` mean channel i is retained. Minimize

`E(x) = -alpha sum_i I_i x_i + beta sum_{i<j} R_ij x_i x_j + lambda (sum_i x_i - k)^2`,

with `k = 4`, `alpha = 1.0`, `beta = 0.5`, and `lambda = 10.0`. Exact enumeration must
score all 256 bitstrings and verify that the lowest-energy result has cardinality four.
Direct subset scoring and assembled QUBO energy must rank every bitstring identically.

## QAOA comparison

Use an ideal statevector simulator, the current Qiskit V2 primitive interfaces, depth
`p = 1`, COBYLA, and the declared optimizer and sampling seeds. Decode Qiskit display-order
bitstrings into channel-order vectors through one tested function. Report the best sampled
feasible subset, its probability, objective gap to exact, cardinality-feasible probability,
optimizer evaluations, and final parameters.

## Structural pruning

For a selected subset, rebuild `conv1` with four output channels and copy only the selected
filters and biases. Rebuild `conv2` with four input channels and copy the corresponding input
slices; keep all 16 outputs. Later layers retain their shapes. Verify numerical equivalence
between the masked original and reconstructed model before fine-tuning, within documented
floating-point tolerance.

## Required evaluations

Evaluate the unpruned baseline, importance-only top-k, exact graph/QUBO, and ideal QAOA
selections. For each selected subset report immediate masking, structural reconstruction,
and post-fine-tuning results. Include remove-selected-channel ablation as an interpretation
check. Report loss, accuracy, parameters, serialized state size, selection overlap and
stability, plus latency only under controlled measurement.
