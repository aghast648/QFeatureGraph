# Experimental Protocol

## Objective

The protocol isolates whether pairwise redundancy changes fixed-k selection usefully beyond
single-channel importance, while keeping exact optimization available as ground truth.

## Required comparisons

| Comparison | Role |
|---|---|
| Unpruned CNN | Predictive and size baseline |
| Importance-only top-k | Tests whether the graph adds value |
| Exact graph/QUBO | Reference graph-based subset |
| Ideal QAOA | Quantum engineering comparison |
| Masked original | Immediate selection effect |
| Structurally reconstructed | Real parameter reduction |
| Fine-tuned reconstructed | Recoverable predictive performance |
| Remove-selected ablation | Checks whether selected channels matter |

## Run sequence

1. Create and persist deterministic split indices.
2. Train one baseline model for each declared seed.
3. Freeze the checkpoint; build the calibration graph and importance vector.
4. Run importance-only and exact graph-based selection.
5. Verify every energy and bit-order invariant before QAOA.
6. Run ideal depth-1 QAOA and decode sampled results.
7. Evaluate masked and reconstructed models before fine-tuning.
8. Fine-tune under the same policy for every selector.
9. Use validation results for analysis and protocol checks.
10. Freeze analysis choices, then evaluate the official test split once per final model.

## Tuning policy

Version 1 does not tune `k`, alpha, beta, lambda, architecture, or QAOA depth on official
test results. Any change to these values requires a new configuration. QAOA optimizer
settings may be debugged on synthetic objective fixtures before canonical runs, but failed
and inconclusive canonical runs remain in the experiment log.

## Reporting policy

Report all three seeds and aggregate mean, standard deviation, and individual values. Report
selection overlap using pairwise Jaccard similarity and the frequency of each channel. Keep
negative importance values, tied exact optima, infeasible QAOA samples, failures, and null
measurements. Do not report timing as acceleration unless warm-up, device, batch size,
repetition count, and synchronization method are documented.
