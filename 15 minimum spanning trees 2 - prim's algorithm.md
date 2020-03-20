[Back to index](index)

# Prim's Algorithm

## Minimum Spanning Trees

For a weighted graph $G = (V, E)$ with weights $wt:E \to \mathbb R$, a minimum spanning tree $T$ is comprised of every vertex in $V$ and a subset of the edges in $E$ such that the total weight of $T$ is the least of all spanning trees.

- Kruskal's Algorithm finds an MST using a greedy strategy

## Prim's Algorithm

- initialize tree with some vertex $s$
- given a tree, find the next edge by choosing the minimum weight edge that would connect one new vertex to the tree

```python
def Prim(G, wt, s):
    R = set(s)
    F = empty set
    while R != V:
        (u, v) = min wt edge connecting
                 a node u in R to a node v not in R
        R = union(R, v)
        F = union(F, (u, v))
```

#### Runtime

- loop runs until $R$ contains every vertex, gains one new vertex each time, so runs $n - 1$ times
- finding $(u, v)$ each loop is the hard part

### Finding min $wt$ edge $(u, v)$ with Adjacency Matrix

For every $v \notin R$, $near[v] = u \text{ such that } \forall w \in R, wt(u, v) \leq wt(w, v)$ (and `NIL` if there does not exist an edge $(u, v)$ where $u \in R$)

```python
def Prim(G, wt, s):
    R = set(s)
    F = empty set
    for each v in V - {s}:  # initialize near
        if (v, s) in E:
            near[v] = s
        else:
            near(v) = NIL
    while R != V:
        v = node in R where near(v) != nil
            with smallest wt(v, near(v))
        R = union(R, v)
        F = union(F, (u, v))
        for each w in R:
            if (w, v) in E and near(w) = NIL
               or wt(w, v) < wt(v, near(v)):
                near(w) = v
    return F
```

Assuming we are using an adjacency matrix, this implementation's runtime is $\mathcal O(n^2)$ - better than Kruskal's for dense graphs (though with a better implementation, Kruskal's can be just as fast for sparse graphs)