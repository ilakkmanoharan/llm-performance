LEVEL 4 — Advanced Difficulty (Edge-Case Scenario)
Question (Level 4)

Suppose a range (B,C] currently has inconsistent replicas across nodes. Replica synchronization maintains its Merkle tree:
Tree(B,C).root = H(...)
A joining node inserts token X, splitting the range:
Old:
(B, C]
Tree(B,C)
New:
(B, X]
(X, C]
Because the old range contained divergence, the tree summarizing (B,C] no longer matches either sub-range.
Explain why Strategy 1 is brittle in this edge-case.

Golden Response (Level 4)

This situation is brittle because splitting an already inconsistent range destroys the Merkle structure that Dynamo uses to identify which keys are out of sync, forcing the system to rebuild trees and re-scan data before repair can proceed.
Referenced in: Partitioning Algorithm (token insertion reshapes ranges) + Replica Synchronization (Merkle trees summarize exact key-range contents).

The tree for (B,C] encodes the hash of all keys across the entire interval. Once the range becomes (B,X] and (X,C], the original hash no longer reflects either sub-range and therefore cannot be used to detect divergence within each smaller interval.

Fresh trees must be constructed for (B,X] and (X,C], which requires reading all keys in each sub-range. The system cannot identify which replicas disagree until these new trees exist.

This delays anti-entropy, increases the window during which replicas remain inconsistent, and slows node join because repair work cannot begin until the new Merkle structures are regenerated.