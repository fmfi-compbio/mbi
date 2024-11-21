---
title: "Cvičenia pre biológov: Zhlukovanie, nadreprezentácia, viacnásobné testovanie"
---

* TOC
{:toc}


Na týchto cvičeniach sa budeme venovať trom štatistickým témam
súvisiacim s komparatívnou genomikou a s analýzou expresie génov. Tieto
techniky sa však využívajú aj v iných oblastiach a môžete sa s nimi
často stretnúť v genomických článkoch.

## Zhlukovanie

- Máme vstupné dáta, väčšinou ako vektory (postupnosti čísel) dĺžky $n$.
- Snažíme sa ich rozdeliť do skupín tak, aby dáta v rámci skupiny boli podobné a medzi skupinami rôzne.

Využitie:
- hľadanie génov s podobným profilom expresie,
- hľadanie skupín pacientov s podobným profilom expresie génov (objavovanie podtypov nejakej choroby),
- hľadanie rodín podobných proteínov,
- automatická segmentácia obrázkov (napríklad rozlíšiť jednotlivé políčka microarray alebo gélu od pozadia).

Na prednáške sme videli hierarchické zhlukovanie, ktoré z dát vytvorilo
strom. Teraz si ukážeme zhlukovanie, ktoré sa snaží dáta rozdeliť na $k$
skupín, kde $k$ je vopred daný parameter.

### Zhlukovanie k-means, definícia problému

Pozri tiež prezentáciu 

- **Vstup:** n-rozmerné vektory $x_1, x_2, \dots, x_t$ a počet zhlukov $k$
- **Výstup:** 
  - Rozdelenie vektorov do $k$ zhlukov zapísané ako $c_1, c_2, \dots, c_t$, kde  $c_i\in \{1,2,\dots k\}$ je priradenie vektoru $x_i$ k zhluku
  - n-rozmerné vektory $\mu_1, \mu_2, \dots, \mu_k$ - centrá každého zhlukov
- **Úloha**: minimalizovať súčet štvorcov vzdialeností od každého vektoru k centru jeho zhluku:
$J(\mu, c) = \sum_{i = 1}^t \big\| x_i - \mu_{c_i} \big\|_2^2$
  - $\big\| x_i - \mu_{c_i} \big\|_2^2$ je druhá mocnina vzdialenosti vektora $x_i$ od centra jeho zhluku

Pre vektory $a=(a_1,\dots, a_n)$ a $b=(b_1,\dots b_n)$ je druhá
mocnina vzdialenosti $\big\|a-b\|_2^2 = \sum_{i=1}^n (a_i-b_i)^2$.


### Algoritmus k-means

Heuristika, ktorá nenájde vždy najlepšie zhlukovanie. Začne z nejakého
zhlukovania a postupne ho zlepšuje.

1. inicializácia: náhodne vyber $k$ centier $\mu_1, \mu_2, \dots, \mu_k$, napr. z našich vstupných vektorov
2. opakuj kým sa niečo mení:
  - priraď každý bod k najbližšiemu centru:
    $c_i = \arg\min_j \big\| x_i - \mu_j \big\|_2$
  - vypočítaj nové centrá:
      $\mu_j = \operatorname{avg_{i : c_i = j}} x_i$ (spriemerujeme všetky vektory v každom zhluku)

## Nadreprezentacia, obohatenie (enrichment)

- Mnohé celogenómové analýzy nám dajú zoznam génov, ktoré sa v nejakom
  ukazovateli výrazne líšia od priemeru.
- Napríklad gény s pozitívnym výberom v komparatívnej genomike, gény
  výrazne nadexprimované alebo podexprimované v microarray
  experimentoch, gény regulované určitým transkripčným faktorom a pod.
- Niektoré z nich budú preskúmanejšie (známa funkcia a pod.), niektoré
  môžu mať nejaké údaje o funkcii prenesené z homologov a ďalšie môžu
  byť úplne neznáme
- Čo s takým zoznamom "zaujímavých génov"?
  - možnosť 1: vybrať si z neho niekoľko málo zaujímavých kandidátov a
    preskúmať ich podrobnejšie (experimentálne alebo bioinformaticky)
  - možnosť 2: zistiť, či táto celá skupina je obohatená o gény určitých
    skupín
    - napr. v pripade pozitivneho vyberu nam casto vychadzaju geny
      suvisiace s imunitou, lebo su pod velkym evolucnym tlakom od patogenov
    - takato analyza nam teda da informaciu o suvislostiach medzi
      roznymi procesmi
- Priklad (Kosiol et al)
  - 16529 genov celkovo, 70 genov v GO kategorii innate immune response (0.4% zo vsetkych genov)
  - 400 genov s pozitivnym vyberom, mame 8 genov s innate immune response (2% zo vsetky genov s poz. vyb.)
  - Celkovy pocet genov $n$, imunitnych $n_i$, pozitivny vyber $n_p$, imunitnych
    s poz. vyb. $n_{ip}$.
  - Kontingencna tabulka

|          | Pozitivny vyber | Bez poz. vyberu | Sucet      |
| -------- | --------------- | --------------- | ---------- |
| Imunitne | 8 ($n_{ip}$)    | 62              | 70 ($n_i$) |
| Ostatne  | 392             | 16067           | 16459      |
| Sucet    | 400 ($n_p$)     | 16129           | 16529 ($n$)|

  - Nulova hypoteza: geny v nasom zozname boli nahodne vybrane z celeho
    genomu, t.j. ak v celom genome je frekvencia imunitnych genov $n_i/n$
    (cca 0.4%), vo vzorke velkosti $n_p$ (geny s pozitivnym vyberom)
    ocakavame cca $np \cdot (n_i / n)$ imunitnych genov.
      - Aj v nulovej hypoteze vsak vzorka velkosti $n_p$ cisto nahodou moze
        obsahovat viac alebo menej takych genov.
      - Presnejsie mame urnu s $n_i$ (70) bielymi a $n-n_i$ (16459) ciernymi
        gulickami, vytiahneme nahodne $n_p$ (400) guliciek, kolko bude
        medzi nimi bielych, nazvime tuto nahodnu premennu $X_{ip}$
      - v nasom priklade by sme ocakavali 1.7 genu s innate immune
        response, ale mame 8 (4.7xviac)
  - Rozdelenie pravdepodobnosti $X_{ip}$ je hypergeometricke, t.j.
    $\Pr(X_{ip}=n_{ip}) = {n_i \choose n_{ip}}{n-n_i\choose n_p-n_{ip}}/{n\choose n_p}$
  - Aka je pravdepodobnost, ze v nulovej hypoteze bude $X_ip$ tolko, kolko
    sme namerali alebo viac? (Chvost rozdelenia). V nasom pripade
    p-value 2.8e-4.
  - Hypergeometric or Fisher's exact test, pripadne ich aproximacie pre
    velke hodnoty v tabulke (chi^2 test) zisti, ci sa nasa tabulka velmi
    lisi od toho, co by sme ocakavali v nulovej hypoteze

  - Suvisiace clanky
      - Reimand, Jüri, et al. "Pathway enrichment analysis and
        visualization of omics data using g: Profiler, GSEA, Cytoscape
        and EnrichmentMap." Nature protocols 14.2 (2019): 482.
        [1](https://www.nature.com/articles/s41596-018-0103-9)

  - Existuju web servery, napr. GOrilla pre ludske geny:
    <https://cbl-gorilla.cs.technion.ac.il/>, DAVID
    <https://davidbioinformatics.nih.gov/>, g:Profiler
    <https://biit.cs.ut.ee/gprofiler/>
  - Treba dat pozor, ci pocitaju to co chceme


  - Kod v statistickom systeme R na pocitanie hypergeometrickeho
    rozdelenia

```
    > dhyper(0:70, 70, 16529-70, 400);
     [1]  1.793421e-01  3.126761e-01  2.679872e-01  1.505169e-01  6.231088e-02
     [6]  2.027586e-02  5.400796e-03  1.210955e-03  2.332580e-04  3.920215e-05
    [11]  5.818723e-06  7.702558e-07  9.166688e-08  9.873221e-09  9.678760e-10
    [16]  8.677204e-11  7.143849e-12  5.420388e-13  3.802134e-14  2.472342e-15
    [21]  1.493876e-16  8.405488e-18  4.412274e-19  2.164351e-20  9.935473e-22
    [26]  4.273662e-23  1.724446e-24  6.533742e-26  2.326517e-27  7.791092e-29
    [31]  2.455307e-30  7.285339e-32  2.036140e-33  5.361856e-35  1.330660e-36
    [36]  3.112566e-38  6.862558e-40  1.426089e-41  2.792792e-43  5.153006e-45
    [41]  8.955105e-47  1.465159e-48  2.255667e-50  3.265636e-52  4.442631e-54
    [46]  5.674366e-56  6.797781e-58  7.629501e-60  8.012033e-62  7.860866e-64
    [51]  7.193798e-66  6.129013e-68  4.851139e-70  3.558526e-72  2.412561e-74
    [56]  1.506983e-76  8.641725e-79  4.530590e-81  2.161126e-83  9.326620e-86
    [61]  3.617279e-88  1.250737e-90  3.817900e-93  1.016417e-95  2.323667e-98
    [66] 4.469699e-101 7.034762e-104 8.698702e-107 7.924236e-110 4.728201e-113
    [71] 1.386176e-116
    phyper(7, 70, 16529-70, 400, lower.tail=FALSE);
    # pr pocet bielych>7 (t.j. >=8) ak taham 400 z vreca so 70 bielymi a 16529-70 ciernymi
    # sucet cisiel z tabulky od 2.332580e-04 az po koniec
    
    d = dhyper(0:15, 70, 16529-70, 400);
    plot(0:15,d)
    
    # test pre danu tabulku
    a=matrix(c(8,62,392,16067),nrow=2, ncol=2)
    fisher.test(a,alternative = "greater")
```

## Multiple testing correction

  - V mnohych situaciach robime vela testov toho isteho typu, kazdy ma
    urcitu p-value
  - Napr. testujeme 1000 genov v genome na pozitivny vyber, zvolime tie,
    kde p-value je menšia ako 0.05
  - Alebo testujeme obohatenie 1000 funkcnych kategorii v nejakej vzorke
    genov, zvolime tie, kde p-value je menšia ako 0.05
  - Problem: ak kazda z 1000 kategorii ma 5% sancu tam byt len nahodou,
    ocakavali by sme 50 cisto nahodnych pozitivnych vysledkov. Ak sme
    napr. nasli 100 pozitivnych vysledkov (obohatenych kategorii), cca
    polovica z nich je zle
  - Preto potrebujeme pri velkom mnozstve testov umelo znizit prah na
    p-value tak, aby nahodny sum netvoril velke percento nasich
    vysledkov
  - Toto sa vola multiple testing correction, je viac technik, napr. FDR
    (false discovery rate)

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
