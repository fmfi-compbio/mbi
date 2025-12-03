---
title: "Tutorial for Computer Scientists: Integer Linear Programming"
---

* TOC
{:toc}


## Practical Programs for NP-hard Problems

  - Sometimes we want to find the optimal solution to an NP-hard problem
  - One option is to reduce it to another NP-hard problem for which
    there are fairly good practical programs, such as **integer
    linear programming (ILP)**
  - These find optimal solutions, many instances are solved in reasonable time,
    but some may run very long time
  - [CPLEX](http://www-01.ibm.com/software/integration/optimization/cplex-optimizer/)
    and [Gurobi](http://www.gurobi.com/html/academic.html) are commercial
    ILP packages, academic licenses are free
  - Non-commercial programs, such as [SCIP](http://scip.zib.de/),
    [SYMPHONY](https://projects.coin-or.org/SYMPHONY) in the COIN-OR project,
    [GLPK](https://www.gnu.org/software/glpk/)    
  - [Minisat](http://minisat.se/) open source SAT solver, also
    Lingeling, glucose, CryptoMiniSat, painless
  - [Concorde](http://www.tsp.gatech.edu/concorde.html) - TSP solver 
    solves the traveling salesman problem with symmetric distances,
    free for academic use
      - For interest: [TSP art](http://www.oberlin.edu/math/faculty/bosch/tspart-page.html)

## Definition of ILP

### Linear program

  - We have real variables $x_1,\dots,x_n$, the goal is to minimize (or maximize) some linear
    combination $\sum_i a_i x_i$ where $a_i$ are given weights.
  - We also have several constraints in the form of linear equalities or
    inequalities, e.g. $\sum_i b_i x_i \le c$
  - We seek values of the variables that minimize the objective sum,
    but satisfy all constraints
  - Linear programs can be solved in polynomial time


### Integer linear program (ILP)

  - A program in which all/some variables must have integer
    values, or even only allow values 0 and 1.
  - This is an NP-complete problem.

## How to Write (NP-hard) Problems as ILP

Knapsack

  - Problem: given items with weights $w_1,\dots, w_n$ and values
    $c_1,\dots, c_n$. Which items to select so that total weight is at most T
    and the total value is maximized?
  - Use binary variables $x_1,\dots, x_n$, where $x_i = 1$ if
    we took the i-th item and 0 otherwise.
  - We want to maximize $\sum_i c_i x_i$ subject to $\sum_i w_i x_i \le T$

Set cover:

  - We have $n$ subsets $S_1,\dots, S_n$ of the set $\{1,\dots, m\}$. We want to select as
    few input sets as possible so that their union is the whole set $\{1,\dots, m\}$
  - Binary variables $x_i=1$ if we select the i-th set
  - We want to minimize $\sum_{i=1}^n x_i$ subject to, for each $j \in \{1,\dots,m\}$
    $\sum_{i:j\in S_i} x_i \ge 1$
  - We have $m$ inequalities, one for each element to be covered.

## RNA Sequence Alignment with Structure

Given two RNA sequences $X_1,\dots, X_n$ and $Y_1,\dots, Y_m$
and for each we have a set of pairs
of bases (positions within the sequence) that could be present in
the secondary structure (pair sets $P_X$ and $P_Y$).

  - The set of pairs may be a specific known secondary structure of the
    sequence or a larger set of pairs that could occur in the structure,
    for example pairs with high probability of being paired in an SCFG model,
    or even all pairs of complementary bases. 
 
The goal is to find the optimal alignment of these two sequences, using
the usual scoring for matches, mismatches, and gaps, but additionally
adding a bonus for matches in the secondary structure. Two potential pairs, one from
each sequence, are considered aligned if the bases at both ends are
aligned to each other. In scoring, we select a subset of aligned pairs
so that each base is in at most one pair, and assign a positive score
to each such pair.

This formulation allows us to solve several problems:
- align two RNAs with known structures (may contain pseudoknots), giving a bonus if both ends of a pair are aligned to a pair in the other sequence (without pseudoknots this can be solved in polynomial time, how?)
- find a common structure for two RNA sequences whose RNA structure is unknown, considering all pairs of complementary bases as potential pairs on input
- align an RNA sequence to another RNA sequence with known structure (considering all complementary pairs in the first sequence as potential pairs and the known structure pairs in the second sequence as potential pairs)


Example: if match is 1, mismatch and gap -1, pair >10/3, the first
alignment wins, if pair <10/3, second wins (of these two)
```
 [ [[    ] ]]
-GCGGAUAACCCC
 |   |      |  3 matches, 5 mismatches, 4 gaps, 3 pairs
GG-AUA-CCA-UC
 [ [[    ] ]]

( ((    ) ) )
GCGGAUAACCC-C
  ||||| ||  |  8 matches, 1 mismatch, 3 gaps, 0 pairs
--GGAUA-CCAUC
   (((    )))
```
X=GCGGAUAACCC, Y=GGAUACCAUC,
$P_X$={(1,12),(3,11),(4,9)}, $P_Y$={(2,10),(3,9),(4,8)}


Constants
- $a_{i,j}$ score for aligning bases $X_i$ and $Y_j$ (match or mismatch)
- $g$ gap penalty in alignment
- $p$ bonus for aligned pair

Variables (all binary):
- $x_{i,j}$ whether $X_i$ and $Y_j$ are aligned
- $z_{1,i}$ whether $X_i$ is unaligned
- $z_{2,j}$ whether $Y_j$ is unaligned
- $y_{i,j,k,\ell}$ whether $X_i$ and $Y_j$ are aligned, $X_k$ and $Y_\ell$ are aligned, and $i$ and $k$ are chosen as a pair, $j$ and $\ell$ as a pair (this is defined only for values where $(i,k)\in P_X$ and $(j,\ell)\in P_Y$ and $i<k$, $j<\ell$)

Maximize
- $\sum_{i,j} a_{i,j} x_{i,j} + g\cdot(\sum_i z_{1,i} + \sum_j z_{2,j}) + p\cdot \sum_{i,j,k,\ell} y_{i,j,k,\ell}$

Constraints
- $z_{1,i} + \sum_j x_{i,j}=1$ for each $i$ (each base from $X$ is aligned exactly once or unaligned)
- $z_{2,j} + \sum_i x_{i,j}=1$ for each $j$ (the same for $Y$)
- $x_{i,j}+x_{k,\ell}\le 1$ for each $i,j,k,\ell$ such that $i<k$ but $j>\ell$ (prohibit crossing pairs)
- $\sum_{k,\ell} y_{i,j,k,\ell}\le x_{i,j}$ for each $i,j$ (if $i,j$ is not aligned, no pairs containing $i$ and $j$ will be counted, and if aligned, at most one such pair)
- $\sum_{i,j} y_{i,j,k,\ell}\le x_{k,\ell}$ for each $k,\ell$ (similarly for the other side)

What is the size of the program in terms of $m$ and $n$? Estimate the number of variables, inequalities, all nonzero terms. Which parts shrink if $P_X$ and $P_Y$ are relatively small?


Source:

Bauer, Markus, Gunnar W. Klau, and Knut Reinert. [Accurate multiple sequence-structure alignment of RNA sequences using combinatorial optimization.](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-8-271) BMC Bioinformatics 8 (2007): 1-18.

Bauer, Markus, Gunnar W. Klau, and Knut Reinert. [An exact mathematical programming approach to multiple RNA sequence-structure alignment.](https://www.erudit.org/en/journals/aor/2008-v3-n2-aor3_2/aor3_2art03.pdf) Algorithmic Operations Research 3.2 (2008): 130-146.



### Protein Threading

Similar problem, shown for interest, not covered in detail 

  - Goal: protein A has known sequence and structure, protein B only
    sequence. We want to align proteins A and B, taking into account the
    known structure, i.e. if two amino acids are close in A, their
    equivalents in B should be "compatible".
  - We want to solve this problem by identifying cores in structure A,
    which should remain conserved in evolution without insertions and
    deletions and in the same order. These cores are separated by loops,
    whose length can vary arbitrarily and whose alignments will not be
    scored.
  - Problem formulation: Given sequence $B=b_1,\dots,b_n$, $m$ lengths of cores
    $c_1,\dots,c_m$ and two scoring tables. The first table $E_{i,j}$ express how well
    sequence $b_j..b_{j+c_i-1}$ fits into the sequence of core $i$. The second table $F_{i,j,k,\ell}$
    expresses how well cores $i$ and $k$ would interact if they had
    sequences starting in $B$ at positions $j$ and $\ell$. The task is to choose
    core positions in sequence $B$ denoted $x_1<x_2<\dots <x_m$ so that no two cores overlap and
    the highest score is achieved.
  - Note: we did not discuss how to choose cores and scoring tables,
    which is a modeling, not algorithmic problem (one could try e.g.
    probabilistic models)

#### Protein Threading as ILP

  - Variables in the program:
      - $x_{i,j}=1$ if the start of the i-th core is aligned with $b_j$
      - $y_{i,j,k,\ell}=1$ if the start of the i-th core is at $b_j$ and the start of the k-th core is
        at $b_\ell$ ($i<k$, $j<\ell$)
  - Want to maximize $\sum E_{i,j} x_{i,j} + \sum F_{i,j,k,\ell} y_{i,j,k,\ell}$
  - Constraints:
      - $\sum_j x_{i,j}=1$ for each $i$ (each core is placed exactly once)
      - $x_{i,\ell}+x_{i+1,k}\le 1$ for all $i,k,\ell$, where $k<\ell+c_i$ (to
        prevent overlapping cores $i$ and $i+1$)
      - $y_{i,j,k,\ell}\le x_{i,j}$ for all $i,j,k,\ell$, where $i<k$, $j<\ell$
      - $y_{i,j,k,\ell}\le x_{k,\ell}$ for all $i,j,k,\ell$, where $i<k$, $j<\ell$
      - $y_{i,j,k,\ell}\ge x_{i,j}+x_{k,\ell}-1$ for all $i,j,k,\ell$, where $i<k$,
        $j<\ell$

  Note that last three sets of constrains ensure that $y_{i,j,k,\ell}=1$ if and only if both
  $x_{i,j}=1$ and $x_{k,\ell}=1$. This could be achieved for a given $i,j,k,\ell$ by a single
  quadratic constraint $y_{i,j,k,\ell} = x_{i,j} \cdot x_{k,\ell}$, but ILP solvers do not allow quadratic constraints.

For consideration:

  - What will be the size of the program as a function of $n$ and $m$?
  - What if not all the cores interact with each other? Can we save on program size?
  - Why did the authors introduce cores at all, and how would we change the program
    if we wanted to consider each amino acid separately?

Source:

  - Jinbo Xu, Ming Li, Dongsup Kim, and Ying Xu. [RAPTOR: optimal
    protein threading by linear programming.](http://ttic.uchicago.edu/~jinbo/SelectedPubs/RAPTOR.pdf) Journal of bioinformatics
    and computational biology 1, no. 01 (2003): 95-117.

