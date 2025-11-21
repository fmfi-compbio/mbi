---
title: "Tutorial for Computer Scientists: Introduction to Bioinformatics Databases and Online Tools"
---

* TOC
{:toc}


## NCBI, blast

  - National Center for Biotechnology Information
    <http://www.ncbi.nlm.nih.gov/>
  - Collects publicly accessible data from molecular biology
  - We will use the BLAST tool to search for homologs (similar sequences)
    - Let us find homologs of the following sequence in the chicken genome (choose
        nucleotide blast, database others and from the menu Refseq reference genomes,
        organism chicken (taxid:9031), program blastn)
      - This is a sequenced piece of human mRNA. Where in the chicken genome we
        found a homolog, what is its length, score, E-value, % identical bases?

```
AACCATGGGTATATACGACTCACTATAGGGGGATATCAGCTGGGATGGCAAATAATGATTTTATTTTGAC
TGATAGTGACCTGTTCGTTGCAACAAATTGATAAGCAATGCTTTCTTATAATGCCAACTTTGTACAAGAA
AGTTGGGCAGGTGTGTTTTTTGTCCTTCAGGTAGCCGAAGAGCATCTCCAGGCCCCCCTCCACCAGCTCC
GGCAGAGGCTTGGATAAAGGGTTGTGGGAAATGTGGAGCCCTTTGTCCATGGGATTCCAGGCGATCCTCA
CCAGTCTACACAGCAGGTGGAGTTCGCTCGGGAGGGTCTGGATGTCATTGTTGTTGAGGTTCAGCAGCTC
CAGGCTGGTGACCAGGCAAAGCGACCTCGGGAAGGAGTGGATGTTGTTGCCCTCTGCGATGAAGATCTGC
AGGCTGGCCAGGTGCTGGATGCTCTCAGCGATGTTTTCCAGGCGATTCGAGCCCACGTGCAAGAAAATCA
GTTCCTTCAGGGAGAACACACACATGGGGATGTGCGCGAAGAAGTTGTTGCTGAGGTTTAGCTTCCTCAG
TCTAGAGAGGTCGGCGAAGCATGCAGGGAGCTGGGACAGGCAGTTGTGCGACAAGCTCAGGACCTCCAGC
TTTCGGCACAAGCTCAGCTCGGCCGGCACCTCTGTCAGGCAGTTCATGTTGACAAACAGGACCTTGAGGC
ACTGTAGGAGGCTCACTTCTCTGGGCAGGCTCTTCAGGCGGTTCCCGCACAAGTTCAGGACCACGATCCG
GGTCAGTTTCCCCACCTCGGGGAGGGAGAACCCCGGAGCTGGTTGTGAGACAAATTGAGTTTCTGGACCC
CCGAAAAGCCCCCACAAAAAGCCG
```

## Uniprot

  - A comprehensive view of proteins, with many links to other databases, some content curated manually
    - Let's look at the well-known coronavirus Spike protein
    - Find it on <https://www.uniprot.org/> under the name SPIKE_SARS2
    - Examine its page in detail: which features were predicted by bioinformatics methods discussed in the lecture?
    - Notice one of the Pfam domains and visit its page

## UCSC genome browser

  - <https://genome-euro.ucsc.edu/>
    - Online graphical tool for genome browsing
    - Configurable, many options, user-friendly interface
    - Possibility to download data suitable for further processing or to display your own data
    - Focus on the human genome, but some other genomes also available

**Basics**

  - In the blue menu at the top, choose Genomes, then choose the human genome. In the `search term` box, enter HOXA2. In the search results (UCSC genes) choose the homeobox A2 gene on chromosome 7.
      - Let's look at this page together
      - At the top are controls for moving left, right, zooming in, zooming out
      - Below that is a chromosome schematic, with the displayed region marked in red
      - Below that is an image of the selected region, various tracks
      - Below that is a list of all tracks, which can be turned on, off, and configured
      - Clicking on an image often displays more information about the given gene or other data source
      - Exons are thick in genes, UTRs thinner, introns horizontal lines
  - Clicking on a gene or another part of some track usually gives more information about it. Clicking on the list next to the track (left edge of the image) gives more information about the track and allows you to set display parameters

**Comparative genomics**

  - In the **multiz alignments** section, you can see alignments to various other genomes (you can turn on which ones). You can see how the
    level of alignment changes when you zoom in and zoom out.
  - When you zoom in to the "base" level, i.e., about 100bp displayed, in the multiz alignment box you can see the alignment with the homologous segment in other genomes.
  - In the '''conservation by PhyloP''' section, you can see a graph of how strongly conserved individual columns of the alignment are
  - You can turn on the Placental Chain/Net track and see on which chromosomes there is an orthologous segment in other genomes

**Working with tables, downloading annotations**

  - The Tables item in the top menu allows you to query tables that contain coordinates of genes and other elements.
  - The basic thing: export, for example, all genes in the displayed region in some format:
      - sequence: fasta file of proteins, genes or mRNA with various settings
      - GTF: coordinates
      - Hyperlinks to genome browser: clickable page
  - Instead of exporting, you can look at various statistics
  - More complex use: intersection of two tables, e.g., genes that are more than 50% covered by sequence repeats
      - In the intersection, choose group: Variation and repeats, track:
        RepeatMasker, choose records that have at least 50% overlap with RepeatMasker
      - Using summary/statistics we can find out how many there are in the genome, we can browse them through Hyperlinks to genome browser
        