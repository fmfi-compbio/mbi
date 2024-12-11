---
title: "Cvičenia pre informatikov: RNA štruktúra"
---

* TOC
{:toc}



## Opakovanie Nussinovovej algoritmu

Z cvičných príkladov na skúšku

  - Vyplnte maticu dynamického programovania (Nussinovovej algoritmus)
    pre nájdenie najväčšieho počtu dobre uzátvorkovaných spárovaných báz
    v RNA sekvencii GAACUUCACUGA (dovoľujeme len komplementárne páry
    A-U, C-G) a nakreslite sekundárnu štruktúru, ktorú algoritmus
    našiel.

## Rozsirenia Nussinovovej algoritmu

  - lahke: kazdy par i,j musi mat vzdialenost |i-j|\>=3 (RNA sa na
    kratsom useku nevie ohnut o 180 stupnov)
  - tazsie (bolo s hintom na skuske): chceme davat skore iba
    "stackovanym parom", t.j. ak i a j aj i+1 a j-1 su sparovane,
    dostaneme +1, osamotene pary nedostavaju ziadne skore. Úlohou je
    opäť pre danú sekvenciu nájsť dobre uzátvorkovanú štruktúru s
    maximálnym skóre.
      - pomocka: pouzijeme dve tabulky A a B, pričom A\[i,j\] obsahuje
        maximálne skóre pre podreťazec X\[i...j\] a B\[i...j\] obsahuje
        maximálne skóre pre podreťazec X\[i...j\], za predpokladu, že
        X\[i\] a X\[j\] sú spárované v štruktúre (táto hodnota je
        definovaná iba pre i a j, kde sú X\[i\] a X\[j\]
        komplementárne).
      - zjednodusenu verziu mate na domacej ulohe

## Stochasticke bezkontextove gramatiky

  - Ako asi funguje algoritmus, ktory hlada najpravdepodobnejsie
    odvodenie?
      - Najskor pre jednoduchu gramatiku z prednasky s jednym neterminalom S-\>aSu\|uSa\|cSg\|gSc\|aS\|cS\|gS\|uS\|Sa\|Sc\|Sg\|Su\|SS\|epsilon
      - Ako by sa upravilo pre zlozitejsie gramatiky s viac neterminalmi, napr. tie pre kovariancne modely?
      - rozsirme Nussinovovej algoritmus o dalsi rozmer - neterminal, z
        ktoreho je podretazec X\[i...j\] vygenerovany
      - vo vseobecnych gramatikach vzniknu nejake problemy pri niektorych typoch pravidiel, napr. A-\>B alebo A-\>BCD. Vedeli by sme sa tymto problemom vyhnut zmenou gramatiky?
  - Je najpravdepodobnejsie odvodenie to iste ako najpravdepodobnejsia
    sekundarna struktura pri gramatike z prednasky?
      - jednu strukturu vieme vyjadrit pomocou viacerych odvodeni, napr.
        v jednoduchej strukture nizsie vieme slucku `ccu` generovat zlava
        aj sprava (cS vs Su), tiez hocikde vieme spravit S-\>SS a potom
        jedno S znicit
```
    acgccucgu
    (((...)))
```
  - Viete zmenit gramatiku tak, aby najlavejsie odvodenia zodpovedali 1
    k 1 sekundarnym strukturam?
      - napr. S-\>aS\|cS\|gS\|tS\|aSuS\|uSaS\|cSgS\|gScS\|epsilon
      - vid clanok Dowell RD, Eddy SR. [Evaluation of several lightweight
        stochastic context-free grammars for RNA secondary structure
        prediction.](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-5-71) BMC bioinformatics. 2004 Jun 4;5(1):71.
