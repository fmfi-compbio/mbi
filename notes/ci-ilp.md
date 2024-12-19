---
title: "Cvičenia pre informatikov: Celočíselné lineárne programovanie"
---

* TOC
{:toc}


### Praktické programy na NP ťažké problémy

  - Obcas chceme najst optimalne riesenie nejakeho NP-tazkeho problemu
  - Jedna moznost je previest na iny NP tazky problem, pre ktory
    existuju pomerne dobre prakticke programy, napriklad **integer
    linear programming (ILP)**
  - najdu optimalne riesenie, mnohe instancie zrataju v rozumnom case,
    ale mozu bezat aj velmi dlho
  - [CPLEX](http://www-01.ibm.com/software/integration/optimization/cplex-optimizer/)
    a [Gurobi](http://www.gurobi.com/html/academic.html) komercne
    baliky na ILP, akademicka licencia zadarmo
  - nekomerncne programy, napriklad [SCIP](http://scip.zib.de/),
    [SYMPHONY](https://projects.coin-or.org/SYMPHONY) v projekte COIN-OR,
    [GLPK](https://www.gnu.org/software/glpk/)    
  - [Minisat](http://minisat.se/) open source SAT solver, tiež
    Lingeling, glucose, CryptoMiniSat, painless
  - [Concorde](http://www.tsp.gatech.edu/concorde.html) - TSP solver 
    riesi problem obchodneho cestujuceho so symetrickymi vzdialenostami,
    zadarmo na akademicke ucely
      - Pre zaujimavost: [TSP art](http://www.oberlin.edu/math/faculty/bosch/tspart-page.html)

### ILP

**Lineárny program**

  - Mame reálne premenné $x_1\dots x_n$, minimalizujeme (alebo maximalizujeme) nejaku ich linearnu
    kombinaciu $\sum_i a_i x_i$ kde $a_i$ su dane vahy.
  - Mame tiez niekolko podmienok v tvare linearnych rovnosti alebo
    nerovnosti, napr. $\sum_i b_i x_i \le c$
  - Hladame teda hodnoty premennych, ktore minimalizuju cielovu sumu,
    ale pre ktore platia vsetky podmienky
  - Da sa riesit v polynomialnom case

**Integer linear program**

  - Program, v ktorom vsetky/vybrane premenne musia mat celociselne
    hodnoty, alebo dokonca povolime iba hodnoty 0 a 1.
  - NP uplny problem

### Ako zapísať (NP-ťažké) problémy ako ILP

Knapsack

  - Problem: mame dane predmety s hmotnostami $w_1\dots w_n$ a cenami
    $c_1\dots c_n$. Ktore z nich vybrat, aby celkova hmotnost bola najviac T
    a cena bola co najvyssia?
  - Pouzijeme binarne premenne $x_1\dots x_n$, kde $x_i = 1$ prave vtedy ked
    sme zobrali i-ty predmet.
  - Chceme maximalizovat $\sum_i c_i x_i$
  - za podmienky ze $\sum_i w_i x_i \le T$

Set cover:

  - Mame n mnozin $S_1\dots S_n$ nad mnozinou $\\{1\dots m\\}$. Chceme vybrat co
    najmensi pocet zo vstupnych mnozin tak, aby ich zjednotenie bola
    cela mnozina {1..m}
  - Binarne premenne $x_i=1$ ak vyberieme i-tu mnozinu
  - Chceme minimalizovat $\sum_{i=1}^n x_i$
  - za podmienky, ze pre kazde j z {1..m} plati
    $\sum_{i:j\in S_i} x_i \ge 1$

### Zarovnanie sekvencií RNA so štruktúrou

Máme dané dve sekvencie RNA $X_1\dots X_n$ a $Y_1\dots Y_m$
a pre každú z nich máme danú množinu párov
báz (pozícií v rámci sekvencie), ktoré by mohli byť prítomné v
sekundárnej štruktúre (množiny párov označíme $P_X$ a $P_Y$).

  - Množina párov môže byť konkrétna známa sekundárna štruktúra danej
    sekvencie alebo väčšia množina párov, ktoré by sa v štruktúre mohli
    vyskytovať, napríklad dvojice, ktoré majú pomerne veľkú
    pravdepodobnosť byť spárené v SCFG modeli alebo dokonca všetky
    dvojice komplementárnych báz. 
 
Cieľom je nájsť optimálne zarovnanie týchto dvoch sekvencií, v ktorom
použijeme obvyklé skórovanie zhôd, nezhôd a medzier, ale navyše
pridáme bonus za zhody v štruktúre. Dva potenciálne páry, každý z
jednej sekvencie, považujeme za zarovnané, ak sú navzájom zarovnané
bázy na ich obidvoch koncoch. Do skórovania vyberieme podmnožinu
zarovnaných párov tak, aby každá báza bola v najviac jednom páre a
každému takému páru priradíme nejaké kladné skóre.

Touto formulaciou mozeme riesit niekolko problemov
- zarovnat dve RNA so znamymi strukturami (mozu obsahovat pseudouzly), pricom davame bonus ak sa obidva konce paru zarovnaju s parom v druhej sekvencii (bez pseudozulov sa da v polynomialnom case)
- najst spolocnu strukturu pre dve RNA sekvencie, ktorych RNA struktura nie je znama
- zarovnat RNA sekvenciu k inej RNA sekvencii so znamou strukturou


Príklad: ak je zhoda 1, nezhoda a medzera -1, pár >10/3, vyhráva prvé
zarovnanie, ak pár <10/3, vyhráva druhé (z týchto dvoch)
```
 [ [[    ] ]]
-GCGGAUAACCCC
 |   |      |  3 zhody, 5 nezhôd, 4 medzery, 3 páry
GG-AUA-CCA-UC
 [ [[    ] ]]

( ((    ) ) )
GCGGAUAACCC-C
  ||||| ||  |  8 zhôd, 1 nezhoda, 3 medzery, 0 párov
--GGAUA-CCAUC
   (((    )))
```
X=GCGGAUAACCC, Y=GGAUACCAUC,
$P_X$={(1,12),(3,11),(4,9)}, $P_Y$={(2,10),(3,9),(4,8)}


Konštanty
- $a_{i,j}$ cena zarovnania báz $X_i$ a $Y_j$ (zhoda alebo nezhoda)
- $g$ cena medzery v zarovnaní
- $p$ bonus za zarovnaný pár

Premenné (všetky sú binárne):
- $x_{i,j}$ ci su $X_i$ a $Y_j$ zarovnane
- $z_{1,i}$ ci $X_i$ zostala nezarovnana
- $z_{2,j}$ ci $Y_j$ zostala nezarovnana
- $y_{i,j,k,l}$ ci su i a j zarovnane, k a l zarovnane a i a k zvolene ako par, j a l zvolene ako par (iba pre hodnoty kde $(i,k)\in P_X$ a $(j,l)\in P_Y$ a $i<k$, $j<l$)

Maximalizujeme
- $\sum_{i,j} a_{i,j} x_{i,j} + g\cdot(\sum_i z_{1,i} + \sum_j z_{2,j}) + p\cdot \sum_{i,j,k,l} y_{i,j,k,l}$

Podmienky
- $z_{1,i} + \sum_j x_{i,j}=1$ pre kazde $i$ (kazda baza z X je zarovnana prave raz alebo nezarovnana)
- $x_{i,j}+x_{k,l}\le 1$ pre kazde i,j,k,l take ze $i<k$ ale $j>l$ (kriziace sa pary)
- $z_{2,j} + \sum_i x_{i,j}=1$ pre kazde $j$ (to iste pre Y)
- $\sum_{k,l} y_{i,j,k,l}\le x_{i,j}$ pre kazde i,j (ak i,j nie je zarovnane, nebudu sa pocitat ziadne dvojice pary obsahujuce i a j a ak je zarovnane, tak najviac jeden taky par)
- $\sum_{i,j} y_{i,j,k,l}\le x_{i,j}$ pre kazde k,l (podbne pre druhu stranu)

Aka je velkost programu vzhladom na m a n (pocet premennych, nerovnosti, vsetkych nenulovych clenov)? Ktore casti sa zmensia ak $P_X$ a $P_Y$ su relativne male?


Zdroj:

Bauer, Markus, Gunnar W. Klau, and Knut Reinert. [Accurate multiple sequence-structure alignment of RNA sequences using combinatorial optimization.](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-8-271) BMC Bioinformatics 8 (2007): 1-18.

Bauer, Markus, Gunnar W. Klau, and Knut Reinert. [An exact mathematical programming approach to multiple RNA sequence-structure alignment.](https://www.erudit.org/en/journals/aor/2008-v3-n2-aor3_2/aor3_2art03.pdf) Algorithmic Operations Research 3.2 (2008): 130-146.



### Protein threading

Podobný problém, uvedené pre zaujímavosť, nerobili sme 

  - Ciel: protein A ma znamu sekvenciu aj strukturu, protein B iba
    sekvenciu. Chceme zarovnat proteiny A a B, pricom budeme brat do
    uvahy znamu strukturu, t.j. ak su dve amino kyseliny blizko v A tak
    ich ekvivalenty v B by mali byt "kompatibilne".
  - Tento problem chceme riesit tak, ze v strukture A urcime nejake
    jadra, ktore by v evolucii mali zostat zachovane bez inzercii a
    delecii a v rovnakom poradi. Tieto jadra su oddelene sluckami,
    ktorych dlzka sa moze lubovolne menit a ktorych zarovnania nebudeme
    skorovat.
  - Formulacia problemu: Mame danu sekvenciu B=b1..bn, dlzky m jadier
    c\_1...c\_m a skorovacie tabulky E\_ij, ktora vyjadruje, ako dobre
    bj..b\_{j+c\_i-1} sedi do sekvencie jadra i a E\_ijkl ktora
    vyjadruje, ako dobre by jadra i a k interagovali, keby mali
    sekvencie zacinajuce v B na poziciach j a l. Uloha je zvolit polohy
    jadier x\_1\<x\_2\<...\<x\_m tak, aby sa ziadne dve jadra
    neprekryvali a aby sme dosiahli najvyssie skore.
  - Poznamka: nevraveli sme, ako konkretne zvolit jadra a skorovacie
    tabulky, co je modelovaci, nie algoritmicky problem (mozeme skusit
    napr. nejake pravdepodobnostne modely)

#### Protein threading ako ILP

  - Premenne v programe:
      - x\_ij=1 ak je zaciatok i-teho jadra zarovnane s b\_j
      - y\_ijkl=1 ak je zaciatok i-teho jadra na b\_j a zaciatok k-teho
        na b\_l (i\<k, j\<l)
  - Chceme maximalizovat $\sum E_{ij} x_{ij} + \sum E_{ijkl} y_{ijkl}$
  - Podmienky:
      - $\sum_j x_{ij}=1\,$ pre kazde i
      - $x_{il}+x_{i+1,k}\le 1$ pre vsetky i,k,l, kde k\<l+c\_i
      - $y_{ijkl}\le x_{ij}$ pre vsetky i,j,k,l, kde i\<k, j\<l
      - $y_{ijkl}\le x_{kl}$ pre vsetky i,j,k,l, kde i\<k, j\<l
      - $y_{ijkl}\ge x_{ij}+x_{kl}-1$ pre vsetky i,j,k,l, kde i\<k,
        j\<l

Na zamyslenie:

  - Aka bude velkost programu ako funkcia n a m?
  - Co ak nie vsetky jadra navzajom interaguju? Mozeme na velkosti
    programu usetrit?
  - Preco asi vobec autori zaviedli jadra a ako by sme zmenili program,
    ak by sme chceli uvazovat kazdu aminokyselinu zvlast?

Zdroj:

  - Jinbo Xu, Ming Li, Dongsup Kim, and Ying Xu. [RAPTOR: optimal
    protein threading by linear programming.](http://ttic.uchicago.edu/~jinbo/SelectedPubs/RAPTOR.pdf) Journal of bioinformatics
    and computational biology 1, no. 01 (2003): 95-117.
