---
title: "Cvičenia pre informatikov: Felsensteinov algoritmus"
---

* TOC
{:toc}

## Výpočet úspornosti stromu dynamickým programovaním

* Máme daný strom a jeden stĺpec zarovnania, ktorý určuje bázu v každom liste
* Cieľom je spočítať, koľko najmenej mutácií muselo nastať v evolučnej histórii podľa tohto stromu, aby sme získali tieto konkrétne bázy v listoch
* Podproblém: 
$N[v,a]$: koľko zmien treba v podstrome pod vrcholom $v$, ak vo $v$ bude symbol $a$?
* Rekurencia pre vnútorný vrchol $v$ s deťmi $y$ a $z$: 
$N[v,a] = \min_b\{N[y,b]+[a\neq b]\}+\min_c\{N[w,c]+[a\neq c]\}$
  * ako $[podmienka]$ označujeme hodnotu 1 ak podmienka platí a 0 ak neplatí
* Potrebujeme dodefinovať triviálny prípad v liste $v$ v ktorom je v podľa zarovnania symbol $b$:
$N[v,a] = [a\ne b]$


## Felsensteinov algoritmus 1981

* Máme daný strom $T$ s dĺžkami hrán a bázy v listoch (jeden stĺpec
  zarovnania) a model substitúcií (zadaný napr. maticou rýchlostí $R$).
  Spočítajme pravdepodobnosť, že z modelu dostaneme práve túto
  kombináciu báz v listoch.
- Označenie:
    - Nech $X_v$ je náhodná premenná reprezentujúca bázu vo vrchole $v$ a nech
      $x_v$ je konkrétna báza v liste $v$.
    - Nech listy sú $1\dots n$ a vnútorné vrcholy $n+1\dots 2n-1$, pričom koreň je $2n-1$.
    - Nech $p_v$ je rodič vrchola $v$ a nech dĺžka hrany z $v$ do rodiča je $t_v$.
    - Nech $\Pr(a\stackrel{t}{\rightarrow} b)$ je pravdepodobnosť, že $a$ sa zmení na $b$ za čas $t$
      (spočítame z matice $R$, viď minulé cvičenia).
        - Napr. v Jukes-Cantorovom modeli
            $\Pr(A\stackrel{t}{\rightarrow} A) = (1+3e^{-\frac{4}{3} t})/4$,
            $\Pr(A\stackrel{t}{\rightarrow} C) = (1-e^{-\frac{4}{3} t})/4$
    - Nech $q_a$ je pravdepodobnosť bázy $a$ v koreni (ekvilibrium matice $R$)
      - Napr. v Jukes-Cantorovom modeli $q_a = 1/4$

- Ak by sme poznali bázy vo všetkých vrcholoch, máme
  $\Pr(X_1=x_1 \dots X_{2n-1}=x_{2n-1}|T,R)=q_{x_{2n-1}} \prod_{v=1}^{2n-2}P(x_v|x_{p_v}, t_v)$

- Chceme pravdepodobnosť
  $P(X_1=x_1, X_2=x_2,\dots X_n=x_n|T,R)=\sum_{x_{n+1}\dots x_{2n-1}\in \\{A,C,G,T\\}^{n-1}} P(X_1=x_1 \dots X_{2n-1}=x_{2n-1}|T,R)$
- Počítať súčet cez exponenciálne veľa dosadení hodnôt za vnútorné vrcholy je neefektívne, spočítame rýchlejšie dynamickým programovaním.
- Nech $A[v,a]$ je pravdepodobnosť dát v podstrome s koreňom $v$ ak $X_v=a$
- $A[v,a]$ počítame od listov ku koreňu
- V liste $A[v,a] = [a=x_v]$
- Vo vnút. vrchole $v$ s deťmi $y$ a $z$ máme
  $A[v,a] = \sum_{b,c} A[y,b]A[z,c]\Pr(a\stackrel{t_y}{\rightarrow} b)\Pr(a\stackrel{t_z}{\rightarrow} c)$
- Celková pravdepodobnosť je $P(X_1=x_1, X_2=x_2,\dots X_n=x_n|T,R)=\sum_a A[r,a] q_a$ pre koreň $r$.

### Zložitosť, zlepšenie

- Zložitosť $O(n|\Sigma|^3)$
- Pre nebinárne stromy exponenciálne
- Zlepšenie
  $A[v,a] = (\sum_{b} A[y,b]\Pr(a\stackrel{t_z}{\rightarrow} c))\cdot (\sum_c A[z,c]\Pr(a\stackrel{t_z}{\rightarrow} c))$
- Zložitosť $O(n|\Sigma|^2)$ aj pre nebinárne stromy

### Chýbajúce dáta

- Ak v niektorom liste máme neznámu bázu N, nastavíme $A[v,a]=1$ pre všetky $a$
- Podobne sa spracovávajú medzery v zarovnaní, aj keď mohli by sme mať aj model explicitne ich modelujúci

## Aposteriórna pravdepodobnosť 

Nerobili sme, uvedené pre zaujímavosť.

- Čo ak chceme spočítať pravdepodobnosť $\Pr(X_v=a|X_1=x_1,
  X_2=x_2,\dots X_n=x_n,T,R)? Zaujímajú nás teda sekvencie genómov predkov.
- Potrebujeme $B[v,a]$: pravdepodobnosť dát ak podstrom $v$ nahradím listom s bázou $a$.
- $B[v,a]$ počítame od koreňa k listom
- V koreni $B[v,a] = q_a$
- Vo vrchole $v$ s rodičom $u$ a súrodencom $x$ máme
    $B[v,a]=\sum_{b,c} B[u,b]A[x,c]\Pr(b\stackrel{t_v}{\rightarrow} a) \Pr(b\stackrel{t_v}{\rightarrow} c)$
  - Žiadaná pravdepodobnosť je
    $B[v,a]A[v,a]/\Pr(X_1=x_1, X_2=x_2,\dots X_n=x_n|T,R)$

