LEVEL 2 — Moderate Difficulty

Question (Level 2)

Under Dynamo’s initial Strategy 1, the ring might have tokens:
Tokens = {A, B, C, D}
Ranges = (A,B], (B,C], (C,D], (D,A]
Each range maintains its own Merkle tree:
Tree(A,B).root = H(...)
Tree(B,C).root = H(...)
If a joining node inserts two tokens {X1, X2}, several ranges are split:
(A, X1], (X1, B]
(C, X2], (X2, D]
Every sub-range must have its own Merkle tree.
Explain why Strategy 1 causes severe bootstrapping cost when this occurs.

Golden Response (Level 2)

Bootstrapping becomes expensive because splitting multiple ranges forces the system to rescan data for several new intervals and to regenerate multiple Merkle trees from scratch.
Referenced in: Partitioning Algorithm (random-token splits) + Replica Synchronization (Merkle trees per key-range).

When {X1, X2} appear in the ring, several unrelated ranges are fragmented at once. The nodes responsible for those ranges must perform storage scans to determine which keys belong in each newly formed interval.

Each old Merkle tree—for example Tree(A,B)—summarizes the entire original interval and cannot be reused for (A,X1] or (X1,B]. The internal hash structure no longer aligns with the new boundaries, so new Merkle trees must be constructed for each split.

Because several ranges may be affected in one join event, the cost multiplies, slowing down the system’s ability to add nodes while maintaining steady performance.
