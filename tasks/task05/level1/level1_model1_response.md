When a new node joins by inserting a token that splits an existing key-range, the original Merkle tree for that range becomes invalid because it encompasses keys now divided across two separate ranges, necessitating the recalculation of entirely new Merkle trees for each resulting sub-range to maintain per-range integrity for replica synchronization.

The recalculation occurs exactly because Merkle trees are hierarchical hash structures built bottom-up from individual key hashes within a specific range; splitting requires reprocessing all keys in the affected range to construct independent trees for the new sub-ranges, involving repeated hashing operations that are computationally intensive.

Cause: The split disrupts the existing tree's structure, as parent node hashes depend on child hashes tied to the full range's keys, forcing a full rebuild rather than a simple subtree extraction, since keys must be repartitioned and rehashed under new root nodes for each sub-range.

Consequence: This intensive recalculation consumes significant CPU resources on production nodes already handling high request volumes, severely prolonging the bootstrapping time—often to nearly a day during peak seasons—making it impossible to rapidly scale the system and respond to sudden load increases without extended downtime or degraded performance.

Referenced in: Experiences and insights gained by running Dynamo in production (Section 6).