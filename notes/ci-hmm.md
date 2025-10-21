---
title: "Cvičenia pre informatikov: Algoritmy pre HMM"
---

* TOC
{:toc}

## HMM opakovanie

Parametre HMM:

  - $a_{u,v}$: prechodová pravdepodobnosť zo stavu $u$ do stavu
    $v$
  - $e_{u,x}$: pravdepodobnosť emisie $x$ v stave $u$
  - $\pi_{u}$: pravdepodobnosť, že začneme v stave $u$


  - Sekvencia $S = S_1 S_2 \dots  S_n$
  - Anotácia $A = A_1 A_2 \dots A_n$

$Pr(S, A) = \pi_{A_1} e_{A_1,S_1} \prod_{i=2}^n a_{A_{i-1, A_i}} e_{A_i, S_i}$

Trénovanie

  -   
    Proces, pri ktorom sa snažíme čo najlepšie odhadnúť pravdepodobnosti
    $a_{u,v}$ a $e_{u,x}$ v modeli podľa trénovacích dát

Usudzovanie (inferencia)

  -   
    Proces, pri ktorom sa snažíme pre sekvenciu $S$ nájsť anotáciu
    $A$, ktorá sekvenciu $S$ emituje s veľkou pravdepodobnosťou.

## Inferencia pomocou najpravdepodobnejšej cesty, Viterbiho algoritmus

Hľadáme najpravdepodobnejšiu postupnosť stavov $A$, teda
$\arg\max_A \Pr(A, S)$. Úlohu budeme riešiť dynamickým programovaním.

  - Podproblém $V[i,u]$: Pravdepodobnosť najpravdepodobnejšej cesty
    končiacej po $i$ krokoch v stave $u$, pričom vygeneruje
    $S_1 S_2 \dots S_i$.
  - Rekurencia:
      - $V[1,u] = \pi_u e_{u,S_1}$ (\*)
      - $V[i,u] = \max_w V[i-1, w] a_{w,u} e_{u,S_i}$ (\*\*)

Algoritmus:

1.  Nainicializuj $V[1,*]$ podľa (\*)
2.  for i=2 to n=dĺžka reťazca
      -  for u=1 to m=počet stavov
          - vypočítaj $V[i,u]$ pomocou (\*\*)
3.  Maximálne $V[n,j]$ je pravdepodobnosť najpravdepodobnejšej cesty

Aby sme vypísali anotáciu, pamätáme si pre každé $V[i,u]$ stav $w$,
ktorý viedol k maximálnej hodnote vo vzorci (\*\*).

Zložitosť: $O(nm^2)$

Poznámka: pre dlhé sekvencie budú čísla $V[i,u]$ veľmi malé a môže
dôjsť k podtečeniu. V praxi teda používame zlogaritmované hodnoty,
namiesto násobenia súčet.

## Inferencia - dopredný algoritmus

Aká je celková pravdepodobnosť, že vygenerujeme sekvenciu $S$, t.j.
$\sum_A Pr(A,S).$ Podobný algoritmus ako Viterbiho.

Podproblém $F[i,u]$: pravdepodobnosť, že po $i$ krokoch vygenerujeme
$S_1, S_2, \dots S_i$ a dostaneme sa do stavu $u$.

$F[i,u] = \Pr(A_i=u\wedge S_1, S_2, \dots, S_i) = \sum_{A_1, A_2, \dots, A_i=u} \Pr (A_1, A_2, ..., A_i \wedge S_1, S_2, ..., S_i)$

$F[1,u] = \pi_u e_{u,S_1}$

$F[i,u] = \sum_v F[i-1,v] a_{v,u} e_{u,S_i}$

Celková pravdepodobnosť $\sum_u F[n,u]$

## Inferencia - posterior decoding

Aposteriórna pravdepodobnosť stavu u na pozícii i:
$Pr(A_i=u|S_1\dots S_n)$

Pre každý index i chceme nájsť stav u s najväčšiou aposteriórnou
pravdepodobnosťou, dostaneme tak inú možnú anotáciu.

Spustíme dopredný algoritmus a jeho symetrickú verziu, spätný
algoritmus, ktorý počíta hodnoty
$B[i,u]=\Pr(S_{i+1}\dots S_n | A_i=u)$

Aposteriórna pravdepodobnosť stavu u na pozícii i:
$\Pr(A_i=u|S_1\dots S_n) = F[i,u] B[i,u] / \sum_u F[n,u].$

Posterior decoding uvažuje všetky anotácie, nielen jednu s najvyššou
pravdepodobnosťou. Môže však vypísať anotáciu, ktorá má sama o sebe
nulovú pravdepodobnosť (napr. počet kódujúcich báz v géne nie je
deliteľný 3).

## Trénovanie HMM

  - Stavový priestor + povolené prechody väčšinou ručne
  - Parametre (pravdepodobnosti prechodu, emisie a počiatočné)
    automaticky z trénovacích sekvencií
      - Ak máme anotované trénovacie sekvencie, jednoducho počítame
        frekvencie
      - Ak máme iba neanotované sekvencie, snažíme sa maximalizovať
        vierohodnosť trénovacích dát v modeli. Používajú sa heuristické
        iteratívne algoritmy, napr. Baum-Welchov, ktorý je verziou
        všeobecnejšieho algoritmu EM (expectation maximization).
  - Čím zložitejší model a viac parametrov máme, tým potrebujeme viac
    trénovacích dát, aby nedošlo k preučeniu, t.j. k situácii, keď
    model dobre zodpovedá nejakým zvláštnostiam trénovacích dát, nie
    však ďalším dátam.
  - Presnosť modelu testujeme na zvláštnych testovacích dátach, ktoré
    sme nepoužili na trénovanie.

## Tvorba stavového priestoru modelu

  - Promótor + niekoľko prokaryotických génov
  - Repeaty v intrónoch: multiple path problem
  - Intrón má dĺžku aspoň 10

## Zovšeobecnené HMM

Nebrali sme, uvedene pre zaujimavost

  - Predstavme si HMM s dvoma stavmi, napr. gén / negén, pričom každý
    stav má prechod do seba aj do druhého stavu
  - Úloha: Nech p je pravdepodobnosť, že zostaneme v tom istom stave,
    (1-p), že prejdeme do druhého stavu. Aká je pravdepodobnosť, že v
    stave zostaneme presne k krokov (k\>=1)?
      - Riešenie: $p^k (1-p)$
      - Toto rozdelenie sa nazýva geometrické a pravdepodbnosť
        exponenciálne rýchlo klesá s rastúcim k
  - Keď sa pozrieme na histogram reálny dĺžkov génov / exónov a iných
    oblastí, väčšinou sa nepodobá na geometrické rozdelnie, môže
    pripomínať napr. normálne rozdelenie s určitou priemernou dĺžkou a
    rozptylom okolo
      - Jednoduché HMM teda dobre nemodeluje tento fenomén
  - Zovšeobecnené HMM (semi-Markov) pracuje tak, že v stave má ľubovoľné
    rozdelenie pravdepodobnosti dĺžok. Model vôjde do stavu, vygeneruje
    dĺžku k z tohto rozdelenia, potom vygeneruje k znakov z príslušnej
    emisnej tabuľky a na záver sa rozhodne, ktorým prechodom opustí stav
  - Úloha: ako spočítame pravdepodobnosť konkrétnej sekvencie a
    konkrétnej postupnosti stavov aj s dĺžkami? (zaveďme si aj nejaké
    vhodné označenie)
  - Úloha: ako treba upraviť Viterbiho algoritmus pre tento model? Aká
    bude jeho zložitosť?
      - Zložitosť bude kvadraticky rásť od dĺžky sekvencie, predtým
        rástla lineárne
  - Predstavme si teraz, že rozdelenie dĺžok má hornú hranicu D takú, že
    všetky dĺžky väčšie ako D majú nulovú pravdepodobnosť.
      - Úloha: ako sa toto obmedzenie prejaví v zložitosti Viterbiho
        algoritmu?
      - Uloha: navrhnite, ako modelovať zovšeobecnený HMM s rozdelením
        dĺžok ohraničeným D pomocou normálneho stavu, kde sa jedne
        zovšeobecnený stav nahradí vhodnou postupnosťou D obyčajných
        stavov.

## Párové HMM (pair HMM)

Nebrali sme, uvedene pre zaujimavost

  - Emituje dve sekvencie
  - V jednom kroku moze emitovat:
      - pismenka v oboch sekvenciach naraz
      - pismenko v jednej skevencii
      - pismenko v druhej sekvencii

Priklad: HMM s jednym stavom v, takym, ze

  - $e_{v,x,x}=p_1$
  - $e_{v,x,y}=p_2 (x\ne y)$,
  - $e_{v,x,-}=p_3$,
  - $e_{v,-,x}=p_3$
  - tak, aby sucet emisnych pravdepodobnosti bol 1
  - Co reprezentuje najpravdepodobnejsia cesta v tomto HMM?

Zlozitejsi HMM: tri stavy M, X, Y, uplne navzajom poprepajane

  - $e_{M,x,x}=p_1$
  - $e_{M,x,y}=p_2 (x\ne y)$,
  - $e_{X,x,-}=1/4$,
  - $e_{Y,-,y}=1/4$,
  - Co reprezentuje najpravdepodobnejsia cesta v tomto HMM?

**Viterbiho algoritmus pre parove HMM**

  - V\[i,j,u\] = pravdepodobnost najpravdepodobnejsej postupnosti
    stavov, ktora vygeneruje x1..xi a y1..yj a skonci v stave u
  -

$V\[i,j,u\] = \\max\_w \\left\\{ \\begin{array}{l}
V\[i-1,j-1,w\] \\cdot a\_{w,u} \\cdot e\_{u,x\_i,y\_j} \\\\\\\\
V\[i-1,j,w\] \\cdot a\_{w,u} \\cdot e\_{u,x\_i,-} \\\\ V\[i,j-1,w\] \\cdot a\_{w,u}\\cdot e\_{u,-,y\_j} \\\\ \\end{array}\\right.$

  - Casova zlozitost O(mnk^2) kde m a n su dlzky vstupnych sekvencii, k
    je pocet stavov

Ako by sme spravili parove HMM na hladanie genov v dvoch sekvenciach
naraz?

  - Predpokladajme rovnaky pocet exonov
  - V exonoch medzery len cele kodony (oboje zjednodusuje)
  - Inde hocijake medzery

