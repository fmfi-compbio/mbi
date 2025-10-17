---
title: Materials
---

[Week 1](#W1) · [Week 2](#W2) · [Week 3](#W3) · **[Week 4](#W4)** · [Week 5](#W5) · [Week 6](#W6) · [Week 7](#W7)
· [Week 8](#W8) · [Week 9](#W9) · [Week 10](#W10) · [Week 11](#W11) · [Week 12](#W12) · [Week 13](#W13)


## Literature

- **BV**: Brejová, Vinař: Metódy v bioinformatike. (preliminary
  version of lecture notes in Slovak, only several lectures) [pdf](./pdf/Skripta-2015-10-01.pdf) 
- **DEKM**: Durbin, Eddy, Krogh, Mitchison: Biological sequence
  analysis: Probabilistic Models of Proteins and Nucleic Acids.
  Cambridge University Press 1998. Can be studied in the FMFI library
  under code I-INF-D-21
- **ZB**: Zvelebil, Baum: Understanding Bioinformatics. Taylor &
  Francis 2008. Can be studied in the FMFI library under code
  I-INF-Z-2

For each lecture, we list the book chapters best corresponding to the
covered material. However, the lecture may differ substantially from the
listed chapters which serve as the source of additional information.

**Recordings of lectures in Slovak from 2018/19**

- [playlist on youtube](https://www.youtube.com/playlist?list=PLU2XVjShDFwVeTDmo9Uv2NHWz3ijUjy5p)

## Notes and presentations

L: lecture (everybody), TI: tutorial for computer science/informatics students, TB: tutorial for biology/chemistry/physics students 

<a name="W1"></a>
### Sept. 25

#### L: Introduction, course rules, sequencing and genome assembly

{% include pdf.html file="p-intro" name="pdf 1" show=1 dot=1
%} {% include pdf.html file="p-seq" name="pdf 2" show=1 dot=1
%} {% include youtube.html id="3HI-wKmTuSs" name="video 1" show=1 dot=1
%} {% include youtube.html id="iHt5WPVFd7E" name="video 2" show=1 dot=1
%} BV chapter 1

#### TI: Introduction to biology

{% include pdf.html file="ci-introbio" name="pdf" show=1 dot=1
%} {% include notes.html file="ci-introbio" name="notes" show=1 dot=1
%} {% include youtube.html id="EtPqUOHTJD4" name="video" show=1 dot=1
%} ZB chapter 1

#### TB: Introduction to computer science, UCSC genome browser

{% include pdf.html file="cb-introcs" name="pdf" show=1 dot=1
%} {% include notes.html file="cb-browser" name="notes" show=1 dot=0
%}

<a name="W2"></a>
### Oct. 2

#### L: Genome assembly 2

{% include pdf.html file="p-seq2" name="pdf" show=1 dot=1
%} {% include youtube.html id="YvZlTL1qyUg" name="video" show=1 dot=0
%}

#### TI: Introduction to probability, genome coverage by sequencing reads

{% include notes.html file="ci-prob-en" name="notes (en, current)" show=1 dot=1
%} {% include notes.html file="ci-prob" name="notes (sk, older)" show=1 dot=1
%} {% include notebook.html file="ci-prob" name="colab" show=1 dot=0
%}

#### TB: Introduction to dynamic programming, introduction to probability

{% include pdf.html file="cb-dp-code" name="pdf" show=1 dot=1
%} {% include notes.html file="cb-dp" name="notes 1" show=1 dot=1
%} {% include notes.html file="cb-prob" name="notes 2" show=1 dot=0
%} 


<a name="W3"></a>
### Oct. 9

#### L: Sequence alignment: Smith-Waterman, Needleman-Wunsch, scoring

{% include pdf.html file="p-aln1" name="pdf" show=1 dot=1
%} {% include youtube.html id="0GkhkRiqbl4" name="video" show=1 dot=1
%} BV chapter 2, DEKM chapter 2.1-2.4, 2.8, ZB chapter 4.1-4.4, 5.1-5.2

#### TI: Burrows-Wheeler transform

{% include pdf.html file="ci-bwt" name="pdf" show=1 dot=0
%}

#### TB: Dynamic programming for sequence alignment, dotplots

{% include notes.html file="cb-aln1" name="notes" show=1 dot=1
%} {% include pdf.html file="cb-aln" name="pdf 1" show=1 dot=1
%} {% include pdf.html file="cb-dotplots" name="pdf 2" show=1 dot=1
%}  {% include notebook.html file="cb-aln1" name="dyn. prog. in colab" show=1 dot=0 %}

<a name="W4"></a>
### Oct. 16

#### L: Sequence alignment: heuristic alignment (BLAST), statistical significance of alignments, whole genome alignments, multiple alignments

{% include pdf.html file="p-aln2" name="pdf" show=1 dot=1
%} {% include youtube.html id="jkQGXiqKbxM" name="video" show=1 dot=1
%} BV chapter 2, DEKM chapter 2.5, 2.7, 6.1-6.3; ZB chapter 4.5-4.7, 5.3-5.5

#### TI: Advanced algorithms for sequence alignment

{% include notes.html file="ci-aln1-en" name="notes (en, current)" show=1 dot=1
%} {% include notes.html file="ci-aln1" name="notes (sk, older)" show=1 dot=0
%}

#### TB: Programs for sequence alignment, scoring schemes, introduction to projects

{% include notes.html file="cb-aln2" name="notes" show=1 dot=1
%} [Projects](./Project.html)


<a name="W5"></a>
### Oct. 23
#### L: Gene finding, hidden Markov models

{% include pdf.html file="p-gene" name="pdf" show=0 dot=1
%} {% include youtube.html id="s3NiBeaiHPM" name="video" show=0 dot=1
%} BV chapter 4, DEKM chapter 3; ZB chapter 9.3, 10.4-10.7

#### TI:  Fast similarity search, BLAST, MinHash

{% include notes.html file="ci-aln2" name="notes" show=0 dot=1
%} {% include pdf.html file="ci-seeds" name="pdf" show=0 dot=0
%}

#### TB:  Hidden Markov models, E-value

{% include notes.html file="cb-hmm" name="notes" show=0 dot=1
%} {% include pdf.html file="cb-hmm-evalue" name="pdf" show=0 dot=0
%}

<a name="W6"></a>
### Oct. 30
#### L: Phylogenetic tree reconstruction (parsimony, neighbor joining, models of evolution)

{% include pdf.html file="p-phylo" name="pdf" show=0 dot=1
%} {% include youtube.html id="RzfNHvZH4l8" name="video" show=0 dot=1
%} BV chapter 3, DEKM chapter 7,8; ZB chapter 7, 8.1-8.2, video

#### TI:  Algorithms for HMM

{% include notes.html file="ci-hmm" name="notes" show=0 dot=0
%} {% include pdf.html file="ci-hmm" name="pdf" show=0 dot=0
%}

#### TB:  Substitution models, bootstrap, tree rooting

{% include notes.html file="cb-phylo1" name="notes" show=0 dot=0
%} {% include pdf.html file="cb-phylo" name="pdf" show=0 dot=0
%}

<a name="W7"></a>
### Nov. 6
#### L: Comparative genomics, detection of positive and purification selection, comparative gene finding, phylogenetic HMMs

{% include pdf.html file="p-compgen" name="pdf" show=0 dot=1
%} {% include youtube.html id="1WM4QI2qx8A" name="video" show=0 dot=1
%} BV chapter 5, ZB chapter 9.8, 10.8,

#### TI:  Substitution models

{% include notes.html file="ci-chains" name="notes" show=0 dot=1
%} {% include notebook.html file="ci-chains" name="colab" show=0 dot=0
%} {% include pdf.html file="ci-chains" name="pdf" show=0 dot=0
%}

#### TB:  Practical phylogenetic trees

{% include notes.html file="cb-phylo2" name="notes" show=0 dot=0
%}


<a name="W8"></a>
### Nov. 13
#### L: Gene expression, clustering, classification, transcription factors, sequence motifs

{% include pdf.html file="p-expr" name="pdf" show=0 dot=1
%} {% include youtube.html id="GFJ_oDV1KGU" name="video" show=0 dot=1
%} DEKM chapter 5.1, 11.5, ZB chapter 6.6,15.1,16.1-16.5,17.1

#### TI:  Felsenstein algorithm, algorithms for HMM and phyloHMM

{% include notes.html file="ci-phylo" name="notes" show=0 dot=0
%}

#### TB:  K-means clustering, enrichment, multiple testing correction

{% include notes.html file="cb-expr" name="notes" show=0 dot=1
%} {% include pdf.html file="cb-kmeans" name="pdf" show=0 dot=0
%}

<a name="W9"></a>
### Nov. 20
#### L: Protein structure and function

{% include pdf.html file="p-prot" name="pdf" show=0 dot=1
%} {% include youtube.html id="ugMM81jZRpc" name="video" show=0 dot=1
%} DEKM chapter 5; ZB chapter 4.8-4.10, 6.1-6.2, 13.1-13.2

#### TI:  Motif finding by EM and Gibbs sampling

{% include notes.html file="ci-motifs" name="notes" show=0 dot=0
%}

#### TB:  Introduction to context-free grammars, enrichment, motifs

{% include notes.html file="cb-grammars" name="notes1" show=0 dot=1
%} {% include notes.html file="cb-expr2" name="notes2" show=0 dot=0
%}

<a name="W10"></a>
### Nov. 27
#### L: RNA, secondary structure, Nussinov algorithm, stochastic context-free grammars, RNA family profiles

{% include pdf.html file="p-rna" name="pdf" show=0 dot=1
%} {% include youtube.html id="_Hh03Khsr9k" name="video" show=0 dot=1
%} DEKM chapter 10, ZB chapter 11.9

#### TI: Examples of biological databases, introduction to context-free grammars, HMM topology

{% include notes.html file="ci-tools" name="notes1" show=0 dot=1
%} {% include notes.html file="cb-grammars" name="notes2" show=0 dot=0
%}

#### TB:  Proteins, example of command-line tools

{% include notes.html file="cb-prot" name="notes1" show=0 dot=1
%} {% include notes.html file="cb-linux" name="notes2" show=0 dot=1
%} 

<a name="W11"></a>
### Dec. 4
#### L: Population genetics

{% include pdf.html file="p-popgen" name="pdf" show=0 dot=1
%} {% include youtube.html id="7vPt1vQX21M" name="video" show=0 dot=0
%}

#### TI:  RNA structure 

{% include notes.html file="ci-rna" name="notes" show=0 dot=0
%}

#### TB:  Course summary, graphs, population genetics, RNA structure, expression data

{% include pdf.html file="cb-summary" name="pdf1" show=0 dot=1
%} {% include pdf.html file="cb-graphs" name="pdf2" show=0 dot=1
%} {% include notes.html file="cb-popgen" name="notes1" show=0 dot=1
%} {% include notes.html file="cb-rna" name="notes2" show=0 dot=1
%} {% include notebook.html file="cb-expr" name="notebook" show=0 dot=0
%} 

<a name="W12"></a>
### Dec. 11
#### L: Optional journal club presentations (no lecture)

#### TI:  Integer linear programming, course summary

{% include pdf.html file="ci-summary" name="pdf" show=0 dot=1
%} {% include notes.html file="ci-ilp" name="notes" show=0 dot=0
%} 

#### TB:  Project consultations in January

<a name="W13"></a>
### Dec. 16

