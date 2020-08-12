[Back to index](index)

# Bloom Filters

kind of a "probabilistic dictionary"

- maintain the "fingerprints" of the elements of a set S
- `search(S, x)`
  - returns `no` if $x \not \in S$
  - returns `probably_yes` if $x \in S$, but also sometimes when $x \not \in S$

## Applications:

- hyphenation
- checking URLs in case they are malicious without storing entire set of malicious sites
  - let a user visit the URL if it is not malicious
  - warn the user if the URL might be malicious
  - can accomplish this with a 10 mb bloom filter, rather than a 500 mb list of sites

## Implementation

- bit array `BF` of size $m$, so we have `BF[0]`, `BF[1]`, ... `BF[m-1]`

- $t$ independent hash functions $h_1, h_2, ..., h_t$

  - $h_i : U \to \lbrace 0, 1, ..., m-1 \rbrace$
  - $h_i$ conforms to simple uniform hashing assumption

- `insert(S, x)`

  ```python
  def insert(S, x):
      for i in range(t):
          BF[hi(x)] = 1
  ```
  
- `search(S, x)`
  
  ```python
  def search(S, x):
      for i in range(t):
          if BF[hi(x)] == 0:
              return false
      else:
          return true
  ```
  
  
  - takes $\mathcal O(t)$ time

### Probability of a false positive

If $n$ is the number of insertions, $t$ is the number of hash functions, $m$ is the size of $BF$

$$
\begin{align*}
P(BF[i] = 0) &= P\left( \bigcap_{k=1}^n \bigcap_{j=1}^t h_j(x_k) \neq 1 \right) \\
&= \prod_{k=1}^n \prod_{j=1}^t P( h_j(x_k) \neq 1) \\
&= (1 - 1/m)^{nt} \\
&\approx (e^{-1/m})^{nt} = e^{-nt/m}
\end{align*}
$$

Let $q = P(BF[i] = 1) = 1 - e^{-nt/m}$

$$
\begin{align*}
P(\text{false positive}) &= P(BF[h_1(x)] = 1 \cap BF[h_2(x)] = 1\;\cap\; ...\; \cap\; BF[h_t(x)] = 1) \\
&\approx P(BF[h_1(x)] = 1) \cdot P(BF[h_2(x)] = 1)\; \cdot\; ...\; \cdot\; P(BF[h_t(x)] = 1) \\
&= q^t = (1 - e^{-nt/m})^t
\end{align*}
$$

Using calculus, to minimize $P(\text{false positive})$, we should choose $\displaystyle t = \frac{m \ln 2}{n} \approx 0.69 \frac{m}{n}$

