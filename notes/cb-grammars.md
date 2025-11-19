---
title: "Cvičenia pre biológov aj informatikov: Bezkontextové gramatiky"
---

* TOC
{:toc}


## Úvod

  - Na modelovanie štruktúry RNA sa používajú stochastické bezkontextové
    gramatiky (bude na ďalšej prednáške)
  - My si teraz ukážeme [bezkontextové
    gramatiky](https://www.cs.rochester.edu/~nelson/courses/csc_173/grammars/cfg.html),
    ktoré nemajú pravdepodobnosti
  - Zaviedol Noam Chomsky v lingvistike 50-te roky 20. storočia, tiež
    dôležité v informatike

## Čo je to gramatika

  - Príklad: $S\to aSb$, $S\to \epsilon$ (píšeme aj skrátene $S\to aSb\|\epsilon$)
  - Dva typy symbolov: terminály (malé písmená), neterminály (veľké
    písmená)
  - Pravidlá prepisujúce neterminál na reťazec terminálov a neterminálov
    (môže byť aj prázdny reťazec, ktorý označujeme $\epsilon$)
  - Neterminál S je "štartovací"

**Použitie gramatiky** na generovanie reťazcov

  - Začneme so štartovacím neterminálom S
  - V každom kroku prepíšeme najľavejší neterminál podľa niektorého
    pravidla
  - Skončíme, keď nezostanú žiadne neterminály
  - Príklad: $S\to aSb \to aaSbb \to aaaSbbb \to \epsilon$
  - Aké všetky slová vie táto gramatika generovať?
      - V tvare aa...abb...b s rovnakým počtom á-čok a b-čiek
        (informatici píšu $a^kb^k$)

## Cvičenia

  - Zostavte gramatiku na slová typu aa..abb..b kde á-čok je rovnako
    alebo viac ako b-čok, $a^ib^j$ pre $i\ge j$
      - $S\to aSb \| aS \| \epsilon$
  - Zostavte gramatiku pre slová toho istého typu, kde á-čok je viac ako
    b-čok, t.j. $i>j$
      - $S\to aSb \| aT$, $T\to aT \| \epsilon$ alebo $S\to aSb \| aS \| a$
  - Zostavte gramatiku pre dobre uzátvorkované výrazy zo zátvoriek
    (,),\[,\]. Napr. \[()()(\[\])\] je dobre uzátvorkovaný, ale \[(\])
    nie je.
      - $S\to SS\| (S) \| \[S\] \| \epsilon$
      - príklad odvodenia v tejto gramatike:
        S-\>\[S\]-\>\[SS\]-\>\[SSS\]-\>\[(S)SS\]-\>\[()SS\]-\>\[()(S)S\]-\>\[()()S\]-\>\[()()(S)\]-\>\[()()(\[S\])\]-\>\[()()(\[\])\]

## Parsovanie reťazca pomocou gramatiky

Úloha je určiť, ako mohol byť reťazec vygenerovaný pomocou pravidiel

  - Gramatika pre dobre uzátvorkované výrazy nám pomôže určiť, ktorá
    zátvorka patrí ku ktorej: tie, ktoré boli vygenerované v jednom
    kroku
  - Podobne na budúcej prednáške budeme mať gramatiku pre RNA, ktorá bude v jendom kroku generovať spárené nukelotidy

## Ďalšie cvičenia

  - Zostavte gramatiku na DNA palindrómy, t.j. sekvencie, ktoré zozadu
    po skomplementovaní báz dajú to isté, ako napr. GATC
      - $S\to gSc \| cSg \| aSt \| tSa \| \epsilon$
  - Vlásenky RNA s ľubovoľne dlhou spárovanou časťou a 3 nespárovanými
    nukleotidmi v strede
      - $S\to gSc \| cSg \| aSu \| uSa \| aaa \| aac \| aag \| aau \| ... \| uuu$

  - Tažší príklad: Zostavte gramatiku na slová s rovnakým počtom a-čok a
    b-čok v ľubovoľnom poradí
      - $S\to \epsilon \| aSbS \| bSaS$
      - ako bude generovať aababbba?
      - preco vie vygenerovat vsetky take retazce?

