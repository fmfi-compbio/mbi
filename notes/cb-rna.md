---
title: "Cvičenia pre biológov: RNA štruktúra"
---

  - Znama databaza rodin RNA genov je Rfam: <https://rfam.xfam.org/>
  - Najdite si v nej rodinu RF00015 (U4 spliceosomal RNA)
  - V casti Secondary structure si mozete pozriet obrazky farebne
    kodovane podla roznych kriterii
      - Skuste pochopit, co jednotlive obrazky a ich farby znamenaju
  - Jedna z mnohych ludskych kopii je tato:

```
AGCTTTGCGCAGTGGCAGTATCGTAGCCAATGAGGTTTATCCGAGGCGCG
ATTATTGCTAATTGAAAACTTTTCCCAATACCCCGCCATGACGACTTGAA
ATATAGTCGGCATTGGCAATTTTTGACAGTCTCTACGGAGA
```

  - Skuste ju najst v ludskom genome nastrojom BLAT v [UCSC genome
    browseri](https://genome-euro.ucsc.edu)
  - Pozrite si tracky GENCODE genes, conservation, RepeatMasker v jej
    okoli
  - Vo verzii hg19 (kam sa viete z inej verzii dostat cez horne menu
    View-\>In Other Genomes) je track "CSHL Sm RNA-seq" ktory obsahuje
    RNASeq kratkych RNA z roznych casti buniek, zapnite si v jeho
    nastaveniach aj zobrazenie RNA z jadra (nucleus)
  - Zadajte sekvenciu na [RNAfold serveri](http://rna.tbi.univie.ac.at/cgi-bin/RNAWebSuite/RNAfold.cgi)
  - Ak vypocet dlho trva, pozrite si [vysledok](http://rna.tbi.univie.ac.at//cgi-bin/RNAWebSuite/RNAfold.cgi?PAGE=3&ID=fdr_TwjicB)
  - Podoba sa na strukturu zobrazenu v Rfame? v com sa lisi?


  - RNA dizajn: mozete sa skusit zahrat na stranke
    <https://www.eternagame.org/web/>

## Expresia génov

**NCBI Gene Expression Omnibus** <http://www.ncbi.nlm.nih.gov/geo/>

  - Databaza gene expression dat na NCBI
  - Do Search okienka zadajme *GDS2925*
  - Mali by sme dostat dataset *Various weak organic acids effect on
    anaerobic yeast chemostat cultures*
  - Mozeme si pozriet zakladne udaje, napr. citation, platform
  - Link "Expression profiles" nam zobrazi grafy pre rozne geny
  - Pri kazdom profile mozeme kliknut na profile neighbors, aby sme
    videli geny s podobnym profilom
  - Data analysis tools, cast Cluster heatmaps, K-means, skuste rozne
    pocty clustrov
      - napr.
        [K=4](http://www.ncbi.nlm.nih.gov/geo/gds/analyze/kmeans2.cgi?&ID=GDS2925&dist=1&method=0&PC=1&NC=5&k=4)
        a
        [K=5](http://www.ncbi.nlm.nih.gov/geo/gds/analyze/kmeans2.cgi?&ID=GDS2925&dist=1&method=0&PC=1&NC=5&k=5)
        pre Pearsonovu korelaciu
      - mozeme is pozriet aj hierarchicke zhlukovanie

