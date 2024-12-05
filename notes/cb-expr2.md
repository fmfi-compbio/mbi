---
title: "Cvičenia pre biológov: Nadreprezentácia, hľadanie motívov"
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

## Sekvenčné motívy, program MEME

  - Vazobne miesta transkripcnych faktorov sa casto reprezentuju ako
    sekvencne motivy
  - Ak mame skupinu sekvencii, mozeme hladat motiv, ktory maju spolocny
  - Znamy program na tento problem je MEME
  - Chodte na stranku <https://meme-suite.org/>
  - Zvolte nastroj MEME a v casti *Input the primary sequences* zvolte
    *Type in sequences* a zadajte [tieto
    sekvencie](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb11/seq.fa)
  - Pozrite si ostatne nastavenia. Co asi robia?
  - Ak server pocita dlho, mozete si pozriet [vysledky](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb11/MEME.html)

## Kvasinkové transkripčné faktory v SGD

  - Yeast genome database SGD obsahuje pomerne podrobne stranky pre
    jednotlive transkripcne faktory
  - Pozrime si stranku pre transkripcny faktor [GAL4](https://www.yeastgenome.org/locus/S000006169/regulation)
  - A tiez podstranku pre [vazobny motif](http://yetfasco.ccbr.utoronto.ca/MotViewLong.php?PME_sys_qf2=1510)
  - Mozeme si najst gen G4 aj v databaze [JASPAR](https://jaspar2020.genereg.net/)

