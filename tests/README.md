# Test Plan

Tests are added with the implementation they validate. Planned files are:

- `test_deterministic_splits.py`
- `test_model_shapes.py`
- `test_parameter_counts.py`
- `test_activation_capture.py`
- `test_dead_channel_handling.py`
- `test_correlation_graph.py`
- `test_ablation_importance.py`
- `test_qubo_energy_equivalence.py`
- `test_exact_cardinality.py`
- `test_qubo_ising_equivalence.py`
- `test_qiskit_bit_ordering.py`
- `test_structural_pruning.py`
- `test_reproduction_smoke.py`

The QUBO, Ising, cardinality, reconstruction, and bit-order tests are release blockers.
