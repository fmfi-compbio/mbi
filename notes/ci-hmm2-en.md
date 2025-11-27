---
title: "Tutorial for Computer Scientists: More HMMs"
---

* TOC
{:toc}

## Introduction

In this course we have seen HMMs for gene finding and also profile HMMs for representing protein families. Recall that the states and possible transitions between them are usually set up manually based on biological knowledge. Probabilities are then trained from data. Today we will try to design states and transitions for two applications of HMMs.

## Designing the State Space of the Model

#### A transmembrane protein homology prediction HMM 
  - Sonnhammer, E.L., Von Heijne, G. and Krogh, A., 1998, June. [A hidden Markov model for predicting transmembrane helices in protein sequences](https://cdn.aaai.org/ISMB/1998/ISMB98-021.pdf). In ISMB (Vol. 6, pp. 175-182).
  - Classes we want to predict for each amino acid: inside the cell, outside the cell, membrane
  - First attempt: three states, one for each class. Which transitions should we allow?
  - Problem 1: If we enter the mebrane state from inside, we must exit to outside, and vice versa. How can we handle this with HMM states?
  - Problem 2: A single state cannot model length distribution of membrane segments well. A typical membrane segment is 15-25 amino acids long. How can we enforce this length range in the model?

#### Jumping profile HMMs for recombination detection in HIV sequences
  <!-- - To be covered after the protein lecture which introduces profile HMMs -->
  - Schultz, A.K., Zhang, M., Bulla, I., Leitner, T., Korber, B. and Stanke, M., 2006. [A jumping profile Hidden Markov Model and applications to recombination sites in HIV and HCV genomes](https://link.springer.com/article/10.1186/1471-2105-7-265). BMC Bioinformatics, 7(1).
  - We have a large multiple sequence alignment of HIV sequences from different subtypes
  - From this, we build profile HMMs for each subtype
  - Given a new HIV sequence, we want to determine its subtype
  - We could align it to each profile HMM (e.g. by the Viterbi algorithm) and choose the best scoring alignment
  - How could we combine all profile HMMs into a single HMM to run Viterbi just once? Would it be faster than aligning to each profile HMM separately? Could we incorporate different frequencies of subtypes in the population into the model?
  - Some HIV sequences are recombinants of different subtypes. This means that for example start of the sequence comes from subtype A, then a part from subtype B, then again from subtype A, etc.
  - How could we model this in the combined HMM so that the Viterbi algorithm can find recombination breakpoints?


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


$V\[i,j,u\] = \\max\_w \\left\\{ \\begin{array}{l}
V\[i-1,j-1,w\] \\cdot a\_{w,u} \\cdot e\_{u,x\_i,y\_j} \\\\\\\\
V\[i-1,j,w\] \\cdot a\_{w,u} \\cdot e\_{u,x\_i,-} \\\\ V\[i,j-1,w\] \\cdot a\_{w,u}\\cdot e\_{u,-,y\_j} \\\\ \\end{array}\\right.$

  - Time complexity $O(mnk^2)$ where $m$ and $n$ are the lengths of the input sequences, $k$ is the number of states

How would we construct a pair HMM for gene finding in two sequences simultaneously?

  - Assume the same number of exons
  - In exons, gaps only as whole codons (both simplified)
  - Elsewhere, any gaps allowed
