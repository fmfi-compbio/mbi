---
title: "Cvičenie pre biológov: Úvod do dynamického programovania"
---

* TOC
{:toc}

  - Dynamické programovanie je technika, ktorú uvidíme na ďalšej prednáške na hľadanie zarovnaní
    (alignments)
  - Uvažujme problém platenia pomocou najmenšieho počtu mincí
  - Napr. máme mince hodnoty 1,2,5 centov, z každej dostatok kusov
  - Ako môžeme zaplatiť určitú sumu, napr. 13 centov, s čo najmenším
    počtom mincí?
  - Aké je riešenie? 5+5+2+1 (4 mince)
  - Všeobecná formulácia:
      - Vstup: hodnoty k mincí m\_1,m\_2,...,m\_k a cieľová suma X
        (všetko kladné celé čísla)
      - Výstup: najmenší počet mincí, ktoré potrebujeme na zaplatenie X
  - V našom príklade k=3, m\_1 = 1, m\_2 = 2, m\_3 = 5, X=13
  - Jednoduchý spôsob riešenia: použi najväčšiu mincu, ktorá je najviac
    X, odčítaj od X, opakuj
  - Príklad: najprv použijeme mincu 5, zostane X=8, použijeme opäť mincu
    5, zostane X=3, použijeme mincu 2, zostane X=1, použijeme mincu 1.
  - Nefunguje vždy: zoberme mince hodnôt 1,3,4. Pre X=6 najlepšie
    riešenie je 2 mince: 3+3, ale náš postup (algoritmus) nájde 3 mince
    4+1+1
  - Ukážeme si algoritmus na základe dyn. programovania, ktorý pre každý
    vstup nájde najlepšie riešenie
  - Zrátame najlepší počet mincí nielen pre X, ale pre všetky možné
    cieľové sumy 1,2,3,...,X-1,X
  - To sa zdá byť ťažšia úloha, ale ukáže sa, že z riešenia pre menšie
    sumy vieme zostaviť riešenie pre väčšie sumy, takže nám to vlastne
    pomôže
  - Spravíme si tabuľku, kde si pre každú sumu i=0,1,2,...X pamätáme
    A\[i\]=najmenší počet mincí, ktoré treba na vyplatenie sumy i
  - Ukážme si to na príklade s mincami 1,3,4

```
    i      0    1    2    3    4    5    6    7    8    9  
    A[i]   0    1    2    1    1    2    2    2    2    3
```

  - Nevypĺňali sme ju žiadnym konkrétnym postupom, nejde o algoritmus
  - Ale predstavme si, ze teraz chceme vyplniť A\[10\].
  - V najlepšom riešení je prvá minca, ktorú použijeme 1,3, alebo 4
  - ak je prvá minca 1, máme ešte zaplatiť sumu 10-1=9, tú podľa tabuľky
    vieme najlepšie zaplatiť na 3 mince, takže potrebujeme 4 mince na
    zaplatenie 10
  - ak je prvá minca 3, máme ešte zaplatiť 10-3 = 7, na čo potrebujeme
    podla tabuľky 2 mince, takže spolu 3 mince na zaplatenie 10
  - ak je prvá minca 4, máme ešte zaplatiť 10-4 = 6, na čo treba 2
    mince, t.j. 3 mince na 10
  - Nevieme, ktorá z týchto možností je naozaj v najlepšom riešení, ale
    pre druhé dva prípady dostávame menej mincí, takže výsledok budu 3
    mince (napr. 3+3+4)
  - Zovšeobecníme: A\[i\] = min { A\[i-1\]+1, A\[i-3\]+1, A\[i-4\]+1 }
  - A\[11\] = min { 3+1, 2+1, 2+1} = min {4, 3, 3 } = 3
  - Pre sústavu mincí 1,2,5, máme A\[i\] = 1+ min { A\[i-1\], A\[i-2\],
    A\[i-5\] }
  - Vo všeobecnosti A\[i\] = 1+ min { A\[i-m\_1\], A\[i-m\_2\], ...,
    A\[i-m\_k\] }
  - Vzorec treba modifikovať pre malé hodnoty i, ktoré sú menšie ako
    najväčšia minca, lebo A\[-1\] a pod. nie je definované
  - Pre zaujímavosť, program v Pythone, stačí meniť hodnoty m a X:

```Python
    m = [1,3,4]
    X = 11
    k = len(m)
    nekonecno = math.inf
    A = [0]
    for i in range(1, X + 1):
      min = nekonecno
      for j in range(k):
         if i >= m[j] and A[i - m[j]] < min:
           min = A[i - m[j]]
      A.append(1 + min)
    print(A)
```

  - Ako nájsť, ktoré mince použiť?
  - Pridáme druhú tabuľku B, kde v B\[i\] si pamätáme, ktorá bola
    najlepšia prvá minca, keď sme počítali A\[i\] (ak je viac možností,
    zoberieme ľubovoľnú, napr. najväčšiu)

```
    i      0    1    2    3    4    5    6    7    8    9   10   
    A[i]   0    1    2    1    1    2    2    2    2    3    3
    B[i]   -    1    1    3    4    4    3    4    4    4    4
```

  - Potom ak chceme nájsť napr. mince pre 10, vidíme, že prvá bola
    B\[10\]=4. Zvyšok je 6 a prvá minca na vyplatenie 6 je B\[6\]=3.
    Zostáva nám 3 a B\[3\]=3. Potom nám už zostáva 0, takže sme hotoví.
    Takže najlepšie vyplatenie je 4+3+3
  - Celý program v Pythone:

```Python
    m = [1,3,4]
    X = 11
    k = len(m)
    nekonecno = math.inf
    A = [0]
    B = [-1]
    for i in range(1, X + 1):
      min = nekonecno
      min_minca = -1
      for j in range(k):
         if i >= m[j] and A[i - m[j]] < min:
           min = A[i - m[j]]
           min_minca = m[j]
      A.append(1 + min)
      B.append(min_minca)
    
    while X > 0:
        print(B[X])
        X = X - B[X]
```

Dynamické programovanie vo všeobecnosti

  - Okrem riešenia celého problému vyriešime aj veľa menších
    podproblémov
  - Riešenia podproblémov ukladáme do tabuľky
  - Pri riešení väčšieho podproblému používame už vypočítané hodnoty pre
    menšie podproblémy

Aká je časová zložitosť tohto algoritmu?

  - Dva parametre: X a k
  - Tabuľka veľkosti O(X), každé políčko čas O(k). Celkovo O(Xk)
