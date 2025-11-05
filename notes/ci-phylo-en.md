---
title: "Tutorial for Computer Scientists: Felsenstein's Algorithm"
---

* TOC
{:toc}

## Computing Tree Parsimony Using Dynamic Programming

* We have a given tree and one alignment column that determines the base at each leaf
* The goal is to calculate the minimum number of mutations that must have occurred in the evolutionary history according to this tree to obtain these specific bases at the leaves
* Subproblem: 
$N[v,a]$: how many changes are needed in the subtree below vertex $v$, if vertex $v$ has symbol $a$?
* We compute $N[v,a]$ from leaves to root
* Recurrence for internal vertex $v$ with children $y$ and $z$: 
$N[v,a] = \min_b\{N[y,b]+[a\neq b]\}+\min_c\{N[z,c]+[a\neq c]\}$
  * we denote as $[condition]$ the value 1 if the condition holds and 0 if it does not
* We need to define the trivial case at leaf $v$ which has symbol $b$ according to the alignment:
$N[v,a] = [a\ne b]$


## Felsenstein's Algorithm 1981

* We have a given tree $T$ with edge lengths and bases at leaves (one alignment
  column) and a substitution model (given e.g. by rate matrix $R$).
  Let us compute the probability that the model produces exactly this
  combination of bases at the leaves.
- Notation:
    - Let $X_v$ be a random variable representing the base at vertex $v$ and let
      $x_v$ be a specific base at leaf $v$.
    - Let the leaves be $1\dots n$ and internal vertices $n+1\dots 2n-1$, where the root is $2n-1$.
    - Let $p_v$ be the parent of vertex $v$ and let the edge length from $v$ to its parent be $t_v$.
    - Let $\Pr(a\stackrel{t}{\rightarrow} b)$ be the probability that $a$ changes to $b$ in time $t$
      (computed from matrix $R$, see previous tutorial).
        - E.g. in the Jukes-Cantor model
            $\Pr(A\stackrel{t}{\rightarrow} A) = (1+3e^{-\frac{4}{3} t})/4$,
            $\Pr(A\stackrel{t}{\rightarrow} C) = (1-e^{-\frac{4}{3} t})/4$
    - Let $q_a$ be the probability of base $a$ at the root (equilibrium of matrix $R$)
      - E.g. in the Jukes-Cantor model $q_a = 1/4$
- If we knew the bases at all vertices, we have
  $\Pr(X_1=x_1 \dots X_{2n-1}=x_{2n-1}|T,R)=q_{x_{2n-1}} \prod_{v=1}^{2n-2}P(x_v|x_{p_v}, t_v)$
- We want the probability
  $P(X_1=x_1, X_2=x_2,\dots X_n=x_n|T,R)=\sum_{x_{n+1}\dots x_{2n-1}\in \\{A,C,G,T\\}^{n-1}} P(X_1=x_1 \dots X_{2n-1}=x_{2n-1}|T,R)$
- Computing the sum over exponentially many assignments of values to internal vertices is inefficient; we can compute it faster using dynamic programming.
- Let $A[v,a]$ be the probability of data in the subtree rooted at $v$ if $X_v=a$
- We compute $A[v,a]$ from leaves to root
- At a leaf $A[v,a] = [a=x_v]$
- At an internal vertex $v$ with children $y$ and $z$ we have
  $A[v,a] = \sum_{b,c} A[y,b]A[z,c]\Pr(a\stackrel{t_y}{\rightarrow} b)\Pr(a\stackrel{t_z}{\rightarrow} c)$
- The total probability is $\Pr(X_1=x_1, X_2=x_2,\dots X_n=x_n\|T,R)=\sum_a A[r,a] q_a$ for root $r$.

### Complexity, Improvement

- Complexity $O(n\|\\Sigma\|^3)$.
- For non-binary trees exponential.
- Improvement
  $A[v,a] = (\sum_{b} A[y,b]\Pr(a\stackrel{t_z}{\rightarrow} c))\cdot (\sum_c A[z,c]\Pr(a\stackrel{t_z}{\rightarrow} c))$
- Complexity $O(n\|\\Sigma\|^2)$ even for non-binary trees.

### Missing Data

- If at some leaf we have an unknown base N, we set $A[v,a]=1$ for all $a$.
- Gaps in the alignment are handled similarly, although we could have a model that explicitly models them.

## Posterior Probability

We did not cover this, included for interest.

- What if we want to compute the probability $\\Pr(X_v=a \| X_1=x_1, X_2=x_2,\dots X_n=x_n,T,R)$? We are interested in the sequences of ancestral genomes.
- The algorithm is similar to forward-backward algorithm for HMMs.
- We need $B[v,a]$: the probability of data if we replace subtree $v$ with a leaf having base $a$.
- We compute $B[v,a]$ from root to leaves.
- At the root we have $B[v,a] = q_a$.
- At vertex $v$ with parent $u$ and sibling $x$ we have
    $B[v,a]=\sum_{b,c} B[u,b]A[x,c]\Pr(b\stackrel{t_v}{\rightarrow} a) \Pr(b\stackrel{t_x}{\rightarrow} c)$.
- The desired probability is $B[v,a]A[v,a] / \\Pr(X_1=x_1, X_2=x_2,\dots X_n=x_n\|T,R)$.

