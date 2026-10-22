---
title: "Cvičenia pre biológov: Skryté Markovove modely, E-hodnota"
---


## Príklady stavových automatov pre HMM

Uvažujme HMM so špeciálnym začiatočným stavom b a koncovým stavom e,
ktoré nič negenerujú.

  - Nakreslite HMM (stavový diagram), ktorý generuje sekvencie, ktoré
    začínajú niekoľkými červenými písmenami a potom obsahujú niekoľko
    modrých
  - Ako treba zmeniť HMM, aby dovoloval ako "niekoľko" aj nula?
  - Ako treba zmeniť HMM, aby počet červených aj modrých bol vždy párne
    číslo?
  - Ako zmeniť HMM, aby sa striedali červené a modré kusy parnej dlzky? 
  Poznámka: v HMM na hľadanie génov sme chceli mať dĺžku kódujúcej oblasti 
  deliteľnú 3, čo je podobná myšlienka

V ďalších príkladoch uvažujeme aj to, ktoré písmená sú v ktorom stave
povolené (pravdepodobnosť emisie \> 0) a ktoré sú zakázané

  - červená sekvencia dĺžky dva, ktorá začína na A
  - červená sekvencia dĺžky dva, ktorá je hocičo okrem AA
  - červená sekvencia dĺžky tri, ktorá je hocičo okrem stop kodónu
    (TAA, TAG, TGA)
  - toto sa dá rozšíriť na HMM, ktorý reprezentuje ORF, teda niečo, čo
    začína štart kodónom, potom niekoľko kodónov, ktoré nie sú
    stop kodónom a na koniec stop kodón

Ďalší biologický príklad HMM: topológia transmembránových proteínov.

- Máme proteín, ktorý prechádza (raz alebo viackrát) cez membránu bunky. Niektoré aminokyseliny sú
  vnútri bunky, niektoré sú vo vonkajšom prostredí a niektoré sú v
  membráne. Chceme ofarbiť aminokyseliny proteínu farbami: vonku,
  v membráne, vo vnútri.
- Nie každá sekvencia farieb je rozumná, napr. vonku - vo vnútri, alebo vonku -
  membrána, vonku
- Aké potrebujeme v HMM stavy, aby sme mohli generovať rozumné sekvencie farieb?
- HMM boli použité na predikciu topológie transmembránových proteínov, napr. v programe [TMHMM](https://www.sciencedirect.com/science/article/pii/S0022283600943158), neskôr rozšírený na hlboké neurónové siete v programe [DeepTMHMM](https://www.biorxiv.org/content/10.1101/2022.04.08.487609v1)

## E-hodnota (E-value) zarovnania

  - Príklady k tejto časti v prezentácii 
  - Máme dotaz dĺžky $m$, databázu dĺžky $n$, skóre najlepšieho lokálneho
    zarovnania $S$
  - E-hodnota je očakávaný počet zarovnaní so skóre aspoň $S$, ak dotaz aj
    databáza sú náhodné
  - Hračkársky príklad: dotaz dĺžky $m=10$, databáza dĺžky $n=300$, skóre $S=6$
  - Zoberme náš náhodný model s obsahom GC 50%
  - Máme vrece s guličkami označenými A,C,G,T, z každej 25%
  - Vytiahneme guličku, zapíšeme si písmeno, hodíme ju naspať, zamiešame
    a opakujeme s ďalším písmenom atď, až kým nevygenerujeme $m$ písmen pre
    dotaz a $n$ písmen pre databázu
  - Pre naše vygenerované sekvencie spočítame, koľkokrát sa dotaz
    vyskytuje v databáze
  - Celý experiment opakujeme veľa krát a spočítame priemerný počet
    výskytov, čo bude odhad E-hodnoty

Výpočet strednej hodnoty vzorcom namiesto simulácie (rýchlejsie)

  - zložitá [matematická teória](https://www.ncbi.nlm.nih.gov/BLAST/tutorial/Altschul-1.html)
  - E-hodnota sa približne dá odhadnúť vzorcom: $E = Kmn e^{-\lambda S}$
  - $n$ a $m$ su dĺžky porovnávaných sekvencií, $S$ je skóre, $K$ a $\lambda$ sú
    parametre, ktoré závisia od skórovacej schémy a od frekvencií
    výskytu jednotlivych báz v našom modeli náhodnej sekvencie.
  - Napríklad blastn pre skórovací systém zhoda 1, nezhoda -1, medzera -2
    používa $\lambda=0.800, K=0.0640$
  - $E = 0.0640 nm 0.45^S$.
  - Zdvojnásobenie dĺžky databázy alebo dĺžky dotazu zdvojnásobí E-hodnotu
  - Zníženie skóre o 1 tiež zhruba zdvojnásobí E-value (delenie 0.45,
    t.j. nasobenie 2.2)
      - Číslo, ktorým násobíme, závisí od $\lambda$, a teda od skórovacej
        schémy a frekvencií výskytu báz


