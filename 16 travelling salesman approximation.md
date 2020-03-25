[Back to index](index)

# Metric Travelling Salesman Problem Approximation via MSTs

## Travelling Salesman Problem

Given a graph $G = (V, E)$, a Hamiltonian cycle of $G$ is a simple cycle that visits every vertex once (not every graph has a Hamiltonian cycle!)

The Travelling Salesman Problem is the task of finding the minimum weight Hamiltonian cycle in a graph

- called travelling salesman because it models a situation where a salesman would like to visit every city in the optimal way
- TSP is an NP-complete problem
  - NP-complete is a class of problems which do not have known efficient (polynomial time) solutions
  - every NP-complete problem is equivalent, so a solution to one problem would yield a solution to every problem
  - determining if it is possible to find a polynomial time solution to an NP-complete problem is one of the Millenium Prize Problems (P vs NP)
  - other NP-complete problems include:
    - boolean satisfiability (3-SAT)
    - determining if a graph has a Hamiltonian cycle
    - finding the maximum size clique (complete subgraph) in a graph
    - subset sum problem - given $X \lbrace x_1, ..., x_n \rbrace \subseteq Z$ and $c \in \mathbb Z$, find a subset of $X$ so that if  $s = \sum_{x \in X} x$, then $s < c$ and $s$ is as large as possible

### Metric TSP

A metric (as in math) is a distance function $d : X^2 \to \mathbb R^{\geq 0}$ which satisfies some properties (that agree with our intuitive understanding of distance)

For any points $x, y \in X$

- $d(x, y) \geq 0$
- $d(x, y) = 0 \Leftrightarrow x = y$
- $d(x, y) = d(y, x)$
- $d(x, y) + d(y, z) \geq d(x, z)$ (triangle inequality)

The metric TSP is the TSP problem with the additional assumption that the weight function on the graph $wt : E \to \mathbb R$ is a metric

- this is still NP-complete

## Approximation Algorithms

- when an optimal solution is difficult to find (e.g. if the problem is NP-complete), finding an approximate solution can be reasonable
- we would like an algorithm that finds a Hamiltonian cycle $H$ so that $wt(H) \leq c \cdot wt(O)$, where $c$ is some constant and $O$ is the optimal (least weight) Hamiltonian cycle
  - unfortunately this is not possible for general TSP (for any $c$) unless $P = NP$ :(
  - however, it is possible for metric TSP!

### Metric TSP 2-Approximation

("2-approximation" means the algorithm finds $H$ so that $wt(H) \leq 2 \cdot wt(O)$)

Suppose $O$ is a minimum weight Hamiltonian cycle of $G$ (optimal solution)

To find a good solution to metric TSP:

1. Find an MST $T$ of $G$
   - $O$ is a spanning graph with positive weights, so $wt(T) \leq wt(O)$ 
2. let $s \in V$ be some vertex, we can start looking for a cycle by doing a DFS on $T$ starting at $s$, recording when nodes are visited *and* when they are revisited - this gives a "pseudo-Hamiltonian" cycle $P_T$
   - this cycle contains each edge twice - once when a node is visited and once again when a node is left, going back to its parent - so $wt(P_T) = 2wt(T)$
3. we can turn $P_T$ into an actual Hamiltonian cycle $H_T$ by skipping vertices that have been visited before
   - with a path $a \to b \to c$, skipping $b$ is good because by the triangle inequality, the new path $a \to c$ has lesser weight
     - this holds because $wt$ is a metric
   - thus $wt(H_T) \leq wt(P_T) = 2wt(T) \leq 2wt(O)$