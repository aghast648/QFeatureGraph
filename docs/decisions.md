# Decision Log

Use short immutable records. Add a new record when a scientific or architectural decision
changes; do not rewrite the historical reason.

| ID | Status | Decision | Reason |
|---|---|---|---|
| D001 | Accepted | Fashion-MNIST is the version 1 dataset | Small, accessible and reproducible |
| D002 | Accepted | Analyze eight post-ReLU `conv1` channels | Exact enumeration remains tractable |
| D003 | Accepted | Use global average channel responses | Produces one comparable value per example and channel |
| D004 | Accepted | Use signed Pearson correlation, then positive part | Preserve analysis while penalizing positive co-activation redundancy |
| D005 | Accepted | Preserve raw masking loss changes | Negative values are scientifically relevant |
| D006 | Accepted | Retain exactly four channels | Creates a clear 50 percent first-layer pruning target |
| D007 | Accepted | Exact enumeration is the reference solver | All 256 assignments can be verified |
| D008 | Accepted | QAOA uses ideal depth one in version 1 | Demonstrates integration without hardware claims |
| D009 | Accepted | QAOA does not imply quantum advantage | The problem is tiny and exactly solvable classically |
| D010 | Accepted | Reconstruct both `conv1` and `conv2` | Masking alone does not reduce stored parameters |
| D011 | Accepted | Hold out the official test set | Reduces test-set leakage |
| D012 | Accepted | Keep notebooks outside core logic | Protects testability and reproducibility |
