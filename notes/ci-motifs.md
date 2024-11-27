---
title: "Cvičenia pre informatikov: Hľadanie motívov"
---

* TOC
{:toc}


## Hľadanie motívov zadefinovaných pravdepodobnostnou maticou

  - Mame danych n sekvencii $S=(S_1\dots S_n)$, kazda dlzky *m*, dlzku
    motivu *L*, nulova hypoteza *q* (frekvencie nukleotidov v genome)
  - Hladame motiv vo forme pravdepodobnostneho profilu dlzky *L* a jeho
    vyskyt v kazdej sekvencii
  - Nech $W[a,i]$ je pravdepodobnost, ze na pozicii *i* motivu bude
    baza *a*, *W* cela matica
  - $o_i$ je pozicia vyskytu v sekvencii $S_i$,
    $O=(o_1 \dots o_n)$ su vsetky vyskyty
  - $\Pr(S|W,O)$ je jednoduchý súčin, kde pre pozície v oknách
    použijeme pravdepodobnosti z *W*, pre pozície mimo okna použijeme
    *q*
      - $\Pr(S_i|W,o_i) =  \prod_{j=1}^{L} W[S_i[j+o_i-1],j] \prod_{j=1}^{o_i-1} q[S_i[j]] \prod_{j=o_i+L}^m q[S_i[j]]$
      - $\Pr(S|W,O) = \prod_{i=1}^n \Pr(S_i|W,o_i)$
  - Hľadáme *W* a *O*, ktoré maximalizujú tuto vierohodnosť Pr(S|W,O)
      - Nepozname efektivny algoritmus, ktory by vedel vzdy najst
        maximum
      - Dali by sa skusat vsetky moznosti *O*, pre dane *O* je najlepsie
        *W* frekvencie z dat
      - Naopak ak pozname *W*, vieme najst najlepsie *O*
          - v kazdej sekvencii *i* skusame vsetky pozicie $o_i$ a
            zvolime tu, ktora ma najvyssiu hodnotu $Pr(S_i|W,o_i)$

### EM algoritmus

  - Iterativne zlepsuje *W*, pricom berie vsetky *O* vahovane podla ich
    pravdepodobnosti vzhladom na *W* z minuleho kola
  - Videli sme na prednaske, tu je trochu prepisany:


  - Inicializácia: priraď každej pozícii *j* v sekvencii $S_i$ nejaké
    skóre $p_{i,j}$
  - Iteruj:
      - Spočítaj *W* zo všetkých možných výskytov v $S_1,\dots,S_k$
        váhovaných podľa $p_{i,j}$
      - Prepočítaj všetky skóre $p_{i,j}$ tak, aby zodpovedali pomerom
        pravdepodobností výskytu *W* na pozícii *j* v $S_i$, t.j.
        $p_{i,j}$ je umerne $Pr(S_i|W,o_i=j)$, pricom hodnoty
        normalizujeme tak, aby sucet v riadku bol 1

### Gibbsovo vzorkovanie (Gibbs sampling)

  - Inicializácia: Vezmi náhodné pozície výskytov *O*
  - Iteruj:
      - Spočítaj *W* z výskytov *O*
      - Vyber náhodne jednu sekvenciu $S_i$
      - Pre každú možnú pozíciu *j* v $S_i$ spočítaj skóre $p_{i,j}$
        (ako v EM) výskytu *W* na tejto pozícii
      - Zvoľ $o_i$ náhodne s váhovaním podľa $s_{i,j}$


  - Takto dostavame postupnost vzoriek $O^{(0)}, O^{(1)}, ...$.
  - Za sebou iduce vzorky sa podobaju (lisia sa len v jednej zlozke
    $o_i$) nie su teda nezavisle
  - Pre kazdu vzorku $O^{(t)}$ najdeme najlepsie $W^{(t)}$ a
    spocitame vierohodnost $\Pr(S|W^{(t)},O^{(t)})$. Nakoniec
    vyberieme *O* a *W*, kde bola vierohodnost najvyssia.
  - Tento algoritmus (s malymi obmenami) bol pouzity v clanku Lawrence,
    Charles E., et al. (1993) "Detecting subtle sequence signals: a
    Gibbs sampling strategy for multiple alignment." Science.
      - V clanku v kazdej iteracii maticu *W* rataju zo vsetkych
        sekvencii okrem $S_i$
      - Obcas robia krok, kde nahodne skusaju posunut vsetky vyskyty o
        jedna dolava alebo doprava
      - Tento algoritmus nie je uplne matematicky korektne Gibbsovo
        vzorkovanie (nema ani poradne zadefinovane rozdelenie, z ktoreho
        vzorkuje). Na spodku stranky pre informaciu uvadzame algoritmus
        Gibbsovho vzorkovanie pre hladanie motivov z ineho clanku.

## Vzorkovanie z pravdepodobnostného modelu vo všeobecnosti

  - Majme pravdepodobnostny model, kde D su nejake pozorovane data a X
    nezname nahodne premenne (napr pre nas D su sekvencie S a X su
    vyskyty O, pripadne aj matica W)
  - mozeme hladat X pre ktore je vierohodnost Pr(D|X) najvyssia
  - alebo mozeme nahodne vzorkovat rozne X z Pr(X|D)

Pouzitie vzoriek

  - spomedzi ziskanych vzoriek zvolime tu, pre ktoru je vierohodnost
    Pr(D|X) najvacsia (iny pristup k maximalizovaniu vierohodnosti)
  - ale vzorky nam daju aj informaciu o tom, aka je velka neurcitost v
    odhade X
      - mozeme odhadovat stredne hodnoty a odchylky roznych velicin
      - napr. pri hladani motivov mozeme sledovat ako casto je ktora
        pozicia vyskytom motivu

<!-- end list -->

  - generovat nezavisle vzorky z Pr(X|D) moze byt tazke
  - metoda Markov chain Monte Carlo (MCMC) generuje postupnost zavislych
    vzoriek $X^{(0)}, X^{(1)},\dots$, konverguje v limite k cielovej
    distribucii Pr(X|D)
  - Gibbsovo vzorkovanie je specialnym pripadom MCMC

### Markovove reťazce

  - **Markovov reťazec** je postupnosť náhodných premenných
    $X^{(0)}, X^{(1)}, \dots,$ taká, že
    $\Pr(X^{(t)}|X^{(0)},\dots,X^{(t-1)}) = \Pr(X^{(t)}|X^{(t-1)})$,
    t.j. hodnota v čase $t$ závisí len od hodnoty v čase $t-1$ a nie
    ďalších predchádzajúcich hodnôt.
  - Nás budú zaujímať **homogénne** Markovove reťazce, v ktorých
    $\Pr(X^{(t)}|X^{(t-1)})$ nezávisí od $t$.
  - Tiez nas zaujimaju len retazce v ktorych nahodne premenne $X_t$
    nadobudaju hodnoty z konecnej mnoziny (mozne hodnoty $X^{(t)}$
    nazyvame **stavy**)
      - Napriklad stavy A,C,G,T
      - V Gibbsovom vzorkovani pre motivy je stav konfiguracia
        premennych O (t.j. mame (m-L+1)^n stavov)
          - Vzorka v kroku t zavisi od vzorky v kroku t-1 (a lisi sa len
            v hodnote jedneho o\_i)

**Matica**

  - Pravdepodobnosti prechodu medzi stavmi za jeden krok mozeme vyjadrit
    maticou pravdepodobnosti P, ktorej prvok $p_{x,y}$ oznacuje
    pravdepodobnost prechodu zo stavu x do stavu y
    $p_{X,Y}=\Pr(X_t=y|X_{t-1}=x)$
      - Sucet kazdeho riadku je 1, cisla nezaporne
  - Ako $p_{x,y}^t$ budeme oznacovat $\Pr(X^{(t)}=y|X^{(0)}=x)$,
    tieto hodnoty dostaneme umocnenim matice *P* na *t*

**Stacionarne rozdelenie**

  - Rozdelenie $\pi$ na mnozine stavov sa nazyva **stacionarne** pre
    Markovov retazec $P$, ak pre kazde j plati
    $\sum_{i}\pi(i)p_{i,j} = \pi(j)\,$ (alebo v maticovej notacii
    $\pi P = \pi$)
  - Ak matica P splna urcite podmienky (je ergodicka), existuje pre nu
    prave jedno stacionarne rozdelenie $\pi$. Navyse pre kazde x a y
    plati $\lim_{t\to\infty} p_{x,y}^{t} = \pi(y)\,$

**Priklady Markovovskych retazcov v bioinformatike**

  - V HMM stavy tvoria Markovov retazec
  - Ine varianty: nekonecne stavove priestory (zlozitejsia teoria),
    spojity cas (videli sme pri evolucnych modeloch), retazce vyssieho
    radu, kde urcujeme $\Pr(X_t|X_{t-r},\dots,X_{t-1})$ a pod.
  - Pouzitie v bioinformatike: charakterizacia nahodnych sekvencii
    (nulova hypoteza), pre DNA sa pouzivaju rady az do 5, lepsie ako
    nezavisle premenne

**Ergodické Markovove reťazce**

  - Vravime ze matica je **ergodicka**, ak $P^t$ pre nejake t\>0 ma
    vsetky polozky nenulove
  - Priklady neergodickych matic

<!-- end list -->

    1 0          0.5 0.5          0 1             0.5 0.5
    0 1          0   1            1 0             1   0
    nesuvisla    slabo suvisla    periodicka      ergodicka

  - V HMM stavy tvoria Markovov retazec; hladanie genov ergodicky
    stavovy priestor, profilove HMM nie

### Markov chain Monte Carlo MCMC

  - Chceme generovať náhodné vzorky z nejakeho cieloveho rozdelenia
    $\pi$, ale toto rozdelenie je prilis zlozite.
  - Zostavime ergodicky Markovov retazec, ktoreho stacionarne rozdelenie
    je rozdelenie $\pi$, tak aby sme efektivne vedeli vzorkovat
    $X^{(t)}$ ak vieme $X^{(t-1)}$.
  - Ak zacneme z lubovolneho bodu $X^{(0)}$, po urcitom case t
    rozdelenie $X^{(t)}$ priblizne $\pi$
  - Ale za sebou iduce vzorky nie su nezavisle\!
  - Vieme vsak odhadovat ocakavane hodnoty roznych velicin
    $\frac{1}{t} \sum_{i=1}^t f(X^{(t)})$ konverguje k
    $E_\pi [f(X)]$

### Gibbsovo vzorkovanie

  - Cielove rozdelenie $\pi(X)$ je cez vektory dlzky *n*
    $X=(x_1,...x_n)$
  - V kazdom kroku vzorkujeme jednu zlozku vektora $x_i$ z podmienenej
    pravdepodobnosti $\Pr(x_i|x_1,\dots,x_{i-1},x_{i+1},\dots x_n)$
  - Ostatne hodnoty nechame rovnake ako v predchadzajucom kroku
  - Hodnotu $i$ zvolime nahodne alebo periodicky striedame
    $i=1,2,\dots,n$

### Dôkaz správnosti Gibbsovho vzorkovania

  - Pozor\! Gibbsovo vzorkovanie nie je vzdy ergodicke, ak niektore
    kombinacie hodnot maju nulovu pravdepodobnost\!
  - Treba dokazat, ze ak je ergodicky, tak ma ako stacionarnu
    distribuciu nase zvolene $\pi$
  - Definicia: Vravime, ze matice P a rozdelenie $\pi$ splnaju
    **detailed balance**, ak pre kazde stavy (dva vektory hodnot) *x* a
    *y* mame $\pi(x)p_{x,y} = \pi(y)p_{y,x}$
  - Lema: ak pre nejaky retazec P a nejake rozdelenie $\pi$ plati
    detailed balance, $\pi$ je stacionarna distribucia pre P
      - Dokaz:
        $\sum_x \pi(x)p_{x,y} = \sum_x \pi(y)p_{y,x} = \pi(y)\sum_x p_{y,x} = \pi(y)$
  - Lema: pre retazec Gibbsovo vzorkovania plati detailed balance
    vzhladom na cielove rozdelnie $\pi$
      - Dokaz: uvazujme dva za sebou iduce vektory hodnot x a y, ktore
        sa lisia v i-tej suradnici. Nech $x_{-i}$ su hodnoty vsetkych
        ostatnych premennych okrem $x_i$
      - $\pi(x)p_{x,y} = \pi(x)\Pr(y_i|x_{-i}) = \Pr(x_{-i})\Pr(x_i|x_{-i}) \Pr(y_i|x_{-i}) = \pi(y)\Pr(x_i|x_{-i}) = \pi(y)\Pr(x_i|y_{-i}) = \pi(y)p_{y,x}$

## Poriadnejšie Gibbsovo vzorkovanie pre motívy

Uvedene pre zaujimavost - podla clanku 

**Pravdepodobnostny model**

  - Rozsirime model, aby aj O a W boli nahodne premenne, takze mame
    rozdelenie Pr(S,W,O)
      - Potom chceme vzorkovat z Pr(O|S) (marginalizujeme cez vsetky
        hodnoty W)
  - Vygeneruje sa nahodne matica pravdepodobnosti W (napr z roznomernej
    distribucie cez vsetky matice)
  - V kazdej sekvencii i sa zvoli okno $o_i$ dlzky L (rovnomerne z
    m-L+1 moznosti)
  - V okne sa generuje sekvencia podla profilu W a mimo okna sa generuje
    sekvencia z nulovej hypotezy (ako predtym)

**Gibbsovo vzorkovanie**

  - Mame dane S, vzorkujeme O ($O^{(0)}, O^{(1)}, \dots$) (ak treba, z
    $O^{(t)}$ mozeme zostavit maticu $W^{(t)}$)
      - zacni s nahodnymi oknami $O^{(0)}$
      - v kroku t+1 zvol jednu sekvenciu i a pre vsetky pozicie $o'_i$
        spocitaj $\Pr(o'_i|O^{(t)}_{-i},S)$ (kde
        $O_{-i}=o_1\dots o_{i-1}o_{i+1}\dots o_n$, t.j. všetky pozície
        výskytov okrem i-tej).
      - nahodne zvol jedno $o'_i$ umerne k tymto pravdepodobnostiam
      - $O^{(t+1)}$ dostaneme z $O^{(t)}$ vymenou pozicie v
        sekvencii i za prave zvolenu
      - opakuj vela krat
  - Konverguje k cielovemu rozdeleniu $\Pr(O|S)$, ale vzorky nie su
    nezavisle
  - Dalsie mozne kroky vo vzorkovani: posun vsetky okna o konstantu
    vlavo alebo vpravo
  - Dalsie moznosti rozsirenia modelu/algoritmu: pridaj rozdelenie cez
    *L* a nahodne zvacsuj/zmensuj *L*, dovol vynechat motiv v niektorych
    sekvenciach, hladaj viac motivov naraz,...

**Ako spocitat $\Pr(o_i|O_{-i},S)$?**

  - nezaujimaju nas normalizacne konstanty, lahko znormalizujeme
    scitanim cez vsetky $o'_i$
  - $\Pr(o_i|O_{-i},S) = \Pr(O|S) / \Pr(O_{-i}|S)$, ale menovatel
    konstanta
  - $\Pr(O|S) = \Pr(S|O)\Pr(O)/\Pr(S)$, kde
    $\Pr(S)=\sum_{O'} \Pr(S|O')\Pr(O')$
  - Menovatel nas nezaujima (normalizacna konstanta)
  - $\Pr(O)$ je tiez konstanta (rovnomerne rozdelenie pozicii okien)
  - Teda mame $\Pr(o_i|O_{-i},S)$ je umerne $\Pr(S|O)$
  - Lahko vieme spocitat $\Pr(S|W,O)$, potrebujeme "zrusit" W, da sa
    spocitat vzorec...
  - Skusame vsetky mozne hodnoty $o'_i$, pocitame pravdepodobnost
    $\Pr(S|O)$, vzorkujeme umerne k tomu

Dalsie detaily vypoctu $\Pr(S|O)$:

  - Nech $S_o$ su len sekvencie v oknach a $S_{-o}$ mimo okien. Mame
    $\Pr(S|O) = \Pr(S_o|O)\Pr(S_{-o}|O)$
  - $\Pr(S_{-o}|O)$ lahko spocitame (nezavisi od W)
  - $\Pr(S_o|O) = \int \Pr(S_o|O,W)\Pr(W)dW$ kde integral ide cez
    hodnoty, kde $w_{a,i}\ge 0$ a $\sum_a w_{a,i} = 1\,$
  - $\Pr(W)$ je konstanta (rovnomerne rozdelenie; nejde o
    pravdepodobnost ale hustotu),
    $\Pr(S_o|O,W) = \prod_{i=1}^L \prod_a (w_{a,i})^{n_{a,i}}$, kde
    $n_{a,i}$ je pocet vyskytov bazy a na pozicii i v oknach
    $o_1\dots o_n$
  - $\Pr(S_o|O) = \prod_{i=1}^L 3!/(n+3)! \prod_a n_{a,i}!$ (bez
    dokazu)

