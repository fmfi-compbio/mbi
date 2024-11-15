---
title: "Cvičenia pre informatikov: Felsensteinov algoritmus"
---

* TOC
{:toc}


## Felsensteinov algoritmus 1981

  - Mame dany strom T s dlzkami hran a bazy v listoch (jeden stlpec
    zarovnania) a model substitucii (zadany napr. maticou rychlosti R).
    Spocitajme pravdepodobnost, ze z modelu dostaneme prave tuto
    kombinaciu baz v listoch.
  - Oznacenie:
      - Nech X\_v je premenna reprezentujuca bazu vo vrchole v a nech
        x\_v je konkretna baza v liste v.
      - Nech listy su 1..n a vnut. vrcholy n+1..2n-1, pricom koren je
        2n-1.
      - Nech p\_v je rodic vrchola v a nech dlzka hrany z v do rodica je
        t\_v.
      - Nech P(a|b,t) je pravdepodobnost, ze b sa zmeni na a za cas t
        (spocitame z matice R, vid minule cvicenia).
          - Napr. v Jukes-Cantorovom modeli
            $P(A|A,t) = (1+3e^{-\frac{4}{3} t})/4$,
            $P(C|A,t)=(1-e^{-\frac{4}{3} t})/4$
      - Nech q\_a je pravdepodobnost bazy a v koreni (ekvilibrium matice
        R)
          - Napr. v Jukes-Cantorovom modeli q\_a = 1/4

<!-- end list -->

  - Ak by sme poznali bazy vo vsetkych vrcholoch, mame
    $P(X_1=x_1 \dots X_{2n-1}=x_{2n-1}|T,R)=q_{x_{2n-1}} \prod_{v=1}^{2n-2}P(x_v|x_{p_v}, t_v)$

<!-- end list -->

  - Chceme pravdepodobnost
    $P(X_1=x_1, X_2=x_2,\dots X_n=x_n|T,R)=\sum_{x_{n+1}\dots x_{2n-1}\in \{A,C,G,T\}^{n-1}} P(X_1=x_1 \dots X_{2n-1}=x_{2n-1}|T,R)$

<!-- end list -->

  - Pocitat sucet cez exponencialne vela dosadeni hodnot za vnutorne
    vrcholy je neefektivne, spocitame rychlejsie dynamickym
    programovanim.
  - Nech A\[v,a\] je pravdepodobnost dat v podstrome s vrcholom v ak
    X\_v=a
  - A\[v,a\] pocitame od listov ku korenu
  - V liste A\[v,a\] = \[a=x\_v\]
  - Vo vnut. vrchole s detmi y a z mame
    $A[v,a] = \sum_{b,c} A[y,b]A[z,c]P(b|a,t_y)P(c|a,t_z)$
  - Celkova pravdepodobnost je
    $P(X_1=x_1, X_2=x_2,\dots X_n=x_n|T,R)=\sum_a A[r,a] q_a$ pre
    koren r.

**Zlozitost, zlepsenie**

  - Zlozitost $O(n|\Sigma|^3)$
  - Pre nebinarne stromy exponencialne
  - Zlepsenie
    $A[v,a] = (\sum_{b} A[y,b]P(b|a,t_y))(\sum_c A[z,c](c|a,t_z))$
  - Zlozitost $O(n|\Sigma|^2)$ aj pre nebinarne stromy

**Chybajuce data**

  - Ak v niektorom liste mame neznamu bazu N, nastavime A\[v,a\]=1
  - Podobne sa spracovavaju medzery v zarovnani, aj ked mohli by sme mat
    aj model explicitne ich modelujuci

**Aposteriorna pravdepodobnost** (nerobili sme)

  - Co ak chceme spocitat pravdepodobnost P(X\_v=a|X\_1=x\_1,
    X\_2=x\_2,\\dots X\_n=x\_n,T,R)? Zaujimaju nas teda sekvencie
    genomov predkov.
  - Potrebujeme B\[v,a\]=pravdpodobnost dat ak podstrom v nahradim
    listom s bazou a.
  - B\[v,a\] pocitame od korena k listom
  - V koreni B\[v,a\] = q\_a
  - Vo vrchole v s rodicom u a surodencom x mame
    $B[v,a]=\sum_{b,c} B[u,b]A[x,c]P(a|b,t_v)P(c|b,t_v)$
  - Ziadana pravdepodobnost je
    $B[v,a]A[v,a]/P(X_1=x_1, X_2=x_2,\dots X_n=x_n|T,R)$
