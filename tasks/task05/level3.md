LEVEL 3 — Higher Reasoning Difficulty

Question (Level 3)

Strategy 1 assigns T random tokens per node, producing irregular ranges such as:
(A,B], (F,G], (M,P]
Replica synchronization builds one Merkle tree per range:
Tree(A,B)
Tree(F,G)
Tree(M,P)
If a new node joins and receives tokens {X1, X2}, these ranges may split:
(A,X1], (X1,B]
(F,X2], (X2,G]
Dynamo reports in production that this type of bootstrapping sometimes took almost a day.
Explain strictly using these mechanisms why this happens.

Golden Response (Level 3)

Bootstrapping becomes extremely slow because random token placement causes many ranges to be reshaped at once, forcing extensive storage scans and Merkle-tree rebuilds across the system.
Referenced in: Partitioning Algorithm (random tokens per node) + Replica Synchronization (Merkle trees per range).

Whenever tokens {X1, X2} are added, each irregular range they intersect must be split—often in several places because the ranges have no predictable structure. Each original owner must then read its entire persistent store to isolate the keys belonging to each new sub-range.

This reshaping invalidates the Merkle trees associated with (A,B], (F,G], and (M,P]. Because the tree captures hashes over the entire original interval, its structure cannot map onto newly split boundaries and must be recomputed for each new sub-range.

During busy seasons, these scans and rebuilds compete with foreground traffic. As a result, bootstrapping can extend into many hours or even a full day because the system must avoid harming customer-facing operations while performing these heavy background tasks.