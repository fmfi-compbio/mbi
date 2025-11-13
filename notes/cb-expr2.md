---
title: "Cvičenia pre biológov: Nadreprezentácia"
---

* TOC
{:toc}

## Nadreprezentácia (cvičenie pri počítači)

Data o expresii ludskych genov v roznych tkanivach a podobne v **UCSC
genome browseri**

  - Chodte na [genome browser](https://genome-euro.ucsc.edu)
  - Zvolte *Tools-\>Other tools-\>Gene Sorter*, 'assembly'' na *hg38*, *sort by* na
    *Expression (GTEx)*, a do okienka *search* zadajme identifikator
    genu *PTPRZ1*
      - Dostane tabulku genov s podobnym profilom expresie ako PTPRZ1
        (červená je vysoká expresia, zelená nízka)
      - Pouzijeme predspracovany [zoznam tychto genov](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb08/zoznam_genov.txt) v textovom formate
  - <https://biit.cs.ut.ee/gprofiler/> mena genov skopirujme do policka
    *Query*, stlacte g:Profile\!
      - Ak by výpočet dlho trval, nájdete ho aj
        [tu](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb08/g_Profiler.html)
      - Vo výslednej tabuľke je každý riadok jedna funkcna kategoria, v
        ktorej su geny s tymto profilom expresie nadreprezentovane,
        kazdy stlpec jeden gen.
      - V spodnej casti tabuly su aj asociacie k chorobam a k
        transkripcnym faktorom, ktore by mohli prislusne geny regulovat
  - Co by sme na zaklade nadreprezentovanych kategorii usudzovali o gene
    PTPRZ1?

  - Najdite tento gen v [Uniprote](https://www.uniprot.org/), potvrdzuje
    nase domnienky?
  - Vratme sa do genome browsera, pozrime si [PTPRZ1 gen v genome](https://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr7%3A121873089-122062036)
  - V browseri su rozne tracky tykajuce sa expresie, napr. GTEx.
    Precitajte si, co je v tomto tracku zobrazene, zapnite si ho a
    pozrite si expresiu okolitych genov okolo PTPRZ1

