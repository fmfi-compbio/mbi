---
title: "Exercises for Computer Scientists: Advanced Sequence Alignment Algorithms"
---

* TOC
{:toc}

## Review of Dynamic Programming for Global Alignment

Consider, for example, scoring: match +1, mismatch -1, gap -1, and input sequences $X=x_1\dots x_m$ and $Y=y_1\dots y_n$. Let $s(x,y)$ be the score of letters *x* and *y*, i.e., 1 if they match and -1 otherwise. We have the recurrence:

$A[i,j]=\max\left\\{A[i-1,j-1]+s(x_i,y_j), A[i-1,j]-1, A[i,j-1]-1\right\\}$

  - How exactly would we implement this?
  - How do we compute the backtracing matrix B?
  - What is the time and memory complexity?

## Representation Using a Graph

Such dynamic programming can be represented as an acyclic directed graph:

  - Vertex (i,j) for each $0\le i\le m, 0\le j \le n$, i.e., for each cell of the DP table
  - Edge from (i-1,j-1) to (i,j) with weight $s(x_i,y_j)$
  - Edge from (i-1,j) to (i,j) with weight -1
  - Edge from (i,j-1) to (i,j) with weight -1
  - The sum of coordinates increases on every edge, so the graph cannot contain a cycle; it is acyclic
  - Every path from (0,0) to (m,n) corresponds to an alignment, and its weight is the alignment score (each edge is one column of the alignment)
  - The optimal alignment thus corresponds to the path with the maximum score

## Short Note on Directed Acyclic Graphs (DAGs)

  - We are given a directed acyclic graph with weighted edges, a start vertex *s*, and an end vertex *t*, and we want to find the path with the maximum weight from *s* to *t*.
  - Finding the maximum-weight path is generally NP-hard (similar to the Hamiltonian path problem)
  - However, in a DAG, we can solve it efficiently
  - First, we topologically sort the graph, i.e., order the vertices so that every edge goes from a vertex with a lower number to a vertex with a higher number. This can be done by a modified depth-first search in time $O(|V|+|E|)$.
  - Then we compute by dynamic programming, where $A[u]$ is the length of the longest path from *s* to *u*:
    $A[u] = \max_{v:v\rightarrow u\in E} A[v]+w(v\rightarrow u)$
    with $A[s]=0$ at the start, and at the end we have the path score in $A[t]$.
  - The computation time is $O(|V|+|E|)$.
  - Note that we also get the longest paths from $s$ to all vertices.

If we apply this algorithm to the graph for global alignment, we get exactly our recurrence (topological sorting can be omitted—the order from top to bottom and left to right is already topologically sorted). The advantage is that by modifying the graph, we can obtain solutions to various related problems without always inventing a new recurrence.

## Local Alignment

  - Alignment can start and end anywhere in the matrix
  - Add a start vertex *s* and an end vertex *t*
  - Add edges $s\to(i,j)$ and $(i,j)\to t$ with weight 0 for every (i,j)
  - Again, this is equivalent to the recurrence from the lecture

Variant: we want to align the entire string *X* to some part of string *Y* (e.g., mapping sequencing reads to a genome)

  - We only change the edges from *s* and to *t* (how?)

## Affine Gap Scoring

  - For example, gap opening o=-3, gap extension e=-1

```
A  -  -  -  T  C  G
A  C  G  C  T  C  C
1 -3 -1 -1  1  1  -1
```

### Incorrect Solution Using Dynamic Programming

We use standard dynamic programming for global alignment, but in the recurrence, we change the calculation of the gap penalty:

$A[i,j]=\max\left\\{A[i-1,j-1]+s(x_i,y_j), A[i-1,j]+c(i-1,j,up), A[i,j-1]+c(i,j-1,left)\right\\}$


* $c(i,j,s) = e$ if in cell $A[i,j]$ we have arrow $s$
* $c(i,j,s) = o$ if in cell $A[i,j]$ we have a different arrow


Why does this solution not work?

  - What if for cell (i,j) there are multiple equally good solutions with different arrows?
  - What if for cell (i,j) the best solution is, say, diagonal, but the second best is only 1 worse and has an up arrow?

This is a common mistake in dynamic programming:

  - For dynamic programming to be correct, it must hold that the optimal solution to a larger subproblem must contain the optimal solution to a smaller subproblem

### Correct Solution Using Dynamic Programming

Solution 1:

  - Add edges for entire contiguous gap segments with the correct score
  - $(i,j)\rightarrow (i,k)$ with score $o+(k-j)e$
  - $(i,j)\rightarrow (k,j)$ with score $o+(k-i)e$
  - Time $O(mn(m+n))$, i.e., cubic
  - Note, there are also paths that do not correspond to any correct score, e.g., $(i,j)\rightarrow (i+1,j)\rightarrow (i+2,j)$ has score 2o, but should be o+e. Fortunately, the edge $(i,j)\rightarrow (i+2,j)$ has a higher score o+e, so the path with score 2o is not used.

Solution 2:

  - Triple each vertex $(i,j)_d, (i,j)_h, (i,j)_v$
  - In the index, we remember where we came from to (i,j) (d=diagonal, h=horizontal, v=vertical)
  - If we go, for example, from $(i,j-1)_h$ to $(i,j)_h$, we continue an existing gap, so the score is e
  - If we go, for example, from $(i,j-1)_d$ to $(i,j)_h$, we start a new gap, so the score is o
  - What edges can we have? How many edges and vertices are there in the graph, and what is the complexity of the algorithm?

## Linear Memory: Hirschberg's Algorithm 1975

  - Classic dynamic programming requires time O(nm).
  - A trivial implementation also uses O(mn) memory. It stores the entire matrix A and the matrix B with backtracing arrows
  - To compute matrix A, we only need two rows: row i is computed only from row i-1, older rows can be discarded
  - But if we want to output the alignment, we still need O(mn) memory for the backtracing matrix B
  - Hirschberg's algorithm reduces memory to O(m+n), roughly doubles the time (still O(mn))
  - We go through the entire matrix and compute matrix A. We remember where our path crosses the middle row of the matrix
    - Let $B_k[i,j]$ be the largest index in row *k* through which the shortest path from (0,0) to (i,j) passes
  - How do we compute $B_k[i,j]$?
    - if $A[i,j] = A[i-1,j-1]+s(x_i,y_j)$, then $B_k[i,j]=B_k[i-1,j-1]$
    - if $A[i,j]=A[i-1,j]+1$, then $B_k[i,j]=B_k[i-1,j]$
    - if $A[i,j]=A[i,j-1]+1$, then $B_k[i,j]=B_k[i,j-1]$
    - This holds if $i>k$. For $i=k$ we set $B_k[i,j]=j$
  - If we already know $A[i-1,*]$ and $B_k[i-1,*]$, we can compute $A[i,*]$ and $B_k[i,*]$
    - Thus, we only need two rows of matrices $A$ and $B_k$
  - Let $k'=B_k[m,n]$. Then in the optimal alignment, $x_1,\dots,x_k$ aligns with $y_1\dots,y_{k'}$ and $x_{k+1}\dots x_m$ with $y_{k'+1}\dots y_n$
    - We use this in a recursive algorithm to compute the alignment:

```
    optA(l1, r1, l2, r2) { // align X[l1..r1] and Y[l2..r2]
        if(r1-l1 <= 1 ||  r2-l2 <=1) 
            solve using dynamic programming
        else {
            k=(r-l+1)/2;
            for (i=0; i<=k; i++) 
               compute A[i,*] from A[i-1,*]
            for (i=k+1; i<=r-l+1; i++) 
               compute A[i,*], B_k[i,*] from A[i-1,*], B_k[i-1,*]
            k2=B_k[r1-l1-1,r2-l2-1];
            optA(l1, l1+k-1, l2, l2+k2-1); 
            optA(l1+k, r2, l2+k2, r2); 
        }
    }
```

Time complexity:

  - Let N=nm (the product of the lengths of the two given strings)
  - At the top level of recursion, we run dynamic programming for the whole matrix. Time is $cN$.
  - At the second level, we have two subproblems of sizes $N_1$ and $N_2$, with $N_1+N_2\le N/2$ (from each column of matrix A, at most half the rows are recomputed).
  - At the third level, we have 4 subproblems $N_{1,1}, N_{1,2}, $N_{2,1}$, $N_{2,2}, with $N_{1,1}+N_{1,2} \le N_1/2$ and $N_{2,1}+N_{2,2} \le N_2/2$, so the total sum of subproblems at the second level is at most $N/4$
  - At the fourth level, the sum of subproblems is at most $N/8$, etc.
We get a geometric series $cN+cN/2+cN/4+\dots$ whose sum is $2cN$.

## Listing All Optimal Solutions

  - Consider, for example, "ordinary" global alignment.
  - Instead of remembering a single back arrow, we remember all arrows that led to the maximum score in $A[i,j]$.
  - Then we can recursively traverse and list all paths from (m,n) to (0,0) that consist only of the remembered arrows.
  - The time to output one path is polynomial, but there can be exponentially many paths!
  - Maybe instead we want just the number of such paths, or all pairs of letters that can be aligned together in some optimal alignment.
