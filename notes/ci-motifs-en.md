---
title: "Tutorial for Computer Scientists: Motif Finding"
---

* TOC
{:toc}


## Finding Sequence Motifs Defined by a Probability Matrix

  - We are given $n$ sequences $S=(S_1,\dots, S_n)$, each of length *m*, motif length *L*, null hypothesis *q* (nucleotide frequencies in the genome)
  - We are looking for a motif in the form of a probability profile of length *L* and its occurrence in each sequence
  - Let $W[a,i]$ be the probability that at position *i* of the motif there is base *a*, *W* is the whole matrix
  - Let $o_i$ be the position of occurrence in sequence $S_i$, $O=(o_1, \dots, o_n)$ are all occurrences
  - $\Pr(S\|W,O)$ is a simple product, where for positions in the motif windows we use probabilities from *W*, for positions outside the windows we use *q*
      - $\Pr(S_i\|W,o_i) =  \prod_{j=1}^{L} W[S_i[j+o_i-1],j] \prod_{j=1}^{o_i-1} q[S_i[j]] \prod_{j=o_i+L}^m q[S_i[j]]$
      - $\Pr(S\|W,O) = \prod_{i=1}^n \Pr(S_i\|W,o_i)$
  - We are looking for *W* and *O* that maximize this likelihood $\Pr(S\|W,O)$
      - There is no known efficient algorithm that always finds the maximum
      - We could try all possibilities for *O*. For a given *O* the best *W* is the frequencies from the data.
      - Conversely, if we know *W*, we can find the best *O*:
          - in each sequence *i* we try all positions $o_i$ and choose the one with the highest value $\Pr(S_i\|W,o_i)$

### EM Algorithm

  - Iteratively improves *W*, taking all *O* weighted by their probability with respect to *W* from the previous round
  - As seen in the lecture, here is a slightly rewritten version:

  - Initialization: assign each position *j* in sequence $S_i$ some score $p_{i,j}$
  - Iterate:
      - Compute *W* from all possible occurrences in $S_1,\dots,S_k$ weighted by $p_{i,j}$
      - Recompute all scores $p_{i,j}$ so that they correspond to the ratios of probabilities of occurrence of *W* at position *j* in $S_i$. That is, $p_{i,j}$ is proportional to $\Pr(S_i\|W,o_i=j)$, and normalized so that the sum of $p_{i,j}$ in each row $i$ is 1.

### Gibbs Sampling

  - Initialization: Take random positions of occurrences *O*
  - Iterate:
      - Compute *W* from occurrences *O*
      - Randomly select one sequence $S_i$
      - For each possible position *j* in $S_i$ compute the score $p_{i,j}$ (as in EM) of occurrence of *W* at this position
      - Choose $o_i$ randomly weighted by $p_{i,j}$


  - In this way, we get a sequence of samples $O^{(0)}, O^{(1)}, \dots$.
  - Consecutive samples are similar (differ only in one component $o_i$), so they are not independent.
  - For each sample $O^{(t)}$ we find the best $W^{(t)}$ and compute the likelihood $\Pr(S\|W^{(t)},O^{(t)})$. Finally, we select *O* and *W* where the likelihood was the highest.
  - This algorithm (with minor modifications) was used in the paper Lawrence, Charles E., et al. (1993) "Detecting subtle sequence signals: a Gibbs sampling strategy for multiple alignment." Science.
      - In the paper, in each iteration, the matrix *W* is computed from all sequences except $S_i$.
      - Occasionally, they make a step where they randomly try to shift all occurrences one position left or right.
      - This algorithm is not strictly mathematically correct Gibbs sampling (it does not even have a properly defined distribution from which it samples). For information, at the bottom of the page we provide the Gibbs sampling algorithm for motif finding from another paper.

## Sampling from a Probabilistic Model in General

  - Suppose we have a probabilistic model where $D$ are some observed data and $X$ are unknown random variables (for us, $D$ are sequences $S$ and $X$ are occurrences $O$, possibly also matrix $W$)
  - We can search for $X$ for which the likelihood $\Pr(D\|X)$ is highest
  - Or we can randomly sample different $X$ from $\Pr(X\|D)$

Use of samples

  - Among the obtained samples, we choose the one for which the likelihood $\Pr(D\|X)$ is highest (another heuristic approach to maximizing likelihood)
  - But samples also give us information about the uncertainty in the estimate of $X$
      - We can estimate means and deviations of various quantities
      - For example, in motif finding, we can track how often each position is an occurrence of the motif

  - Generating independent samples from $\Pr(X\|D)$ can be difficult
  - The Markov chain Monte Carlo (MCMC) method generates a sequence of dependent samples $X^{(0)}, X^{(1)},\dots$, which converges in the limit to the target distribution $\Pr(X\|D)$
  - Gibbs sampling is a special case of MCMC

### Markov Chains

  - See [earlier exercises](./ci-chains.html) 

  - **Markov chain** is a sequence of random variables $X^{(0)}, X^{(1)}, \dots,$ such that
    $\Pr(X^{(t)}|X^{(0)},\dots,X^{(t-1)}) = \Pr(X^{(t)}|X^{(t-1)})$,
    i.e. the value at time $t$ depends only on the value at time $t-1$ and not on earlier values.
  - We are interested in **homogeneous** Markov chains, in which $\Pr(X^{(t)}|X^{(t-1)})$ does not depend on $t$.
  - We are also only interested in chains where the random variables $X_t$ take values from a finite set (possible values of $X^{(t)}$ are called **states**)
      - For example, states A,C,G,T
      - In Gibbs sampling for motifs, the state is the configuration of variables O (i.e. we have (m-L+1)^n states)
          - The sample at step t depends on the sample at step t-1 (and differs only in the value of one $o_i$)

**Matrix**

  - The probabilities of transitions between states in one step can be expressed by a probability matrix P, whose element $p_{x,y}$ denotes the probability of transition from state x to state y
    $p_{X,Y}=\Pr(X_t=y|X_{t-1}=x)$
      - The sum of each row is 1, numbers are non-negative
  - We denote $p_{x,y}^t$ as $\Pr(X^{(t)}=y|X^{(0)}=x)$,
    these values are obtained by raising the matrix *P* to the power *t*

**Stationary Distribution**

  - A distribution $\pi$ on the set of states is called **stationary** for a Markov chain $P$ if for each j
    $\sum_{i}\pi(i)p_{i,j} = \pi(j)\,$ (or in matrix notation $\pi P = \pi$)
  - If matrix P satisfies certain conditions (is ergodic), there exists exactly one stationary distribution $\pi$. Moreover, for each x and y
    $\lim_{t\to\infty} p_{x,y}^{t} = \pi(y)\,$

**Examples of Markov Chains in Bioinformatics**

  - In HMMs, the states form a Markov chain
  - Other variants: infinite state spaces (more complex theory), continuous time (seen in evolutionary models), higher-order chains where we determine $\Pr(X_t|X_{t-r},\dots,X_{t-1})$, etc.
  - Use in bioinformatics: characterization of random sequences (null hypothesis), for DNA orders up to 5 are used, better than independent variables

**Ergodic Markov Chains**

  - We say that a matrix is **ergodic** if $P^t$ for some t\>0 has all entries nonzero
  - Examples of non-ergodic matrices


    1 0          0.5 0.5          0 1             0.5 0.5
    0 1          0   1            1 0             1   0
    disconnected weakly connected periodic      ergodic

  - In HMMs, the states form a Markov chain; gene finding has an ergodic state space, profile HMMs do not

### Markov chain Monte Carlo MCMC

  - We want to generate random samples from some target distribution $\pi$, but this distribution is too complex.
  - We construct an ergodic Markov chain whose stationary distribution is $\pi$, so that we can efficiently sample $X^{(t)}$ if we know $X^{(t-1)}$.
  - If we start from any point $X^{(0)}$, after some time t the distribution of $X^{(t)}$ is approximately $\pi$
  - But consecutive samples are not independent\!
  - However, we can estimate expected values of various quantities
    $\frac{1}{t} \sum_{i=1}^t f(X^{(t)})$ converges to $E_\pi [f(X)]$

### Gibbs Sampling

  - The target distribution $\pi(X)$ is over vectors of length *n* $X=(x_1,...x_n)$
  - In each step, we sample one component $x_i$ from the conditional probability $\Pr(x_i|x_1,\dots,x_{i-1},x_{i+1},\dots x_n)$
  - The other values remain the same as in the previous step
  - The value $i$ is chosen randomly or cycled periodically $i=1,2,\dots,n$

### Proof of Correctness of Gibbs Sampling

  - Gibbs sampling is not always ergodic if some combinations of values have zero probability. Therefore when designing Gibbs sampling, we must ensure that the chain is ergodic.
  - For general Gibbs sampling, we can shown that if it is ergodic, then our chosen $\pi$ is its stationary distribution
  - Definition: We say that matrices P and distribution $\pi$ satisfy **detailed balance** if for every pair of states (two value vectors) *x* and *y* we have $\pi(x)p_{x,y} = \pi(y)p_{y,x}$
  - Lemma: if for some chain P and some distribution $\pi$ detailed balance holds, then $\pi$ is a stationary distribution for P
      - Proof:
        $\sum_x \pi(x)p_{x,y} = \sum_x \pi(y)p_{y,x} = \pi(y)\sum_x p_{y,x} = \pi(y)$
  - Lemma: for the Gibbs sampling chain, detailed balance holds with respect to the target distribution $\pi$
      - Proof: consider two consecutive value vectors x and y that differ in the i-th coordinate. Let $x_{-i}$ be the values of all other variables except $x_i$
      - $\pi(x)p_{x,y} = \pi(x)\Pr(y_i\|x_{-i}) = \Pr(x_{-i})\Pr(x_i\|x_{-i}) \Pr(y_i\|x_{-i}) = \pi(y)\Pr(x_i\|x_{-i}) = \pi(y)\Pr(x_i\|y_{-i}) = \pi(y)p_{y,x}$

## More Proper Gibbs Sampling for Motifs

Given for interest - according to the paper Siddharthan, R., Siggia, E.D. and Van Nimwegen, E., 2005. PhyloGibbs: a Gibbs sampling motif finder that incorporates phylogeny. PLoS computational biology, 1(7), p.e67.

**Probabilistic Model**

  - We extend the model so that O and W are also random variables, so we have the distribution Pr(S,W,O)
      - Then we want to sample from Pr(O|S) (marginalizing over all values of W)
  - A random probability matrix W is generated (e.g. from a uniform distribution over all matrices)
  - In each sequence i, a window $o_i$ of length L is chosen (uniformly from m-L+1 possibilities)
  - In the window, the sequence is generated according to profile W, and outside the window according to the null hypothesis (as before)

**Gibbs Sampling**

  - Given S, we sample O ($O^{(0)}, O^{(1)}, \dots$) (if needed, from $O^{(t)}$ we can construct the matrix $W^{(t)}$)
      - start with random windows $O^{(0)}$
      - in step t+1, choose one sequence i and for all positions $o'_i$ compute $\Pr(o'_i|O^{(t)}_{-i},S)$ (where $O_{-i}=o_1\dots o_{i-1}o_{i+1}\dots o_n$, i.e. all occurrence positions except the i-th).
      - randomly choose one $o'_i$ proportional to these probabilities
      - $O^{(t+1)}$ is obtained from $O^{(t)}$ by replacing the position in sequence i with the newly chosen one
      - repeat many times
  - Converges to the target distribution $\Pr(O|S)$, but samples are not independent
  - Other possible steps in sampling: shift all windows by a constant left or right
  - Other possible model/algorithm extensions: add a distribution over *L* and randomly increase/decrease *L*, allow skipping the motif in some sequences, search for multiple motifs at once, ...

**How to compute $\Pr(o_i\|O_{-i},S)$?**

  - We are not interested in normalization constants, we can easily normalize by summing over all $o'_i$
  - $\Pr(o_i\|O_{-i},S) = \Pr(O\|S) / \Pr(O_{-i}\|S)$, but the denominator is a constant
  - $\Pr(O\|S) = \Pr(S\|O)\Pr(O)/\Pr(S)$, where
    $\Pr(S)=\sum_{O'} \Pr(S\|O')\Pr(O')$
  - The denominator does not interest us (normalization constant)
  - $\Pr(O)$ is also a constant (uniform distribution of window positions)
  - Thus, $\Pr(o_i\|O_{-i},S)$ is proportional to $\Pr(S|O)$
  - We can easily compute $\Pr(S\|W,O)$, we need to "eliminate" W, a formula can be computed...
  - We try all possible values $o'_i$, compute the probability $\Pr(S\|O)$, sample proportional to that

Further details of computing $\Pr(S\|O)$:

  - Let $S_o$ be only the sequences in the windows and $S_{-o}$ outside the windows. We have $\Pr(S\|O) = \Pr(S_o\|O)\Pr(S_{-o}\|O)$
  - $\Pr(S_{-o}\|O)$ can be easily computed (does not depend on W)
  - $\Pr(S_o\|O) = \int \Pr(S_o\|O,W)\Pr(W)dW$ where the integral is over values where $w_{a,i}\ge 0$ and $\sum_a w_{a,i} = 1\,$
  - $\Pr(W)$ is a constant (uniform distribution; not a probability but a density),
    $\Pr(S_o\|O,W) = \prod_{i=1}^L \prod_a (w_{a,i})^{n_{a,i}}$, where $n_{a,i}$ is the number of occurrences of base $a$ at position $i$ in windows $o_1\dots o_n$
  - $\Pr(S_o\|O) = \prod_{i=1}^L 3!/(n+3)! \prod_a n_{a,i}!$ (without proof)
