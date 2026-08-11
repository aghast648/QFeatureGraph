# Mathematical Conventions

## Variables and ordering

Channels use zero-based order `[0, 1, ..., 7]`. Binary vector element `x_i = 1` means the
channel is retained. Every table, serialized vector, graph node, QUBO variable, and decoded
quantum bitstring must use this order unless a field explicitly says `display_order`.

## Optimization direction

All project objectives are minimized. Higher importance reduces energy; retaining a
redundant pair increases energy. The constant term may be omitted for optimization, but it
must be restored when comparing algebraic forms numerically.

## Once-counted polynomial convention

The canonical polynomial is

`E(x) = C + sum_i q_i x_i + sum_{i<j} q_ij x_i x_j`.

Expanding the cardinality penalty gives

- `C = lambda k^2`
- `q_i = -alpha I_i + lambda (1 - 2k)`
- `q_ij = beta R_ij + 2 lambda`

because `x_i^2 = x_i`. Pair coefficients are counted exactly once. If code instead uses a
symmetric matrix in `x^T Q x`, it must place `q_ij / 2` in both off-diagonal cells.

## QUBO-to-Ising mapping

Use `x_i = (1 - z_i) / 2`, so an Ising eigenvalue `z_i = +1` maps to `x_i = 0` and
`z_i = -1` maps to `x_i = 1`. Tests must compare the direct subset score, polynomial QUBO
energy, symmetric-matrix energy, and Ising energy plus offset for all 256 assignments.

## Bitstring decoding

Qiskit commonly displays the highest-index classical bit on the left. The project must keep
that display string unchanged in raw results and produce a separate channel-order vector.
For example, displayed `10000101` represents channel-order `[1, 0, 1, 0, 0, 0, 0, 1]`
when classical bit i stores channel i. This example must be covered by a unit test.

## Numerical policy

- Use float64 for graph, importance, QUBO, and equivalence calculations.
- Use absolute tolerance `1e-10` for algebraic energy-equivalence tests unless a test records
  a justified stricter or looser value.
- Use the model's native inference dtype for neural-network output equivalence and record the
  chosen absolute and relative tolerances.
- Resolve exact ties lexicographically in channel order and record all tied optima.
