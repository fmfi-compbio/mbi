---
title: "Cvičenia pre informatikov: Úvod do pravdepodobnosti"
---

## Čo je to pravdepodobnosť?

  - Myšlienkový experiment, v ktorom vystupuje náhoda, napr. hod ideálnou kockou/mincou
  - Výsledkom experimentu je nejaká hodnota (napr. číslo alebo aj niekoľko čísel, reťazec)
  - Túto neznámu hodnotu budeme volať **náhodná premenná**
  - Zaujíma nás **pravdepodobnosť**, s akou náhodná premenná nadobúda
    jednotlivé možné hodnoty
  - T.j. ak experiment opakujeme veľa krát, ako často uvidíme nejaký
    výsledok

## Príklad 1: Hod kockou

Hodíme idealizovanou kockou, premenná X bude hodnota, ktorú
dostaneme.

  - Možné hodnoty 1,2,..,6, každá rovnako pravdepodobná
  - Pišeme napr. Pr(X=2)=1/6

## Príklad 2: Súčet na dvoch kockách

Hodíme 2x kockou, náhodná premenná X bude súčet hodnôt, ktoré dostaneme.

  - Možné hodnoty: 2,3,...,12
  - Každá dvojica hodnôt (1,1), (1,2),...,(6,6) na kocke rovnako
    pravdepodobná, t.j. pravdepodobnosť 1/36
  - Súčet 5 môžeme dostať 1+4,2+3,3+2,4+1 - t.j. Pr(X=5) = 4/36
  - Súčet 11 môžeme dostať 5+6 alebo 6+5, t.j. Pr(X=11) = 2/36
  - **Rozdelenie pravdepodobnosti:** (tabuľka udávajúca pravdepodobnosť
    pre každú možnú hodnotu)

```
hodnota k:   2     3     4     5     6     7     8     9     10    11    12
Pr(X=k):    1/36  2/36  3/36  4/36  5/36  6/36  5/36  4/36  3/36  2/36  1/36
```

  - Overte, ze súčet pravdepodobností je 1

## Stredná hodnota E(X)

  - priemer z možných hodnôt váhovaných ich pravdepodobnosťami $E(X)=\sum_k \Pr(X=k)\vdot k$
  - v našom príklade
    $E(X) = 2\cdot \frac{1}{36} + 3\cdot \frac{2}{36}+ 4\cdot \frac{3}{36}+ 5\cdot \frac{4}{36}+ 6\cdot \frac{5}{36}+ 7\cdot \frac{6}{36}+ 8\cdot \frac{5}{36}+ 9\cdot \frac{4}{36}+ 10\cdot \frac{3}{36}+ 11\cdot \frac{2}{36}+ 12\cdot \frac{1}{36}=7$
  - Ak by sme experiment opakovali veľa krát a zrátali priemer hodnôt X,
    ktoré nám vyšli, dostali by sme číslo blízke E(X)
  - Iný výpočet strednej hodnoty:
      - X=X1+X2, kde X1 je hodnota na prvej kocke a X2 je hodnota na
        druhej kocke
      - $E(X_1) = 1\cdot \frac{1}{6} + ... + 6\cdot \frac{1}{6}  = 3.5$,
        podobne aj $E(X_2) = 3.5$
      - Platí, že E(X1+X2)=E(X1) + E(X2) a teda E(X) = 3.5 + 3.5 = 7
      - Pozor, pre súčin a iné funkcie takéto vzťahy platiť nemusia,
        napr. $E(X_1 \cdot X_2)$ nie je vždy $E(X_1) \cdot E(X_2)$

## Binomické rozdelenie

- Jeden hod mincou: padna hlava (head, H) s pravdepodobnosťou p a znak (tail, T) 1-p
- N krat hodime mincou, ako X oznacime pocet hlav, ktore padnu
- Priklad: majme mincu, ktora ma hlavu s pravdepodobnosťou p = 1/4 a hodime ju N=3 krat.

```
    HHH 1/64
    HHT 3/64
    HTH 3/64
    HTT 9/64
    THH 3/64
    THT 9/64
    TTH 9/64
    TTT 27/64
```

- $\Pr(X=3) = 1/64$, $\Pr(X=2)=9/64$, $\Pr(X=1)=27/64$,
  $\Pr(X\=0)=27/64$
- taketo rozdelenie pravdepodobnosti sa vola binomicke
- $Pr(X = k) = {N \choose k} p^k (1-p)^(N-k)$, kde
  ${N \choose k} = \frac{N!}{k!(N-k)!}$ a $n! = 1\cdot 2\cdot ...\cdot n$
- napr pre priklad s troma hodmi kockou $\Pr(X=2) = 3!/(2!\cdot 1!)
  \cdot (1/4)^2 \cdot (3/4)^1 = 9/64$
- Zle sa pocita pre velke N, preto sa pre velke N a male p niekedy pouziva aproximacia
  Poissonovym rozdelenim s parametrom $\lambda = Np$, ktore ma
  $\Pr(X_j = k)=e^{-\lambda}\lambda^k / k!$


## Počítanie pokrytia genómov

  - Náš problém: spočítanie pokrytia
      - G = dĺžka genómu, napr. 1 000 000 (predpokladajme, že je
        cirkulárny)
      - N = počet čítaní (readov), napr. 10 000
      - L = dlzka čítania, napr. 1000
      - Celková dĺžka čítaní NL, pokrytie (coverage) NL/G, v nasom
        pripade 10x
      - V priemere každá báza pokrytá 10x
      - Niektoré sú ale pokryté viackrát, iné menej.
      - Zaujímajú nás otázky typu: koľko báz očakávame, že bude
        pokrytých menej ako 3x?
      - Dôležité pri plánovaní experimentov (aké veľké pokrytie
        potrebujem na dosiahnutie určitej kvality)

  - Pokrytie genómu: predpokladáme, že každé čítanie začína na náhodnej
    pozícii zo všetkých možných G
  - Takže ak premenná $Y_i$ bude začiatok i-tého čítania, jej rozdelenie
    bude rovnomerné
      - $\Pr(Y_i=1) = \Pr(Y_i=2) = ... = Pr(Y_i=G) = 1/G$

  - Aká je pravdepodobnosť že nejaké konkrétne i-te čítanie pokrýva
    konkrétnu pozíciu j?
      - $\Pr(Y_i\ge j-L+1 \wedge Y_i \le j) = \Pr(Y_i=j-L+1)+...+\Pr(Y_i=j) =
        L/G$, označme túto hodnotu p, v našom príklade p=0.001 (1
        promile)


  - Uvazujme premennu $X_j$, ktora udava pocet čítaní pokryvajucich
    poziciu j
      - mozne hodnoty 0..N
      - i-te čítanie pretina poziciu j s pravdepodobnostou p=L/G
      - to iste ako keby sme N krat hodili mincou, na ktorej spadne
        hlava s pravd. p a znak 1-p a oznacili ako $X_j$ pocet hlav
      - ide teda o binomické rozdelenie
      - vieme spocitat rozdelenie pravdepodobnosti
        a tiez napr. Pr(X\_i\<3) = Pr(X\_i=0)+Pr(X\_i=1)+Pr(X\_i=2) =
        0.000045+0.00045+0.0023=0.0028
  - Stredna hodnota poctu baz v celom genome s pokrytim k je
    $G\cdot \Pr(X_i=k)$
      - V priemere teda ocakavame 45 baz nepokrytych, 2800 pokrytých
        menej ako 3 krát a pod.
      - Takyto graf, odhad, vieme lahko spravit pre rozne pocty čítaní a
        tak naplanovat, kolko čítaní potrebujeme

Chceme tiež odhadnúť **počet kontigov** (podľa článku E.S. Lander and
M.S. Waterman. ["Genomic mapping by fingerprinting random clones: a
mathematical analysis."](http://www.cs.cmu.edu/~epxing/Class/10810/readings/lander_waterman.pdf) Genomics 2.3 (1988): 231-239)

  - Ak niekoľko báz vôbec nie je pokrytých čítaniami, preruší sa kontig
  - Vieme, koľko báz je v priemere nepokrytých, ale niektoré môžu byť
    vedľa seba
  - Nový kontig vznikne aj ak sa susedné čítania málo prekrývajú
  - Predpokladajme, že na spojenie dvoch čítaní potrebujeme prekryv
    aspoň T=50
  - Nech p je pravdepodobnosť, ze dané čítanie i bude posledné v kontigu
  - Aby sa to stalo, žiadne čítanie j\!=i nesmie začínať v prvých L-T
    bázach kontigu i
  - Každé čítanie tam začína s pravdepodobnosťou q=(L-T)/G
  - Ak X je počet čítaní, ktoré zacinaju v tomto useku, tak p = Pr(X=0)
    = (1-q)^(N-1) podla binomickeho rozdelenia
  - v priemere ich tam zacne E(X) = (N-1)(L-T)/G co je zhruba N(L-T)/G
  - Jednoduchší vzorec pre p dostaneme ak binomické rozdelenie premennej
    X aproximujeme Poissonovým s parametrom $\lambda=N(L-T)/G$ (t.j.
    aby mali rovnakú strednú hodnotu)
  - V Poissonovom rozdelení p = Pr(X=0) = exp(-lambda) = exp(-N(L-T)/G)
  - Presnosť aproximácie: pre parametre N,L,G,T uvedené vyššie dostaneme
    z binomického rozdelenia p=7.459e-5, z Poissonovho 7.485e-5
  - Pre N čítaní dostaneme priemerný počet kontigov N\*p =
    N\*exp(-N(L-T)/G)
  - NL/G je pokrytie, N(L-T)/G je pokrytie, ak by sme dĺžku každého
    čítania skrátili o dĺžku prekryvu
  - Pre T=50 dostaneme priemerný počet koncov kontigov 0.75 (ak
    pokryjeme celý kruh, máme nula koncov, preto je hodnota menšia ako
    1). Ak znížime N na 5000 (5x pokrytie) dostaneme 43 kontigov


  - Môže sa zdať zvláštne, ze pri priemernom pocte nepokrytych baz 45
    mame pocet koncov v priemere menej ako jedna. Situacia je vsak taka,
    ze pri opakovaniach tohto experimentu casto dostavame jeden suvisly
    kontig, ale ak je uz aspon jeden koniec kontigu, byva tam pomerne
    velka medzera. Tu je napriklad 50 opakovani expertimentu s T=0,
    priemerny pocet koncov je 0.55, priemerny pocet nepokrytych baz je
    49.


``` 
nepokr: 0 koncov: 0     nepokr: 0 koncov: 0     nepokr: 0 koncov: 0      
nepokr: 274 koncov: 2   nepokr: 282 koncov: 1   nepokr: 0 koncov: 0      
nepokr: 0 koncov: 0     nepokr: 0 koncov: 0     nepokr: 8 koncov: 1      
nepokr: 0 koncov: 0     nepokr: 12 koncov: 1    nepokr: 0 koncov: 0      
nepokr: 122 koncov: 1   nepokr: 135 koncov: 1   nepokr: 111 koncov: 1    
nepokr: 13 koncov: 1    nepokr: 1 koncov: 1     nepokr: 56 koncov: 1     
nepokr: 265 koncov: 1   nepokr: 0 koncov: 0     nepokr: 10 koncov: 1     
nepokr: 0 koncov: 0     nepokr: 0 koncov: 0     nepokr: 130 koncov: 1    
nepokr: 217 koncov: 1   nepokr: 3 koncov: 1     nepokr: 0 koncov: 0      
nepokr: 0 koncov: 0     nepokr: 0 koncov: 0     nepokr: 86 koncov: 1     
nepokr: 139 koncov: 2   nepokr: 0 koncov: 0     nepokr: 0 koncov: 0      
nepokr: 76 koncov: 1    nepokr: 221 koncov: 1   nepokr: 26 koncov: 1     
nepokr: 0 koncov: 0     nepokr: 1 koncov: 1     nepokr: 0 koncov: 0      
nepokr: 0 koncov: 0     nepokr: 0 koncov: 0     nepokr: 0 koncov: 0      
nepokr: 0 koncov: 0     nepokr: 0 koncov: 0     nepokr: 12 koncov: 1     
nepokr: 103 koncov: 2   nepokr: 0 koncov: 0     nepokr: 71 koncov: 1     
nepokr: 69 koncov: 1    nepokr: 0 koncov: 0    
```

  - Tento jednoduchy model nepokryva vsetky faktory:
      - čítania nemaju rovnaku dlzku
      - Problemy v zostavovani kvoli chybam, opakovaniam a pod.
      - čítania nie su rozlozene rovnomerne (cloning bias a pod.)
      - Vplyv koncov chromozomov pri linearnych chromozomoch
      - Uzitocny ako hruby odhad
      - Na spresnenie mozeme skusat spravit zlozitejsie modely, alebo
        simulovat data


## Zhrnutie

  - Pravdepobnostny model: myslienkovy experiment, v ktorom vystupuje
    nahoda, napr. hod idealizovanou kockou
  - Vysledok je hodnota, ktoru budeme volat nahodna premenna
  - Tabulka, ktora pre kazdu moznu hodnotu nahodnej premennej urci jej
    pravdepodobnost, sa vola rozdelenie pravdepodobnosti, sucet hodnot v
    tabulke je 1
  - Znacenie typu Pr(X=7)=0.1


  - Priklad: mame genom dlzky G=1mil., nahodne umiestnime N=10000 čítaní
    dlzky L=1000
  - Nahodna premenna X\_i je pocet čítaní pokryvajucich urcitu poziciu i
  - Podobne, ako keby sme N krat hodili kocku, ktora ma cca 1 promile
    sancu padnu ako hlava a 99.9% ako znak a pytame sa, kolko krat padne
    znak (1 promile sme dostali po zaukruhleni z L/(G-L+1))
  - Rozdelenie pravdepobnosti sa v tomto pripade vola binomicke a
    existuje vzorec, ako ho spocitat
  - Takyto model nam moze pomoct urcit, kolko čítaní potrebujeme
    osekvenovat, aby napr. aspon 95% pozicii bolo pokrytych aspon 4
    čítaniami
