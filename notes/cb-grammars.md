---
title: "Cvičenia pre biológov: Bezkontextové gramatiky"
---

* TOC
{:toc}


## Bezkontextové gramatiky

  - Na modelovanie štruktúry RNA sa používajú stochastické bezkontextové
    gramatiky (bude na ďalšej prednáške)
  - My si teraz ukážeme [bezkontextové
    gramatiky](https://www.cs.rochester.edu/~nelson/courses/csc_173/grammars/cfg.html),
    ktoré nemajú pravdepodobnosti
  - Zaviedol Noam Chomsky v lingvistike 50-te roky 20. storočia, tiež
    dôležité v informatike

**Gramatika**

  - Príklad: S-\>aSb, S-\>epsilon (píšeme aj skrátene S-\>aSb|epsilon)
  - Dva typy symbolov: terminály (malé písmená), neterminály (veľké
    písmená)
  - Pravidlá prepisujúce neterminál na reťazec terminálov a neterminálov
    (môže byť aj prázdny reťazec, ktorý označujeme epsilon)
  - Neterminál S je "štartovací"

**Použitie gramatiky** na generovanie reťazcov

  - Začneme so štartovacím neterminálom S
  - V každom kroku prepíšeme najľavejší neterminál podľa niektorého
    pravidla
  - Skončíme, keď nezostanú žiadne neterminály
  - Príklad: S-\>aSb-\>aaSbb-\>aaaSbbb-\>epsilon
  - Aké všetky slová vie táto gramatika generovať?
      - V tvare aa...abb...b s rovnakým počtom á-čok a b-čiek
        (informatici píšu \(a^kb^k\))

**Cvičenia**

  - Zostavte gramatiku na slová typu aa..abb..b kde á-čok je rovnako
    alebo viac ako b-čok, \(a^ib^j\) pre \(i\ge j\)
      - S-\>aSb|aS|epsilon
  - Zostavte gramatiku pre slová toho istého typu, kde á-čok je viac ako
    b-čok, t.j. i\>j
      - S-\>aSb|aT T-\>aT|epsilon (alebo S-\>aSb|aS|a)
  - Zostavte gramatiku pre dobre uzátvorkované výrazy zo zátvoriek
    (,),\[,\]. Napr. \[()()(\[\])\] je dobre uzátvorkovaný, ale \[(\])
    nie je.
      - S-\>SS|(S)|\[S\]|epsilon
      - príklad odvodenia v tejto gramatike:
        S-\>\[S\]-\>\[SS\]-\>\[SSS\]-\>\[(S)SS\]-\>\[()SS\]-\>\[()(S)S\]-\>\[()()S\]-\>\[()()(S)\]-\>\[()()(\[S\])\]-\>\[()()(\[\])\]

**Parsovanie reťazca** pomocou gramatiky: určiť, ako mohol byt reťazec
vygenerovaný pomocou pravidiel

  - Gramatika pre dobre uzátvorkované výrazy nám pomôže určiť, ktorá
    zátvorka patrí ku ktorej: tie, ktoré boli vygenerované v jednom
    kroku

**Ďalšie cvičenia**

  - Zostavte gramatiku na DNA palindromy, t.j. sekvencie, ktore zozadu
    po skomplementovani baz daju to iste, ako napr. GATC
      - S-\>gSc|cSg|aSt|tSa|epsilon
  - Vlasenky RNA s lubovolne dlhou sparovanou castou a 3 nesparovanymi
    nukleotidmi v strede
      - S-\>gSc|cSg|aSu|uSa|aaa|aac|aag|aau|...|uuu

  - Tazsi priklad: Zostavte gramatiku na slova s rovnakym poctom acok a
    bcok v lubovolnom poradi
      - S-\>epsilon|aSbS|bSaS
      - ako bude generovat aababbba?
      - preco vie vygenerovat vsetky take retazce?

