---
title: "Tutorial for Computer Scientists: Heuristic Sequence Alignment"
---

* TOC
{:toc}

## Minimizers: saving memory and time in heuristic sequence alignment


### Heuristic sequence alignment 

  - A $k$-mer is defined as $k$ consecutive characters in a string (in our case DNA nucleotides)
  - In lecture: use of $k$-mers as seeds for finding potential matches (BLAST)
  - Note that in the lecture the length of seed was denoted $w$, here $k$
  - When comparing two sequences (or sets of sequences), we store all $k$-mers of one sequence in a dictionary (such as a hash table), then go through all $k$-mers of the other sequence and look them up in the dictionary

  - Example $k=5$,
      - DB on the left, $k$-mers and their positions to be stored in the dictionary,
      - dictionary here shown sorted, but in practice a hash table is used,
      - query on the right, $k$-mers searched in the dictionary, arrows indicate found $k$-mers (alignment seeds)


``` 
Alignment we look for (mismatches shown in small letters):
AGTGGCTGCCAGGCTGG    
cGaGGCTGCCaGGtTGG  

DB:                   Dict:       Query:
AGTGGCTGCCAGGCTGG                 cGaGGCTGCCtGGtTGG  
AGTGG,1               AGGCT,11    CGAGG,1              
 GTGGC,2              AGTGG,1      GAGGC,2              
  TGGCT,3             CAGGC,10      AGGCT,3 -> 11         
   GGCTG,4            CCAGG,9        GGCTG,4 -> 4,12           
    GCTGC,5           CTGCC,6         GCTGC,5 -> 5     
     CTGCC,6          GCCAG,8          CTGCC,6 -> 6        
      TGCCA,7         GCTGC,5           TGCCT,7
       GCCAG,8        GCTGG,13           GCCTG,8       
        CCAGG,9       GGCTG,4,12          CCTGG,9     
         CAGGC,10     GTGGC,2              CTGGT,10    
          AGGCT,11    TGCCA,7               TGGTT,11    
           GGCTG,12   TGGCT,3                GGTTG,12  
            GCTGG,13                          GTTGG,13  
```

### K-mers with a step
  - Trick to reduce required memory (used in the BLAT program): store only every $s$-th $k$-mer from the first sequence, then search all $k$-mers from the second
  - This slightly reduces sensitivity
  - We are guaranteed to find a seed if there are at least $k+s-1$ consecutive matches
  - Could we also search only every $s$-th $k$-mer in the query?
      - What if db and query were the same, but the query was missing the first letter?

Example $k=5$, $s=3$, $k$-mers with numbers are stored in the dictionary; all query $k$-mers are searched
```
Alignment we look for:
AGTGGCTGCCAGGCTGG    
cGaGGCTGCCaGGtTGG  

DB:                  Dict:      Query:
AGTGGCTGCCAGGCTGG               cGaGGCTGCCtGGtTGG  
AGTGG                CCAGG,9    CGAGG,1              
 GTGGC               CTGCC,6     GAGGC,2              
  TGGCT,3            GGCTG,12     AGGCT,3               
   GGCTG             TGGCT,3       GGCTG,4 -> 12           
    GCTGC                           GCTGC,5          
     CTGCC,6                         CTGCC,6 -> 6        
      TGCCA                           TGCCT,7
       GCCAG                           GCCTG,8       
        CCAGG,9                         CCTGG,9     
         CAGGC                           CTGGT,10    
          AGGCT                           TGGTT,11    
           GGCTG,12                        GGTTG,12  
            GCTGG                           GTTGG,13  
```

### Minimizers

  - To reduce dictionary lookups, we can use **minimizers**
  - Consider all groups of $s$ consecutive $k$-mers (sliding window), in each group find the lexicographically smallest $k$-mer (minimizer) and store it in the dictionary
  - When shifting the window by one to the right, often the smallest $k$-mer remains the same and does not need to be stored again, saving memory (and time)
  - We do not search for all $k$-mers of the query, but also only minimizers, which can save time
  - We are again guaranteed to find a seed if there are at least $k+s-1$ consecutive matches
  - Example $k=5$, $s=4$, $k$-mers with numbers are stored/searched in the dictionary

``` 
AGTGGCTGCCAGGCTGG                cGaGGCTGCCtGGtTGG  
AGTGG,1             AGGCT,11     CGAGG              
 GTGGC              AGTGG,1       GAGGC             
  TGGCT             CAGGC,10       AGGCT,3 -> 11          
   GGCTG            CCAGG,9         GGCTG           
    GCTGC,5         CTGCC,6          GCTGC          
     CTGCC,6        GCTGC,5           CTGCC,6 -> 6        
      TGCCA                            TGCCT        
       GCCAG                            GCCTG       
        CCAGG,9                          CCTGG,9     
         CAGGC,10                         CTGGT,10    
          AGGCT,11                         TGGTT    
           GGCTG                            GGTTG   
            GCTGG                            GTTGG  
```

  - Especially advantageous if the first and second set of sequences are the same, e.g., when searching for overlaps in reads during genome assembly. Each read has a set of minimizers, which are used as keys in the dictionary, values are lists of reads. Pairs of reads sharing a minimizer end up in the same list  (binning) and will be considered for overlap computation
  - In practice, we do not use the lexicographically smallest $k$-mer as a minimizer, but each $k$-mer is hashed by some function and the one with the minimal hash value is taken
  - The reason is to avoid minimizers often being sequences like AAAAA, which are overrepresented in biological sequences and often not functionally interesting
  - The average density of minimizers in a sequence with random hashing is about $2/(s+1)$ (in BLAT it was lower, $1/s$)
  - Minimizers are used by minimap2, a very popular tool for aligning reads to each other and to genomes
      - for aligning nanopore reads to a genome it uses $k=15$, $s=10$,
        overlaps in nanopore reads $k=15$, $s=5$, genome comparison with identity over 80% $k=19$, $s=10$

### Summary

| Program | # $k$-mers in dict &nbsp;&nbsp; | # $k$-mers searched | a seed guaranteed in |
| --------|--------------------|---------------------|-----------------------|
| BLAST | $n$ | $m$ | $k$ adjacent matches |
| BLAT | $n/s$ | $m$ | $k+s-1$ adjacent matches |
| minimizers &nbsp;&nbsp; | $2n/(s+1)$ | roughly $2m/(s+1)$ &nbsp;&nbsp;| $k+s-1$ adjacent matches |

  - $n$, $m$ are the lenths of sequences
  - in $k$-mer counts we neglected terms like $-k+1$ 
  


Literature
  - Li, Heng. [Minimap and miniasm: fast mapping and de novo assembly for noisy long sequences.](https://academic.oup.com/bioinformatics/article-abstract/32/14/2103/1742895) Bioinformatics 32.14 (2016): 2103-2110.
  - Li, Heng. [Minimap2: pairwise alignment for nucleotide sequences.](https://academic.oup.com/bioinformatics/article-abstract/34/18/3094/4994778) Bioinformatics 34.18 (2018): 3094-3100. 
  - Roberts M, Hayes W, Hunt BR, Mount SM, Yorke JA. [Reducing storage requirements for biological sequence comparison.](https://academic.oup.com/bioinformatics/article/20/18/3363/202143) Bioinformatics. 2004 Dec 12;20(18):3363-9.
  - Marçais, G., Pellow, D., Bork, D., Orenstein, Y., Shamir, R., & Kingsford, C. (2017). [Improving the performance of minimizers and winnowing schemes.](https://doi.org/10.1093/bioinformatics/btx235) Bioinformatics, 33(14), i110-i117.
    

## MinHash

### A short aside: Text similarity in web analysis

Let us have a set of web pages (a web page is a sequence of words).
We want to find pairs of similar ones among them. How can we define the similarity of two texts?

One way to do this is to look at the number of words shared by each pair of pages. We expect that the more words they have in common, the more similar they are. This measure of similarity is formalized by the mathematical concept of the Jaccard index.

Let $U$ be the universe of words and let $A, B \subseteq U$ be its subsets (i.e., the sets of words of two texts). Then the Jaccard index is $J(A, B) = \frac{\|A \cap B\|}{\|A \cup B\|}$

This measure takes the value 0 only if the sets are disjoint, and the value 1 only if the sets are identical. Otherwise, its value lies in the open interval $(0, 1)$, and the more words they have in common, the higher its value.

Then the question "Which pairs of texts are similar?" can be reformulated, for example, as "Which pairs of texts have the Jaccard index higher than some threshold $\alpha$?".

How quickly could we compute the Jaccard index for two sets of words, each with $n$ elements?

The exact computation of the Jaccard index is not always fast enough for the purposes of a particular application, so a logical solution is to try to compute its value only approximately.

### The first idea: approximation by sampling

  - If we could sample elements $u_1, u_2, \dots, u_s$ from $A \cup B$ (uniformly, independently), and for each element $u_i$ we could quickly determine whether it belongs to the intersection, we could estimate $J(A, B)$ using the random variable $X = \frac{1}{s}\sum_{i=1}^s X_i$ where $X_i=1$ if $u_i$ is in the intersection and $X_i=0$ otherwise
  - This is similar to determining a politician's popularity by a public opinion poll, $u_1, u_2, \dots, u_s$ are "respondents"
  - For each $X_i$ we have $E(X_i) = 0 \cdot \Pr[X_i = 0] + 1 \cdot \Pr[X_i = 1] = \Pr[X_i = 1] = J(A, B)$.
  - By linearity of expectation $E(X) = E(\frac{1}{s}\sum_{i=1}^s X_i) = \frac{1}{s}\sum_{i=1}^s E[X_i] = J(A, B)$.
  - It follows that the random variable $X$ is an unbiased estimator of the Jaccard coefficient.
  - In statistics, the basic measure of the quality of an unbiased estimator is its variance, where $Var(X) \le \frac{1}{4s}$ (see derivation below)
  - As the sample size *s* increases, the variance decreases. A similar situation as in opinion polls, where a larger sample of respondents gives more reliable results.

**Problems:**

  - It is not easy to sample uniformly from $A \cup B$
  - To determine whether $u_i$ is in the intersection, we need to have representations of $A$ and $B$ in memory, which can be a problem for a large collection of documents.
  - Idea: we want to get some other variables $X_i$ that will be independent, have $E(X_i)=J(A,B)$ and will be easier to compute

### Approximation of the Jaccard coefficient: MinHash

We will have random hash functions $h_1, h_2, \dots, h_s$.

For each hash function $h$, we assume that if we use it on some set $A = \{a_1, a_2, \ldots, a_n\}$, then $h(a_1), h(a_2), \ldots, h(a_n)$ will be a random permutation of the set $A$ (not really true for typical hash functions).


For a set $A = \{a_1, a_2, \ldots, a_n\}$ and a hash function $h$, $minHash_{h}(A)$ is defined as follows:


$minHash_h(A) := \min\{h(a_1), h(a_2), \ldots, h(a_n)\}$

Since $h$ is a random hash function, we can view the value $minHash_h(A)$ as a random variable representing a uniformly random selection of an element from the set $A$.

$\Pr(minHash_h(A) = minHash_h(B)) = J(A, B)$

Proof:
- Minimum of $A\cup B$ is in $A\cap B$ with probability equal to the Jaccard index
- If the minimum is outside of $A\cap B$, then the minHashes of $A$ and $B$ are different
- If it is inside $A\cap B$, then the minHashes of $A$ and $B$ are the same

Let us have $s$ independent hash functions $h_1, h_2, \dots, h_s$. For each $i=1,2,\dots,s$ we define the variable $X_i$ as follows:

$X_i := \begin{cases}
1 & \text{if } minHash_{h_i}(A) = minHash_{h_i}(B) \\\\
0 & \text{otherwise}
\end{cases}$
Then $E(X_i) = J(A, B)$ and the variables $X_i$ are independent.

We thus replace the random samples discussed above with such $X_i$ values.


### MinHash algorithm

Computing sketches of documents:
- Choose "random" hash functions $h_1,\dots, h_s$.
- For each text $A=\{a_1, \dots, a_n\}$:
  - For each function $h_i$ from $h_1,\dots, h_s$:
    - $S_{A,i} = \min\{h_i(a_1), h_i(a_2), \ldots, h_i(a_n)\}$
  - Vector $S_A = (S_{A,1}, S_{A,2}, \dots, S_{A,s})$ is called the sketch of $A$

Comparing sketches of documents:
- For each pair of documents $A$, $B$:
  - $x = \|\\{ i : S_{A,i} = S_{B,i}\\}\|$
  - $x/s$ is an estimate of $J(A,B)$

Running time and memory:
 - Let us have $N$ documents, each with $M$ words of length $w$
 - Computing all sketches takes time $O(NMsw)$
 - Sketches take up memory $O(Ns)$, we do not need to keep the documents in memory (when computing sketches, we can read documents word by word and keep only the current sketch values in memory)
 - Comparing one pair of documents takes $O(s)$, comparing all pairs takes $O(N^2s)$

For comparison, the trivial computation of the exact Jaccard measure using hashing would take $O(N^2Mw)$ and all documents would take up $O(NMw)$ memory. So if $N$ is large and $s$ is much smaller than $M$, we save both time and memory.

Alternative: instead of using $s$ different functions, we use only hash function and take not just the minimum, but the $s$ smallest elements. Then we estimate $J(A,B)$ using $\|S_A\\cap S_B\|/s$ where $S_A$ is the set of values in the sketch of set $A$. This saves time when computing the sketch, since we do not need to hash all elements $s$ times.

To avoid comparing all pairs of documents, we can use binning:
- For each $h_i$, create a dictionary mapping hash value to a list of documents for which it was minHash (bins)
- We compare only pairs of documents that end up in the bin (which means that their estimate of $J(A,B)$ will be nonzero)


Literature
  - Broder AZ. [On the resemblance and containment of documents.
    Compression and Complexity of SEQUENCES](https://www.cs.princeton.edu/courses/archive/spring13/cos598C/broder97resemblance.pdf) 1997 (pp. 21-29). IEEE.
    

### Finding similar sequences

As "words" we use all $k$-mers of a given sequence. Then, to find two similar sequences from a set of sequences, we can use minhash.

  - For example, a popular program called Mash for genome comparison uses $k=21$, $s=1000$
  - It stores $s$ smallest values from one hash function
  - The sketch is about 8kb per genome (a genome has millions or billions of nucleotides)

Literature
  - Ondov BD, Treangen TJ, Melsted P, Mallonee AB, Bergman NH, Koren S,
    Phillippy AM. [Mash: fast genome and metagenome distance estimation
    using MinHash.](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0997-x) Genome Biology. 2016 Dec;17(1):1-4.
    

### Calculation of variance (optional)

Consider the variable $X = \frac{1}{s}\sum_{i=1}^s X_i$ where $X_i$ are independent binary variables with $\Pr(X_i=1)=p$. We want to estimate the variance $Var(X)$ which tells us how good estimate we get by asking $s$ respondents/hash functions.

Variance $Var(X_i) = E(X_i^2) - (E(X_i))^2$. Let's compute both values step by step.

$E(X_i^2) = 0^2 \cdot \Pr[X_i = 0] + 1^2 \cdot \Pr[X_i = 1] = \Pr[X_i = 1] = p$

$(E(X_i))^2 = p^2$

So $Var(X_i) = p - p^2 = p(1-p)$. What is the maximum possible value of the variance?

This question is equivalent to "What is the maximum of the function $f(x) = x - x^2$ for $x\in[0, 1]$?". To find the extrema of smooth functions, we need to find the roots of its first derivative. The derivative of this function is $f'(x) = 1 - 2x$, its root is at $0.5$. The value of the function at this point is $1/4$. So $Var(X_i) \leq 1/4$.

$X =\frac{1}{s}\sum_{i=1}^s X_i$ where $X_i$ are independent variables and each has mean $E(X_i) = E(X) = p$ and the same variance $Var(X_i) = Var(X) = p(1 - p)\le 1/4$. The variable $X$ is their average.

Let us look at its variance:
$Var(X) = Var\left(\frac{X_1+\dots+X_s}{s}\right) = \frac{1}{s^2} Var(X_1+\dots+X_s) \overset{*}{=} \frac{1}{s^2} [Var(X_1) + \dots + Var(X_s)] = \frac{1}{s^2} s \cdot Var(X_i) = \frac{Var(X_i)}{s} \leq \frac{1}{4s}$

* The step $(*)$ is possible only because the variables $X_i$ are independent.

We see that the variance can be reduced arbitrarily by increasing the number of respondents/hash functions.

Note that the variable $s \cdot X$ (i.e., the sum, not the average, of the individual $X_i$s) is the sum of independent indicators with the same distribution, and thus has a binomial distribution with parameters $s$ and $p$. Its variance will be $sp(1-p)$, i.e., it increases with $s$.


## Algorithm for computing the sensitivity of a seed (optional)

  - In the lecture we saw a plot of seed sensitivity for BLAST, i.e., the probability that in a random alignment of a certain length and similarity level, we find $k$ consecutive matches.
  - For this probability, we do not have a simple formula, but it can be computed by dynamic programming.
  - Consider a seed of length $k$ (as in the BLAST program for nucleotides; in the lecture, the seed length was denoted $w$, now $k$).
  - Consider a probabilistic alignment model, where each position has probability $p$ of being a match and $(1-p)$ of being a mismatch or gap, and the alignment has length $L$.
  - Random variable $X_i = 1$ if at position $i$ there is a match, 0 otherwise
  - Random variable $Y_i = 1$ if position $i$ is the start of a seed, i.e., if $X_i=1, X_{i+1}=1, \dots, X_{i+k-1}=1$
  - $\Pr(Y_i = 1) = p^k$, since $X_i$ are mutually independent
  - Let $Y = \sum_{i=1}^{L-k-1} Y_i$
  - By linearity of expectation we can easily estimate $E(Y) = (L-k+1)p^k$
  - But we are interested in $\Pr(Y>0) = 1-\Pr(Y=0)$
  - $\Pr(Y=0) = \Pr(Y_1=0 \wedge \dots \wedge Y_{L-k+1}=0)$
  - Why doesn't $\Pr(Y=0) = \Pr(Y_i = 0)^{L-k+1}$ hold?
      - The individual $Y_i$ are not independent, e.g., $\Pr(Y_{i+1}=1\|Y_i=1)=p$
      - In the sequence of $Y_i$s, ones tend to cluster together
  - The probability of absence of a seed $\Pr(Y=0)$ can be computed by dynamic programming
  - Let $A[n]$ be the probability of absence of a seed in the first $n$ columns of the alignment ($0\le n\le L$)
  - We distinguish cases according to how many 1s occur at the very end of sequence  $X_1,\dots,X_n$
      - This number can be $0,1,\dots k-1$ (if it were $k$ or more, we would have a seed occurrence)
  

$A\[n\] = \\left\\{\\begin{array}{ll} 1 & \\mbox{if } n \< k\\\\\\\\
\\sum\_{i=0}^{k-1} p^i (1-p)A\[n-i-1\] & \\mbox{if } n \\ge k
\\end{array}\\right.$

  - In the second line, $p^i(1-p)$ corresponds to $\Pr(X_1...X_n\mbox{ ends with exactly }i\mbox{ ones})$
  - $A[n-i-1]$ is $\Pr(X_1\dots X_{n-i-1}\mbox{ does not contain a seed})$, which is the same as
$\Pr(X_1 \dots X_n\mbox{ does not contain a seed }\| X_1 \dots X_n\mbox{ ends with exactly }i\mbox{ ones})$
