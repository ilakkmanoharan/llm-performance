✅ LEVEL 5 — Expert Difficulty (Deep Cross-Section + Changed Code)
Question (Level 5)

Question: 
The initial partitioning strategy uses random tokens:
Tokens(nodeA) = {A1, A2}
Tokens(nodeB) = {B1, B2}
Resulting ranges:
(A1, B1], (B1, A2], (A2, B2], (B2, A1]
Replica synchronization builds a Merkle tree per range:
Tree(A1,B1).root = H( left || right )
When a new node joins with token X, the range (A1, B1] is split:
Old:
(A1, B1]
Tree(A1,B1)
New:
(A1, X]
(X, B1]
Assume the old code incorrectly attempts to “reuse” the old Merkle tree root:
// buggy pseudo-logic
Tree(A1,X).root = Tree(A1,B1).root
Explain why this reuse is invalid and what breaks as a result.

⭐ Golden Response (Level 5)

Reusing the old root is invalid because the Merkle tree for (A1,B1] summarizes keys that now span two different intervals, so the hash no longer corresponds to the actual contents of (A1,X].
Referenced in: Partitioning Algorithm (range splits) + Replica Synchronization (Merkle tree must match exact range contents).

The original hash incorporates keys that belong to (X,B1], so assigning it to (A1,X] fabricates a tree that misrepresents which keys exist in that interval. The structure ceases to reflect the real dataset and breaks Merkle’s purpose: efficiently identifying divergence.

Correct logic must recompute each interval’s tree from the keys actually in that sub-range:
Tree(A1,X).root = H( computeHashes(keys in (A1,X]) )
Tree(X,B1).root = H( computeHashes(keys in (X,B1]) )

If the old root is reused, Dynamo cannot detect which replicas are out of sync in either of the new ranges. Anti-entropy becomes impossible to perform correctly until the trees are rebuilt, preventing the system from identifying or repairing divergent replicas.