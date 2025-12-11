LEVEL 1 — Basic Difficulty

Question (Level 1)

Dynamo’s Partitioning Algorithm stores data by hashing keys onto a ring of token ranges, where ranges are defined by consecutive randomly assigned tokens. A node’s key-range therefore looks like:
(A, B]
(B, C]
(C, D]
Meanwhile, Replica Synchronization maintains one Merkle tree per key-range, represented conceptually as:
MerkleTree(range):
    root = H(child_left || child_right)
When a new node joins, it inserts new tokens—e.g., inserting token X between A and B, producing:
(A, X]
(X, B]
Using only these mechanisms, explain why this causes performance problems during bootstrapping.

Golden Response (Level 1)

Adding token X forces the system to split the original range and rebuild the Merkle trees for the new ranges, while also locating the keys that belong to each sub-range.
Referenced in: Partitioning Algorithm (random token boundaries) + Replica Synchronization (Merkle trees per range).

Because the new ranges didn’t previously exist, the node that owned (A,B] must rescan its storage to determine which keys belong to (A,X] and which belong to (X,B]. This happens because random token placement reshapes range boundaries whenever a new node joins.

The Merkle tree associated with (A,B] no longer matches either of the new ranges, so neither subtree of the original hash structure can be reused. A fresh Merkle tree must be built for each new interval.

This slows bootstrapping significantly because the system cannot hand off accurate replicas until both the range scans and the Merkle-tree rebuilds complete.