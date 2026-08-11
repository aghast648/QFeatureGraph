# Results Schema

Each run writes to `results/runs/<run_id>/` locally. Curated summaries may later be copied to
a versioned directory under `results/` and committed.

## Required run files

| File | Required content |
|---|---|
| `config.yaml` | Exact configuration snapshot |
| `manifest.json` | Run ID, timestamps, Git state, environment, hardware and hashes |
| `metrics.json` | Split-specific loss, accuracy, size, parameters and timing metadata |
| `importance.csv` | Channel, raw loss change, transformed importance and dead flag |
| `correlation.csv` | Signed Pearson matrix in channel order |
| `redundancy.csv` | Positive-part redundancy matrix in channel order |
| `qubo.json` | Convention, constant, linear, pair and Ising coefficients |
| `selections.json` | Selector, displayed bitstring, channel vector, subset and energy |
| `qaoa.json` | Parameters, evaluations, sample counts, feasibility and objective gap |
| `events.jsonl` | Warnings, failures, fallbacks and protocol-relevant events |

## Common fields

All structured files include `schema_version`, `run_id`, `experiment_name`, `seed`,
`git_commit`, `config_hash`, and `channel_order`. Missing measurements are JSON `null` with
an explanation in `events.jsonl`; they are never silently replaced by zero.

## Selection record

A selection record contains the selector name, retained count, ordered channel list,
channel-order binary vector, optional raw display-order bitstring, direct objective score,
QUBO energy, feasibility, rank, and tie-group identifier.
