# Types of indexes

## Hash

A **hash index** provides the ability to quickly find a tuple ID (TID) by a particular index key. Roughly speaking, it is simply a hash table stored on disk. The only operation supported by a hash index is search by the equality condition.

## B-tree

A **B-tree** is a data structure that enables you to quickly find the required element in leaf nodes of the tree by going down from its root. For the search path to be unambiguously identified, all tree elements must be ordered. B-trees are designed for ordinal data types, whose values can be compared and sorted.
