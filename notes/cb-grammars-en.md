---
title: "Tutorial for biologists and computer scientists: Context-Free Grammars"
---

* TOC
{:toc}

## Introduction

  - Stochastic context-free grammars are used to model RNA structure (to be covered in the next lecture)
  - We will now look at [context-free grammars](https://www.cs.rochester.edu/~nelson/courses/csc_173/grammars/cfg.html), which do not have probabilities
  - Introduced by Noam Chomsky in linguistics in the 1950s, also important in computer science (for example in compilers of programming languages)
  <!-- - Students who took the Formal Languages and Automata Theory course may be familiar with context free grammars and do not need this part of the tutorial -->

## What is a grammar

  - Example: Set of rules $S\to aSb$, $S\to \epsilon$ (also written in a shorter form as $S\to aSb\|\epsilon$)
  - Two types of symbols: terminals (lowercase letters), nonterminals (uppercase letters)
  - Rules rewrite a nonterminal into a string of terminals and nonterminals (can also be the empty string, denoted by $\epsilon$)
  - The nonterminal S is the "start symbol"

**Using a grammar** to generate strings

  - Start with the start nonterminal S
  - At each step, rewrite the leftmost nonterminal according to one of the rules
  - Stop when there are no nonterminals left
  - Example: $S\to aSb \to aaSbb \to aaaSbbb \to aaaSbbb$ (the last step uses the rule $S\to \epsilon$)
  - What strings can this grammar generate?
      - Of the form aa...abb...b with the same number of a's and b's (computer scientists write $a^kb^k$ for $k \ge 0$)

## Exercises

  - Construct a grammar for words of the form aa..abb..b where the number of a's is equal to or greater than the number of b's, $a^ib^j$ for $i\ge j$
      - $S\to aSb \| aS \| \epsilon$
  - Construct a grammar for words of the same type, where the number of a's is greater than the number of b's, i.e. $i>j$
      - $S\to aSb \| aT$, $T\to aT \| \epsilon$ or $S\to aSb \| aS \| a$
  - Construct a grammar for well-parenthesized expressions using parentheses (),\[,\]. For example, \[()()(\[\])\] is well-parenthesized, but \[(\]) is not.
      - $S\to SS\| (S) \| \[S\] \| \epsilon$
      - example of a derivation in this grammar:
        S-\>\[S\]-\>\[SS\]-\>\[SSS\]-\>\[(S)SS\]-\>\[()SS\]-\>\[()(S)S\]-\>\[()()S\]-\>\[()()(S)\]-\>\[()()(\[S\])\]-\>\[()()(\[\])\]

## Parsing a string using a grammar

The task is to determine how a string could have been generated using the rules

  - The grammar for well-parenthesized expressions helps us determine which parenthesis matches which: those that were generated in one step
  - Similarly, in the next lecture we will have a grammar for RNA, which will generate paired nucleotides in one step

## More exercises

  - Construct a grammar for DNA palindromes, i.e. sequences that, when reversed and complemented, yield the same sequence, e.g. GATC
      - $S\to gSc \| cSg \| aSt \| tSa \| \epsilon$
  - RNA hairpins with an arbitrarily long paired part and 3 unpaired nucleotides in the middle
      - $S\to gSc \| cSg \| aSu \| uSa \| aaa \| aac \| aag \| aau \| ... \| uuu$

  - Harder example: Construct a grammar for words with the same number of a's and b's in any order
      - $S\to \epsilon \| aSbS \| bSaS$
      - how does it generate aababbba?
      - why can it generate all such strings?
