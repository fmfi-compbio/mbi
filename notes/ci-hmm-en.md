---
title: "Tutorial for Computer Scientists: Algorithms for HMMs"
---

* TOC
{:toc}

## Hidden Markov models (HMMs) Recap

HMM Parameters:

  - $a_{u,v}$: transition probability from state $u$ to state $v$
  - $e_{u,x}$: emission probability of symbol $x$ in state $u$
  - $\pi_{u}$: probability of starting in state $u$

  - Sequence $S = S_1 S_2 \dots  S_n$
  - Annotation $A = A_1 A_2 \dots A_n$

$\Pr(S, A) = \pi_{A_1} e_{A_1,S_1} \prod_{i=2}^n a_{A_{i-1, A_i}} e_{A_i, S_i}$

**Training:** The process of estimating the probabilities $a_{u,v}$ and $e_{u,x}$ in the model from training data.

**Inference:** The process of applying the model on new data to compute some unkwnon quantity.

For example, finding, for an input sequence $S$, an annotation $A$ that emits sequence $S$ with the highest probability.


## Inference via the most probable path, Viterbi Algorithm

We seek the most probable sequence of states $A$, i.e., $\arg\max_A \Pr(A, S)$. We solve this using dynamic programming.

  - Subproblem $V[i,u]$: Probability of the most probable path that ends after $i$ steps in state $u$ and generates $S_1 S_2 \dots S_i$.
  - Recurrence:
      - $V[1,u] = \pi_u e_{u,S_1}$ (\*)
      - $V[i,u] = \max_w V[i-1, w] a_{w,u} e_{u,S_i}$ (\*\*)

Algorithm:

1.  Initialize $V[1,*]$ according to (\*)
2.  for $i=2$ to $n$ (the length of the string)
      -  for $u=1$ to $m$ (the number of states)
          - compute $V[i,u]$ using (\*\*)
3.  The maximum $V[n,u]$ of states $u$ is the probability of the most probable path

To output the annotation, for each $V[i,u]$ we remember the state $w$ that led to the maximum value in formula (\*\*).

Complexity: $O(nm^2)$

Note: For long sequences, the numbers $V[i,u]$ will be very small and arithmetic underflow may occur. In practice, we use logarithmic values, replacing multiplication with addition.

## The Forward Algorithm

We want to compute the total probability of generating sequence $S$, i.e. $\sum_A Pr(A,S).$ The algorithm is similar to Viterbi.

Subproblem $F[i,u]$: probability that after $i$ steps we generate $S_1, S_2, \dots S_i$ and end up in state $u$.

$F[i,u] = \Pr(A_i=u\wedge S_1, S_2, \dots, S_i) = \sum_{A_1, A_2, \dots, A_i=u} \Pr (A_1, A_2, \dots, A_i \wedge S_1, S_2, \dots, S_i)$

$F[1,u] = \pi_u e_{u,S_1}$

$F[i,u] = \sum_v F[i-1,v] a_{v,u} e_{u,S_i}$

The resulting total probability is $\sum_u F[n,u]$


## Posterior probability of a state

* Posterior probability of state $u$ at position $i$:
$\Pr(A_i = u | S_1 \dots S_n)$.

* This is includes probabilities of all paths that pass thriugh $u$ at position $i$ but can be arbitrary before and after.
* We want to compute this for all $i$ and $u$.
* We run the forward algorithm and its symmetric version, the backward algorithm, which computes values $B[i,u]=\Pr(S_{i+1}\dots S_n | A_i = u)$.
* Posterior probability of state $u$ at position $i$: $\Pr(A_i=u|S_1\dots S_n) = F[i,u] B[i,u] / \sum_u F[n,u].$
* We can compute the posterior probablity for all pairs $u$, $i$ in $O(nm^2)$ total time. 
* One use of such probabilities: visualize, which states in the most probbale annotation are more reliable (with higher posterior) and which are less certain.


## The Backward Algorithm

We want to compute the probability of the right part of the sequence $S_{i+1}\dots S_n$ given the current state $u$ at position $i$, i.e., $\Pr(S_{i+1}\dots S_n|A_i=u)$.

Subproblem $B[i,u]$: probability of generating $S_{i+1}, S_{i+2}, \dots, S_n$ from state $u$.

$B[n,u] = 1$

$B[i,u] = \sum_v B[i+1,v] a_{u,v} e_{v,S_{i+1}}$


## Posterior Decoding

Recall that the Viterbi algorithm seeks a single path with the highets overall probability of generating $S$.

Instead, we can choose at each position $i$ the state $u$ with the highest posterior probability, resulting in a different possible annotation.

Advantage: this annotation takes into account all state paths not just the one most probable. Often there are may state paths with similar probbailities. 

Disadvantage: The annotation obtaned in this way may occassionally have zero probability (e.g., the number of coding bases in a gene is not divisible by 3).


## HMM Training

  - State space + allowed transitions are usually set manually
  - Parameters (transition, emission, and initial probabilities) are set automatically from training sequences
      - If we have annotated training sequences, we simply count frequencies
      - If we have only unannotated sequences, we try to maximize the likelihood of the training data in the model. Heuristic iterative algorithms are used, e.g., Baum-Welch, which is a version of the more general EM (expectation maximization) algorithm.
  - The more complex the model and the more parameters, the more training data is needed to avoid overfitting, i.e., the situation where the model fits peculiarities of the training data but not other data.
  - Model accuracy is tested on separate test data not used for training.

## Designing the State Space of the Model

  - Promoter + several prokaryotic genes
  - Repeats in introns: multiple path problem
  - Intron has length at least 10

## Generalized HMM

Not covered in class, included for interest

  - Imagine an HMM with two states, e.g., gene / non-gene, each with a transition to itself and to the other state
  - Task: Let $p$ be the probability of staying in the same state, $(1-p)$ the probability of switching to the other state. What is the probability of staying in a state for exactly $k$ steps ($k\geq1$)?
      - Solution: $p^k (1-p)$
      - This distribution is called geometric, and the probability decreases exponentially with increasing $k$
  - Looking at the histogram of real gene/exon and other region lengths, it usually does not resemble a geometric distribution, but may look like a normal distribution with a certain mean and variance
      - Simple HMMs do not model this phenomenon well
  - Generalized HMMs (semi-Markov) allow arbitrary length distributions in a state. The model enters a state, generates a length $k$ from this distribution, then generates $k$ symbols from the corresponding emission table, and finally decides which transition to take out of the state
  - Task: How do we compute the probability of a specific sequence and a specific state path with lengths? (introduce suitable notation)
  - Task: How should the Viterbi algorithm be modified for this model? What will its complexity be?
      - Complexity will grow quadratically with sequence length, previously it grew linearly
  - Now suppose the length distribution has an upper bound $D$ so that all lengths greater than $D$ have zero probability.
      - Task: How does this restriction affect the complexity of the Viterbi algorithm?
      - Task: Propose how to model a generalized HMM with length distribution bounded by $D$ using normal states, i.e., replace one generalized state with a suitable sequence of $D$ ordinary states.

## Pair HMM

Not covered in class, included for interest

  - Emits two sequences
  - In one step can emit:
      - letters in both sequences at once
      - a letter in one sequence
      - a letter in the other sequence

Example: HMM with one state $v$, such that

  - $e_{v,x,x}=p_1$
  - $e_{v,x,y}=p_2$ ($x\ne y$),
  - $e_{v,x,-}=p_3$,
  - $e_{v,-,x}=p_3$
  - such that the sum of emission probabilities is 1
  - What does the most probable path represent in this HMM?

More complex HMM: three states $M$, $X$, $Y$, all fully interconnected

  - $e_{M,x,x}=p_1$
  - $e_{M,x,y}=p_2$ ($x\ne y$),
  - $e_{X,x,-}=1/4$,
  - $e_{Y,-,y}=1/4$,
  - What does the most probable path represent in this HMM?

**Viterbi Algorithm for Pair HMM**

  - $V[i,j,u]$ = probability of the most probable sequence of states that generates $x_1..x_i$ and $y_1..y_j$ and ends in state $u$
  -

$V\[i,j,u\] = \\max\_w \\left\\{ \\begin{array}{l}
V\[i-1,j-1,w\] \\cdot a\_{w,u} \\cdot e\_{u,x\_i,y\_j} \\\\\\\\
V\[i-1,j,w\] \\cdot a\_{w,u} \\cdot e\_{u,x\_i,-} \\\\ V\[i,j-1,w\] \\cdot a\_{w,u}\\cdot e\_{u,-,y\_j} \\\\ \\end{array}\\right.$

$V[i,j,u] = \max_w \left\{ \begin{array}{l}
V[i-1,j-1,w] \cdot a_{w,u} \cdot e_{u,x_i,y_j} \\\\
V[i-1,j,w] \cdot a_{w,u} \cdot e_{u,x_i,-} \\\\
V[i,j-1,w] \cdot a_{w,u}\cdot e_{u,-,y_j} \\
\end{array}\right.$

  - Time complexity $O(mnk^2)$ where $m$ and $n$ are the lengths of the input sequences, $k$ is the number of states

How would we construct a pair HMM for gene finding in two sequences simultaneously?

  - Assume the same number of exons
  - In exons, gaps only as whole codons (both simplified)
  - Elsewhere, any gaps allowed

