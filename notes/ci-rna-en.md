---
title: "Tutorial for Computer Scientists: RNA Structure"
---

* TOC
{:toc}


## Review of the Nussinov Algorithm

From practice questions for the exam

  - Fill in the dynamic programming matrix (Nussinov algorithm)
    to find the largest number of well-parenthesized paired bases
    in the RNA sequence GAACUUCACUGA (allowing only complementary pairs
    A-U, C-G). List which pairs of positions are paired in the secondary structure found by the algorithm.
    Explain how the value for substring GAACUAUCUG was derived: 
    what were the considered possibilities and what were their scores.

## Extensions of the Nussinov Algorithm

Easy extension: modify the algorithm so that it will output only pairs $i,j$ that have distance $\|i-j\|\>=4$. The goal is to output the best structure without pseudoknots that contains only such pairs. The reason is that in real RNA structures, hairpin loops must have at least 3 unpaired bases (RNA can bend only gradually).

Harder extension: In real RNA structures, pairs are often stacked, i.e., if $i$ pairs with $j$, then it is likely that $i+1$ pairs with $j-1$. Stacked pairs contribute significantly to the stability of RNA structures. Modify the Nussinov algorithm so that it will output a structure that maximizes the number of stacked pairs. Each stacked pair (i.e., each pair $i,j$ such that also $i+1,j-1$ is a pair) contributes +1 to the score, other pairs contribute 0. The goal is to find a well-parenthesized structure with the maximum score for the given sequence.

Hint: we use two tables A and B, where $A\[i,j\]$ contains the maximum score for the substring $X\[i\dots j\]$ and $B\[i,j\]$ contains     the maximum score for the substring $X\[i\dots j\]$, assuming that  $X\[i\]$ and $X\[j\]$ are paired in the structure (this value is defined only for i and j where $X\[i\]$ and $X\[j\]$ are complementary).

A simplified version of this exercise is in the homework.

## Stochastic Context-Free Grammars

How does the algorithm that finds the most probable derivation work?

  - First for the simple grammar from the lecture with one nonterminal $S\to aSu\|uSa\|cSg\|gSc\|aS\|cS\|gS\|uS\|Sa\|Sc\|Sg\|Su\|SS\|\epsilon$
      - Modify the Nussinov algorithm so that instead of maximizing the number of base pairs,
        it maximizes the probability of the derivation according to the grammar
  - How would the algorithm be modified for more complex grammars with more nonterminals, e.g., those for covariance models?
      - extend the Nussinov algorithm by another dimension - the nonterminal from which the substring $X\[i\dots j\]$ is generated
      - in general context-free grammars, some problems arise with certain types of rules, e.g., $A\to B$ or $A\to BCD$. Could we avoid these problems by changing the grammar?
  - Is the most probable derivation the same as the most probable secondary structure for the grammar from the lecture?
      - we can express one structure using multiple derivations, e.g.,
        in the simple structure below we can generate the loop `ccu` from the left
        or right ($cS$ vs $Su$), also anywhere we can do $S\to SS$ and then
        destroy one $S$
```
    acgccucgu
    (((...)))
```
  - Can you change the grammar so that the leftmost derivations correspond 1-to-1 to secondary structures?
      - e.g., $S\to aS\|cS\|gS\|uS\|aSuS\|uSaS\|cSgS\|gScS\|\epsilon$
      - see the article Dowell RD, Eddy SR. [Evaluation of several lightweight
        stochastic context-free grammars for RNA secondary structure
        prediction.](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-5-71) BMC bioinformatics. 2004 Jun 4;5(1):71.

