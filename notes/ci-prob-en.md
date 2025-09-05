---
title: "Tutorial for Computer Scientists: Introduction to Probability"
---

* TOC
{:toc}

## What is Probability?

  - A thought experiment involving randomness, such as rolling an ideal die or coin.
  - The result of the experiment is some value (for example a number, several numbers or a string).
  - We will call this unknown value a **random variable (náhodná premenná)**.
  - We are interested in the **probability (pravdepodobnosť)** with which the random variable takes on each possible value.
  - That is, if we repeat the experiment many times, how often do we expect to see a particular result.


## Example 1: Rolling a Die (hod kockou)

We roll an ideal die, variable X will be the value we get.

  - Possible values: 1,2,..,6, each equally probable
  - We denote this as Pr(X=2)=1/6

## Example 2: Sum of Two Dice

We roll a dice twice, random variable X is the sum of the two values we get.

  - Possible value of X: 2,3,...,12
  - Each pair of values of the two rolls (1,1), (1,2), ..., (6,6) equally probable. This means the probability of each is 1/36
  - Sum 5 can be obtained in four ways as 1+4, 2+3, 3+2, 4+1. This means Pr(X=5) = 4/36
  - Sum 11 can be obtained as 5+6 or 6+5, yielding Pr(X=11) = 2/36
  - **Probability distribution (rozdelenie pravdepodobnosti)**
  (a table giving the probability for each possible value of X):

```
value k:     2     3     4     5     6     7     8     9     10    11    12
Pr(X=k):    1/36  2/36  3/36  4/36  5/36  6/36  5/36  4/36  3/36  2/36  1/36
```

  - Verify that the sum of probabilities is 1.

## Expected value E(X) (stredná hodnota)

  - average of possible values ​​weighted by their probabilities $E(X)=\sum_k \Pr(X=k)\cdot k$
  - in our example with two dice
    $E(X) = 2\cdot \frac{1}{36} + 3\cdot \frac{2}{36}+ 4\cdot \frac{3}{36}+ 5\cdot \frac{4}{36}+ 6\cdot \frac{5}{36}+ 7\cdot \frac{6}{36}+ 8\cdot \frac{5}{36}+ 9\cdot \frac{4}{36}+ 10\cdot \frac{3}{36}+ 11\cdot \frac{2}{36}+ 12\cdot \frac{1}{36}=7$
  - If we repeated the experiment many times and averaged the values of X ​​that we got, we would get a number close to E(X)
  - The expected value is linear, which often leads to a simpler computation:
      - $X=X_1+X_2$, where $X_1$ is the value of the first roll and $X_2$ of the second roll
      - $E(X_1) = 1\cdot \frac{1}{6} + ... + 6\cdot \frac{1}{6}  = 3.5$;
        similarly $E(X_2) = 3.5$
      - Linearity means that $E(X_1+X_2)=E(X_1) + E(X_2)$, so in our case $E(X) = 3.5 + 3.5 = 7$
      - Beware, such equalities do not necessarily hold for the product of two variables and other functions. So $E(X_1 \cdot X_2)$ is not always necessarily equal to $E(X_1) \cdot E(X_2)$.

## Binomial Distribution (binomické rozdelenie)

- Image a coin toss which yields head (H) with probability p and tail
  (T) with probability 1-p.
- We toss the coin N times; let X be the number of heads that occur.
- Example: let us have a coin with heads probability p = 1/4 and toss it N=3 times.

```
    HHH 1/64
    HHT 3/64
    HTH 3/64
    HTT 9/64
    THH 3/64
    THT 9/64
    TTH 9/64
    TTT 27/64
```

- $\Pr(X=3) = 1/64$, $\Pr(X=2)=9/64$, $\Pr(X=1)=27/64$, $\Pr(X=0)=27/64$.
- This probability distribution is called binomial.
- $Pr(X = k) = {N \choose k} p^k (1-p)^{N-k}$, where
  ${N \choose k} = \frac{N!}{k!(N-k)!}$ and $n! = 1\cdot 2 \cdots  n$
- For example, for the case with three coin tosses $\Pr(X=2) = 3!/(2!\cdot 1!) \cdot (1/4)^2 \cdot (3/4)^1 = 9/64$
- It is hard to compute for large N, so for large N and small p, we sometimes use an approximation
  with the Poisson distribution with parameter $\lambda = Np$, which has
  $\Pr(X_j = k)=e^{-\lambda}\lambda^k / k!$


## Calculating Genome Coverage (Počítanie pokrytia genómov)

  - Our problem: calculating necessary coverage for a genome
      - G = genome length, e.g., 1,000,000 (assume it is circular)
      - N = number of reads, e.g., 10,000
      - L = read length, e.g., 1000
      - Total read length NL, coverage NL/G, in our case 10x
      - On average, each base is covered 10 times.
      - But some are covered more times, others less.
      - We are interested in questions like: how many bases do we expect to be covered less than 3 times?
      - Important for planning experiments (what coverage do I need to achieve a certain quality)

  - We assume that each read starts at a random position unifrmly chosen among all G possibilities.
  - So if variable $Y_i$ is the start of the i-th read, its distribution is uniform
      - $\Pr(Y_i=1) = \Pr(Y_i=2) = ... = Pr(Y_i=G) = 1/G$

  - What is the probability that a particular i-th read covers a particular position j?
      - $\Pr(Y_i\ge j-L+1 \wedge Y_i \le j) = \Pr(Y_i=j-L+1)+...+\Pr(Y_i=j) = L/G$. Denote this value as p; in our example p=0.001 (0.1%)


  - Consider variable $X_j$, which gives the number of reads covering position j
      - possible values 0..N
      - the i-th read covers position j with probability p=L/G
      - this is the same as tossing a coin N times, where heads comes up with probability p and tails with 1-p, and $X_j$ is the number of heads
      - so $X_j$ comes from the binomial distribution
      - we can compute the probability distribution
        and also values such as  $\Pr(X_i<3) = \Pr(X_i=0)+Pr(X_i=1)+Pr(X_i=2) = 0.000045+0.00045+0.0023=0.0028$
  - The expected number of bases in the whole genome with coverage k is $G\cdot \Pr(X_i=k)$
      - On average, we expect 45 bases uncovered, 2800 covered less than 3 times, etc.
      - We can make such estimates for different numbers of reads and thus we can plan how many reads are needed.


We also want to estimate the **number of contigs** (according to the article E.S. Lander and M.S. Waterman. ["Genomic mapping by fingerprinting random clones: a mathematical analysis."](http://www.cs.cmu.edu/~epxing/Class/10810/readings/lander_waterman.pdf) Genomics 2.3 (1988): 231-239)

  - If several bases are not covered by any reads, a contig ends and another starts.
  - We know how many bases are on average uncovered, but some may be next to each other.
  - A new contig also arises if neighboring reads overlap too little.
  - Assume that to join two reads we need an overlap of at least T=50.
  - Let p be the probability that a given read i is the last in a contig
  - For this to happen, no read $j\ne i$ must start in the first L-T bases of contig i
  - Each read starts there with probability q=(L-T)/G
  - If X is the number of reads starting in this region, then $p = \Pr(X=0) = (1-q)^{N-1}$ according to the binomial distribution
  - On average, $E(X) = (N-1)(L-T)/G$ which is about $N(L-T)/G$
  - A simpler formula for p is obtained if we approximate the binomial distribution of X by a Poisson with parameter $\lambda=N(L-T)/G$ (i.e., so that they have the same mean)
  - In the Poisson distribution $p = \Pr(X=0) = e^{-\lambda} = e^{-N(L-T)/G}$
  - Approximation accuracy: for the parameters N,L,G,T above, we get from the binomial p=7.459e-5, from Poisson 7.485e-5
  - For N reads, the average number of contigs is $N\cdot p = N\cdot e^{-N(L-T)/G}$
  - NL/G is coverage, N(L-T)/G is coverage if we shorten each read by the overlap length
  - For T=50 we get an average number of contig ends 0.75 (if we cover the whole circle, we have zero ends, so the value is less than 1). If we reduce N to 5000 (5x coverage) we get 43 contigs.


  - It may seem strange that with an average number of uncovered bases 45, we have an average number of contig ends less than one. The situation is that in repeated experiments we often get one continuous contig, but if there is at least one contig end, there is usually a rather large gap. Here are, for example, 50 repetitions of the experiment with T=0, the average number of ends is 0.55, the average number of uncovered bases is 49.


```
uncovered: 0 ends: 0     uncovered: 0 ends: 0     uncovered: 0 ends: 0      
uncovered: 274 ends: 2   uncovered: 282 ends: 1   uncovered: 0 ends: 0      
uncovered: 0 ends: 0     uncovered: 0 ends: 0     uncovered: 8 ends: 1      
uncovered: 0 ends: 0     uncovered: 12 ends: 1    uncovered: 0 ends: 0      
uncovered: 122 ends: 1   uncovered: 135 ends: 1   uncovered: 111 ends: 1    
uncovered: 13 ends: 1    uncovered: 1 ends: 1     uncovered: 56 ends: 1     
uncovered: 265 ends: 1   uncovered: 0 ends: 0     uncovered: 10 ends: 1     
uncovered: 0 ends: 0     uncovered: 0 ends: 0     uncovered: 130 ends: 1    
uncovered: 217 ends: 1   uncovered: 3 ends: 1     uncovered: 0 ends: 0      
uncovered: 0 ends: 0     uncovered: 0 ends: 0     uncovered: 86 ends: 1     
uncovered: 139 ends: 2   uncovered: 0 ends: 0     uncovered: 0 ends: 0      
uncovered: 76 ends: 1    uncovered: 221 ends: 1   uncovered: 26 ends: 1     
uncovered: 0 ends: 0     uncovered: 1 ends: 1     uncovered: 0 ends: 0      
uncovered: 0 ends: 0     uncovered: 0 ends: 0     uncovered: 0 ends: 0      
uncovered: 0 ends: 0     uncovered: 0 ends: 0     uncovered: 12 ends: 1     
uncovered: 103 ends: 2   uncovered: 0 ends: 0     uncovered: 71 ends: 1     
uncovered: 69 ends: 1    uncovered: 0 ends: 0    
```

  - This simple model does not cover all factors:
      - Reads do not have the same length.
      - Problems in assembly due to errors, repeats, etc.
      - Reads are not distributed uniformly (they are influenced e.g. by the content of G and C bases).
      - Effect of chromosome ends in linear chromosomes.
  - However, the model is useful as a rough estimate.
  - For more accurate results, we can try to make more complex models.


## Summary

  - Probabilistic model is a thought experiment involving randomness, e.g., rolling an ideal die.
  - The result is a value, which we will call a random variable.
  - Probability distribution is a table that for each possible value of the random variable gives its probability. The sum of the values in the table is 1.
  - Notation like Pr(X=7)=0.1

  - Example: we have a genome of length G=1 million, we randomly place N=10,000 reads of length L=1000
  - The random variable $X_i$ is the number of reads covering a given position i
  - Similar to tossing a coin N times, where the chance of heads is about 0.1% and tails 99.9%, and we ask how many times heads comes up (0.1% comes from rounding L/(G-L+1))
  - The probability distribution in this case is called binomial and there is a formula for calculating it.
  - Such a model can help us to determine how many reads we need to sequence so that, for example, at least 95% of positions are covered by at least 4 reads.
