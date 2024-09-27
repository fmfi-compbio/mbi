---
title: "Cvičenie pre biológov: Úvod do pravdepodobnosti"
---



  - Myšlienkový experiment, v ktorom vystupuje náhoda, napr. hod
    ideálnou kockou/korunou
  - Výsledkom experimentu je nejaká hodnota (napr. číslo, alebo aj
    niekoľko čísel, reťazec)
  - Túto neznámu hodnotu budeme volať **náhodná premenná**
  - Zaujíma nás **pravdepodobnosť**, s akou náhodná premenná nadobúda
    jednotlivé možné hodnoty
  - T.j. ak experiment opakujeme veľa krát, ako často uvidíme nejaký
    výsledok

Príklad 1: hodíme idealizovanou kockou, premenná X bude hodnota, ktorú
dostaneme

  - Možné hodnoty 1,2,..,6, každá rovnako pravdepodobná
  - Píšeme napr. Pr(X=2)=1/6

Príklad 2: hodíme 2x kockou, náhodná premenná X bude súčet hodnôt, ktoré
dostaneme

  - Možné hodnoty: 2,3,...,12
  - Každá dvojica hodnôt (1,1), (1,2),...,(6,6) na kocke rovnako
    pravdepodobná, t.j. pravdepodobnosť 1/36
  - Súčet 5 môžeme dostať 1+4,2+3,3+2,4+1 - t.j. Pr(X=5) = 4/36
  - Súčet 11 môžeme dostať 5+6 alebo 6+5, t.j. Pr(X=11) = 2/36
  - **Rozdelenie pravdepodobnosti:** (tabuľka udávajúca pravdepodobnosť
    pre každú možnú hodnotu)


    hodnota i:   2     3     4     5     6     7     8     9     10    11    12
    Pr(X=i):    1/36  2/36  3/36  4/36  5/36  6/36  5/36  4/36  3/36  2/36  1/36

  - Overte, ze súčet pravdepodobností je 1

**Stredná hodnota E(X):**

  - priemer z možných hodnôt váhovaných ich pravdepodobnosťami
  - v našom príklade
    $E(X) = 2\cdot \frac{1}{36} + 3\cdot \frac{2}{36}+ 4\cdot \frac{3}{36}+ 5\cdot \frac{4}{36}+ 6\cdot \frac{5}{36}+ 7\cdot \frac{6}{36}+ 8\cdot \frac{5}{36}+ 9\cdot \frac{4}{36}+ 10\cdot \frac{3}{36}+ 11\cdot \frac{2}{36}+ 12\cdot \frac{1}{36}=7$
  - Ak by sme experiment opakovali veľa krát a zrátali priemer hodnôt X,
    ktoré nám vyšli, dostali by sme číslo blízke E(X)
  - Iný výpočet strednej hodnoty:
      - X=X1+X2, kde X1 je hodnota na prvej kocke a X2 je hodnota na
        druhej kocke
      - $E(X_1) = 1\cdot \frac{1}{6} + ... + 6\cdot \frac{1}{6}  = 3.5$,
        podobne aj E(X2) = 3.5
      - Platí, že E(X1+X2)=E(X1) + E(X2) a teda E(X) = 3.5 + 3.5 = 7
      - Pozor, pre súčin a iné funkcie takéto vzťahy platiť nemusia,
        napr. $E(X_1 \cdot X_2)$ nie je vždy $E(X_1) \cdot E(X_2)$

### Pravdepodobnostný model náhodnej sekvencie

  - Napríklad chceme modelovať náhodnú DNA sekvenciu dĺžky n s obsahom
    GC 40%
  - Máme vrece s guľôčkami označenými A,C,G,T, pričom guľôčok označených
    A je 30%, C 20%, G 20% a T 30%.
  - Vytiahneme guľôčku, zapíšeme si písmeno, hodíme ju naspäť, zamiešame
    a opakujeme s ďalším písmenom atď, až kým nevygenerujeme n písmen

  - Vytiahnime z mechu 2x guľôčku. Prvé písmeno, ktoré nám vyjde,
    označme X1 a druhé X2
  - Pr(X1=A) = 0.3, Pr(X2=C)=0.2
  - Pr(X1=A a X2=C) = Pr(X1=A)\*Pr(X2=C) = 0.3\*0.2 = 0.06
      - T.j. šanca, že dostaneme sekvenciu AC po dvoch ťahoch je 6%
      - Ak rátame pravdepodobnosť, že sa dve nezávislé udalosti stanú
        súčasne, ich pravdepodobnosti násobíme. V tomto prípade to, či
        X1=A je nezávislé od toho, či X2=C
  - Pr(X1 je A alebo C) = Pr(X1=A)+Pr(X1=C) = 0.3+0.2 = 0.5
      - Pravdepodobnosť, že prvé písmeno bude A alebo C je 50%
      - Pravdepodobnosti navzájom sa vylučujúcich udalostí (X1=A a X1=C)
        sa môžu sčítať, čím dostaneme pravdepodobnosť, že aspoň jedna z
        nich nastane
  - Pr(v sekvencii je aspoň jedno A) = Pr(X1=A alebo X2=A) nemôžeme
    počítať ako Pr(X1=A)+Pr(X2=A), lebo sa navzájom nevylučujú a
    prípad, že X1=A a X2=A by sme započítali dvakrát
  - Správne je $\Pr(X_1=A \mathrm{ alebo } X_2=A)
    = \Pr(X_1=A) + \Pr(X_1 \ne A \mathrm{ a } X_2=A)
    = \Pr(X_1=A) + Pr(X_1 \ne A) \cdot Pr(X_2=A)
    = 0.3+0.7\cdot 0.3 = 0.51$
  - Pr(X1=X2) = Pr(X1=X2=A) + Pr(X1=X2=C) + Pr(X1=X2=G) + Pr(X1=X2=T) =
    0.3\*0.3+0.2\*0.2+0.2\*0.2+0.3\*0.3 = 0.26.
  - Ak u označíme pravdepodobnosť u =
    Pr(X1=A)=Pr(X1=T)=Pr(X2=A)=Pr(X2=T) a
    v=Pr(X1=C)=Pr(X1=G)=Pr(X2=C)=Pr(X2=G), aký bude vzorec pre
    Pr(X1=X2)?

**Príklad použitia modelu:** Máme krátky primer AACAT. Koľko bude mať v
priemere výskytov v sekvencii dĺžky 1000 v našom modeli?

  - Pravdepodobnosť, ze AACAT je v náhodnej sekvencii hneď na začiatku
    je Pr(X1=A a X2=A a X3=C a X4=A a X5=A) = 0.3\*0.3\*0.2\*0.3\*0.3 =
    0.00162
  - Rovnaká pravdepodobnosť aj na pozícii 2,3,...996
  - Nech *V* je počet výskytov v celej sekvencii (náhodná premenná s
    možnými hodnotami 0,1,...,996, aj keď napr. 996 to určite nemôže
    byť)
  - Ideálne by sme chceli spočítať celú tabuľku pravdepodobností pre V,
    ale uspokojíme sa aj so strednou hodnotou E(V)
  - Nech Vi je počet výskytov na pozícii i (čo je vždy 0 alebo 1)
  - $V = V_1+V_2+\dots+V_{996} = \sum_{i=1}^{996} V_i$
  - $E(V) = E(V_1)+E(V_2)+\dots+E(V_{996}) = 996 E(V_1)$
  - $E(V_1) = 0\cdot \Pr(V_1=0)+1\cdot \Pr(V_1=1) = \Pr(V_1=1) = 0.00162$
  - $E(V) = 996\cdot 0.00162 = 1.61352$
  - Takže primer AACAT sa v priemere bude v náhodnej sekvencii dĺžky
    1000 s 40% obsahom GC vyskytovať v priemere cca 1,6 krát
  - Primery bývajú dlhšie, takže šanca náhodných výskytov je oveľa
    menšia, čo je to, čo vačšinou chceme (chceme primer cieliť na
    konkrétnu pozíciu, nie na veľa náhodných zhôd)

## Použitie pravdepodobnosti na analýzu potrebného pokrytia pri sekvenovaní

Pozri [cvičenia pre informatikov](./ci-prob.md)

