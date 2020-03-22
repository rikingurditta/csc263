[Back to index](index)

# Hash Tables

Another implementation for the dictionary ADT, but has (under suitable assumptions) constant time operations

## Relevant sets

- the universe: $U = \lbrace k : k \text{ is a possible key} \rbrace$
- keys in use: $K = \lbrace k : k \text{ is used to store something} \rbrace$
  - size of the dictionary is $n = \vert K\vert $

## Naive Implementation - Direct Addressing

Have an array `S` of size $\vert U\vert $, then for a key $k \in \lbrace 0, ..., \vert U\vert  - 1 \rbrace$, store its value in `S[k]`, or if that key is not present, store  a flag in `S[k]` marking it empty

e.g. if $\vert U\vert  = 4$ and we `insert(2, 1)`, then `arr = [-, -, 1, -]`

### But what if $\vert U\vert $ is too big?

- what if $\vert U\vert $ is too big for an array of its size to be stored
- what if $\vert U\vert  >> \vert K\vert $ , so an array of size $\vert U\vert $ would be incredibly wasteful

## Hash Functions

**A hash function maps a large set of inputs to a small set of things to use as keys**

If we have an array `S` of size $m$, we can use a hash function $h : U \to \lbrace 0, ..., m - 1 \rbrace$ to "hash" keys into places in `S`

------

Don't worry about actual hash functions, they are mysterious.

*"People go to Hogwarts for years to learn how to do this sort of thing."*

​		- Danny Heap, on proving the effectiveness of hash functions

------

### Collisions

If $h : U \to K$ and $\vert K\vert  < \vert U\vert $, then $h$ cannot be injective, so there could be a *collision*: two different keys share the same hash

**Solutions**:

- chaining: each slot of array is a (doubly) linked list storing all keys that hash to that slot
- open addressing (commonly used)

### Chaining

- `insert(S, k)`
  - if nothing else has taken slot $h(k)$ thus far, store a node with $k$ as the `head` of `S[h(k)]`
  - otherwise, store $k$ at `S[h(k)]` and point to node that was previously there
- `delete(S, k_ptr)`
  - given a pointer to the node `n` storing $k$, set `n.prev.next = n.next` (linked list deletion)
- `search(S, k)`
  - check if `S[h(k)]` stores `k`
  - if not, check`S[h(k)].next`
    - if not, repeat this step
  - if the end of a linked list is reached, then `k` is not in the dictionary

### SUHA - Simple Uniform Hashing Assumption

We assume that $h(k)$ is equally likely to take on all values in $\lbrace 0, ..., m-1 \rbrace$

#### Application to chaining

SUHA is equivalent to assuming that $\forall i, j \in \lbrace 0, ..., m - 1 \rbrace, n_i = n_j$ where $n_x$ is the number of keys that are hashed to $x$

$\displaystyle \sum_{i = 0}^{m - 1} E[n_i] = n$, and by SUHA all $n_i \approx n_j$, so each $\displaystyle n_i \approx \frac{n}{m}$ (we call $\alpha = n / m$ the *load factor*)

Performance is bad if $\alpha$ gets too big (as $n$ grows), but we can improve this by expanding the hash table (increasing $m$)

