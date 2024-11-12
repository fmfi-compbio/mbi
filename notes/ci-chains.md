---
title: "Cvičenia pre informatikov: Substitučné modely"
---

* TOC
{:toc}


## Markovove reťazce (Markov chains)

- Markovov reťazec je o niečo jednoduchší model ako skrytý Markovov
  model (HMM), nakoľko obsahuje iba stavy a pravdepodobnosti prechodu
  medzi nimi, ale neuvažujeme žiadne emisie
- Formálne, **Markovov reťazec** je postupnosť náhodných premenných
  $X_0, X_1, \dots,$ taká, že $\Pr(X_t|X_0,\dots,X_{t-1}) =
  \Pr(X_t|X_{t-1})$, t.j. stav v čase $t$ závisí len od stavu v čase
  $t-1$ a nie ďalších predchádzajúcich stavov (stavmi nazývame možné hodnoty
  $X_{t}$).
- Nás budú zaujímať **homogénne** Markovove reťazce, v ktorých
  $\Pr(X_t|X_{t-1})$ nezávisí od $t$.
- Tiež nás zaujímajú len reťazce, v ktorých náhodné premenné $X_t$
  nadobúdajú hodnoty z konečnej množiny.
- Príklad 1: model počasia, stavy prší / svieti slnko, $X_t$ je počasie v deň $t$.
- Príklad 2: stavy A,C,G,T, dá sa použiť na generovanie náhodnej DNA alebo na sledovanie mutácií na jednej konkrétnej pozícii v chromozóme. V druhom prípade je $X_t$ báza na tejto pozícii v čase $t$ (analogicky s počasím).

### Matica pravdepodobností prechodu

- Pravdepodobnosti prechodu medzi stavmi mozeme vyjadrit
  maticou $P$, ktorej prvok $p_{x,y}$ oznacuje
  pravdepodobnost prechodu zo stavu x do stavu y
  $p_{x,y}=\Pr(X_t=y|X_{t-1}=x)$
- Pre maticu musí platiť, že súčet kazdeho riadku je 1, všetky prvky
  matice sú nezaporne
- Nezávislé hody mincou sú tiež špeciálny prípad Markovovho reťazca. Ako vyzerá jeho matica?


- Pre reťazec s maticou $P$ spočítajme $\Pr(X_2=y\|X_0=x)$
  $\sum_z \Pr(X_1=z|X_0=x)\cdot\Pr(X_2=y|X_1=z) = \sum_z p_{x,z}p_{z,y}$
- Ide o súčin matice $P$ samej so sebou, t.j. $P^2$
- Podobne $\Pr(X_t=y|X_0=x)$ získame z matice $P^t$
- Koľko trvá výpočet $P^t$ v závislosti od $t$ a počtu stavov $n$?

### Stacionárne rozdelenie

  - Rozdelenie $\pi$ na mnozine stavov sa nazyva **stacionarne** pre
    Markovov retazec $P$, ak pre kazde $y$ plati
    $\sum_{x}\pi(x)p_{x,y} = \pi(y)$ (alebo v maticovej notacii
    $\pi P = \pi$)
  - Ak matica P spĺňa určité podmienky (je ergodická), existuje pre ňu
    práve jedno stacionarne rozdelenie $\pi$. Navyse pre kazde x a y
    platí $\lim_{t\to\infty} p_{x,y}^{t} = \pi(y)$
  - To znamená, že ak umocňujeme maticu $P$ na veľmi veľké čísla,  
    všetky riadky matice sa blížia k stacionárnemu rozdeleniu.
  - Po veľa krokoch už teda príliš nezáleží, v ktorom stave sme začali.

### Ergodické Markovove reťazce**

- Vravíme, že matica je **ergodická**, ak $P^t$ pre nejaké $t>0$ má všetky položky nenulové.
- Príklady neergodických a ergodických matíc:

```
    1 0          0.5 0.5          0 1             0.5 0.5
    0 1          0   1            1 0             1   0
    nesúvislá    slabo súvislá    periodická      ergodická
```


## Substitučné modely - odvodenie

  - Nech $P(b|a,t)$ je pravdepodobnosť, že ak začneme s bázou *a*, tak
    po čase *t* budeme mať bázu *b*.
  - Pre dané *t* môžeme také pravdepodobnosti usporiadať do matice 4x4 (ak
    študujeme DNA):

$S(t) = \left(\begin{array}{cccc} 
P(A|A,t) & P(C|A,t) & P(G|A,t) & P(T|A,t) \\\\\\\\
P(A|C,t) & P(C|C,t) & P(G|C,t) & P(T|C,t) \\\\\\\\
P(A|G,t) & P(C|G,t) & P(G|G,t) & P(T|G,t) \\\\\\\\
P(A|T,t) & P(C|T,t) & P(G|T,t) & P(T|T,t) 
\end{array}\right)$

  - Riadky zodpovedaju povodnej baze $a$, stlpce novej baze $b$
  - Sucet kazdeho riadku je 1

### Požiadavky na S(t)

  - Intuitivne cim vacsie $t$, tym vacsia pravdepodobnost zmeny, pre
    nulovy cas este ziadna zmena nemala kedy nastat, mame teda
    $S(0)=I$ (jednotkova matica)
  - Naopak ked $t$ ide do nekonecna, kazda baza velakrat zmutovala a teda
    uz prilis nezalezi, co to bolo na zaciatku. S(t) ma teda v limite
    pre velke t vsetky riadky rovnake.
  - $\\lim\_{t\\rightarrow \\infty} S(t) =
    \\left(\\begin{array}{cccc}
\\pi\_A & \\pi\_C & \\pi\_G & \\pi\_T \\\\\\\\
\\pi\_A & \\pi\_C & \\pi\_G & \\pi\_T \\\\\\\\
\\pi\_A & \\pi\_C & \\pi\_G & \\pi\_T \\\\\\\\
\\pi\_A & \\pi\_C & \\pi\_G & \\pi\_T 
\\end{array}\\right)$

  - Rozdelenie pravdepodobnosti $\pi$ nazyvame limitne (equilibrium)
  - Predpokladame tiez, ze pravdepodobnost mutacie zavisi len od
    aktualnej bazy, nie od minulych stavov a ze charakter procesu
    mutacii sa v case nemeni. Teda ak mame matice pre casy $t_1$ a
    $t_2$, vieme spocitat maticu pre cas $t_1+t_2$:
    $P(b|a,t_1+t_2)=\sum_x P(x|a,t_1)\cdot P(b|x,t_2)$ a teda v
    maticovej notacii $S(t_1+t_2) = S(t_1)S(t_2)$. Preto takyto model
    nazyvame multiplikativny.
  - Ak by sme uvazovali iba diskretne (celociselne) casy, stacilo by nam
    urcit iba $S(1)$ a vsetky ostatne casy dostaneme umocnenim tejto
    matice. Je vsak elegantnejsie mat $S(t)$ definovane aj pre realne
    $t$.
  - Tento typ modelu sa nazyva Markovov retazec so spojitym casom
    (continuous-time Markov chain)

### Jukes-Cantorov substitučný model

  - Tento model predpoklada, ze vsetky substitucie su rovnako
    pravdepodobne, matica teda musí vyzerať nejako takto:

$S(t) = 
\left(\begin{array}{cccc}
1-3s(t) & s(t) & s(t) & s(t) \\\\\\\\
s(t) & 1-3s(t) & s(t) & s(t) \\\\\\\\
s(t) & s(t) & 1-3s(t) & s(t) \\\\\\\\
s(t) & s(t) & s(t) & 1-3s(t) 
\end{array}\right)$

### Matica rýchlostí pre J-C model

$S(2t) = S(t)^2 = 
\left(\begin{array}{cccc}
1-6s(t)+12s(t)^2 & 2s(t)-4s(t)^2 & 2s(t)-4s(t)^2 & 2s(t)-4s(t)^2 \\\\\\\\
\dots
\end{array}\right)$

  - Pre velmi maly cas *t* je *s(t)* velmi male cislo (blizke 0) a pre
    velmi male *s(t)* su kvadraticke cleny $s(t)^2$ ovela mensie ako
    linearne cleny *s(t)* a teda

$S(2\epsilon) = S(\epsilon)^2\approx
\left(\begin{array}{cccc}
1-6s(\epsilon) & 2s(\epsilon) & 2s(\epsilon) & 2s(\epsilon) \\\\\\\\
\dots
\end{array}\right)$

  - Aj pre ine rozumne male nasobky

$S(c\epsilon) \approx
\left(\begin{array}{cccc}
1-3cs(\epsilon) & cs(\epsilon) & cs(\epsilon) & cs(\epsilon) \\\\\\\\
\dots
\end{array}\right)$

  - Tento vztah dava zmysel: pre velmi male t mozeme zanedbat moznost,
    ze baza zmutovala viackrat a pravdepodobnost jednej mutacie linearne
    rastie s dlzkou casu.
  - Ak oznacime $t=c\epsilon$ a $\alpha = s(\epsilon) / \epsilon$
    dostaneme pre velmi male t

$S(t) \approx
\left(\begin{array}{cccc}
1-3 \alpha t & t\alpha & t\alpha & t\alpha \\\\\\\\
\dots
\end{array}\right)$

  - Vytvorme si teraz maticu rychlosti (intenzit) (transition rate
    matrix, substitution rate matrix)

$R=
\left(\begin{array}{cccc}
-3\alpha & \alpha & \alpha & \alpha \\\\\\\\
\alpha & -3\alpha & \alpha & \alpha \\\\\\\\
\alpha & \alpha & -3\alpha & \alpha \\\\\\\\
\alpha & \alpha & \alpha & -3\alpha
\end{array}\right)$

  - Dostavame, ze pre velmi male casy plati $S(t)\approx I+Rt$
  - $S(t+\epsilon) = S(t)S(\epsilon) \approx S(t)(I+R\epsilon)$ a teda
    $(S(t+\epsilon)-S(t))/\epsilon \approx S(t)R$
  - V limite dostaneme
    $S(t)R = \lim_{\epsilon\rightarrow 0} (S(t+\epsilon)-S(t))/\epsilon = S'(t)$
  - Dostali sme diferencialnu rovnicu S(t)R = S'(t), pociatocny stav
    $S(0)=I$).
  - Nasobenim matic S(t) a R dostavame, ze diagonalny prvok $S(t)R$ je
    $-3\alpha+12\alpha s(t)$ a nediagonalny $\alpha-4\alpha s(t)$.
    Takze dostavame diferencialnu rovnicu
    $s'(t) = \alpha-4\alpha s(t)$ z rovnosti nediagonalnych prvkov (z
    rovnosti diagonalnych prvkov dostavame tu istu rovnicu len
    prenasobenu konstantou -3).

### Poriadnejšie odvodenie diferenciálnej rovnice

  - $s'(t) = \lim_{\epsilon\to 0} \frac{s(t+\epsilon)-s(t)}{\epsilon}$
    z definicie limity
  - $s(t+\epsilon) = (1-3s(t))s(\epsilon) + s(t)(1-3s(\epsilon))+s(t)s(\epsilon)+s(t)s(\epsilon)$
    podla multiplikativnosti S(t)
  - po úprave
    $s(t+\epsilon) = s(\epsilon) +s(t) - 4s(t)s(\epsilon) = s(t)+s(\epsilon)(1-4s(t))$
  - $s'(t) = \lim_{\epsilon\to 0} \frac{s(\epsilon) (1-4s(t))}{\epsilon} = (1-4s(t))\lim_{\epsilon\to 0} \frac{s(\epsilon)}{\epsilon}= (1-4s(t))s'(0)$
  - oznacme $\alpha = s'(0)$ (alfa je konstanta, nezavisi od epsilon
    ani t)
  - $s'(t) = \alpha (1-4s(t))$ (finalna diferencialna rovnica, rovnaka
    ako predtym pre matice)
  - Riesenie diferencialnej rovnice $s(t) = 1/4+c e^{-4\alpha t}$ pre
    kazdu konstantu c
  - Mozeme overit dosadenim do rovnice, pricom
    $s'(t) = -4 c \alpha e^{-4\alpha t}$
  - c=-1/4 dopocitame z pociatocnej podmienky s(0)=0
  - overime tiez, ze $s'(0)=\alpha$

### Vlastnosti riešenia

  - Takže máme maticu:

$S(t)=
\left(\begin{array}{cccc}
(1+3e^{-4\alpha t})/4 & (1-e^{-4\alpha t})/4 & (1-e^{-4\alpha t})/4 & (1-e^{-4\alpha t})/4 \\\\\\\\
\dots
\end{array}\right)$

  - Ked $t\rightarrow \infty$, dostávame všetky prvky matice rovné
    1/4, t.j. $\lim_{t\to \infty}s(t)=\lim_{t\to \infty}1-3s(t)=1/4$.
  - $\alpha$ je teda pravdepodobnosť konkrétnej zmeny za jednotku
    času, ak uvažujeme veľmi krátke časy alebo presnejšie derivácia
    prvku *s(t)* vzhľadom na *t* v bode 0
  - Aby sme nemali naraz aj $\alpha$ aj $t$, zvykneme maticu R
    normalizovat tak, aby priemerný počet substitúcii za jednotku času
    bol 1. V prípade Jukes-Cantorovho modelu je to keď $\alpha=1/3$.

## Substitučné matice, zhrnutie

  - S(t): matica 4x4, kde políčko $S(t)_{a,b}=P(b|a,t)$ je
    pravdepodobnosť, že ak začneme s bázou a, tak po čase t budeme mať
    bázu b.
  - Jukes-Cantorov model predpokladá, že táto pravdepodobnosť je rovnaká
    pre každé dve bázy $a\ne b$
  - Pre daný čas t máme teda všade mimo diagonály s(t) a na diagonále
    1-3s(t)
  - Matica rýchlostí R: pre Jukes-Cantorov model všade mimo diagonály
    $\alpha$, na diagonále $-3\alpha$
  - Pre veľmi malý čas t je S(t) zhruba I-Rt
  - Rýchlost alpha je teda pravdepodobnosť zmeny za jednotku casu, ak
    uvažujeme veľmi krátke časy, resp. derivácia *s(t)* vzhľadom na *t*
    v bode 0
  - Riešením diferenciálnych rovníc pre Jukes-Cantorov model dostávame
    $s(t) = (1-e^{-4\alpha t})/4$
  - Matica rýchlostí sa zvykne normalizovať tak, aby na jednotku času
    pripadla v priemere jedna substitúcia, čo dosiahneme ak
    $\alpha=1/3$

## Použitie na odhad evolučnej vzdialenosti

  - V case $t$ je pravdepodobnost, ze uvidime zmenenu bazu
    $D(t) = \frac{3}{4}(1-e^{-4\alpha t})$
  - V realnom pouziti (vypocet matice vzdialenosti pre metodu spajania
    susedov) mame dve zarovnane sekvencie, medzi ktorymi vidime $d\%$
    zmenenych baz, chceme odhadnut t
      - Spatne teda zratame t, ktore by hodnote $D(t)=d$ prinalezalo.
  - Dostavame teda vzorec pre vzdialenost, ktory sme videli na prednaske
    $t=-\frac{3}{4} \log\left(1-\frac{4}{3}d\right)$
  - Ak $d\rightarrow 0.75$, dostavame $t\rightarrow \infty$
  - Preco sme ten vzorec odvodili takto? V skutocnosti chceme najst
    najvierohodnejsiu hodnotu t, t.j. taku, pre ktore hodnota P(data|t)
    bude najvacsia. Zhodou okolnosti vyjde takto.

## Zložitejšie modely

V praxi sa používajú komplikovanejsie substitučné modely, ktoré majú
všeobecnejšiu maticu rýchlostí R

  - $R = \\left( \\begin{array}{cccc}
  . & \\mu\_{AC} & \\mu\_{AG} & \\mu\_{AT}\\\\\\\\
  \\mu\_{CA} & . & \\mu\_{CG} & \\mu\_{CT}\\\\\\\\
  \\mu\_{GA} & \\mu\_{GC} & . & \\mu\_{GT}\\\\\\\\
  \\mu\_{TA} & \\mu\_{TC} & \\mu\_{TG} & . 
  \\end{array} \\right)$
  - Hodnoty na diagonále matice sa dopočítavajú aby súčet každého riadku bol 0.

  - Hodnota $\mu_{xy}$ v tejto matici vyjadruje rýchlosť, akou sa
    určitá báza x mení na inú bázu y.
  - Presnejšie
    $\mu_{xy} = \lim_{t\rightarrow 0}\frac{\Pr(y\,|\,x,t)}{t}$.

Kimurov model napr. zachytáva, ze puríny sa častejšie menia na iné
puríny (A a G) a pyrimidíny na ine pyrimidíny (C a T).

  - Má dva parametre: rýchlosť tranzícií alfa, transverzií beta

$R=\left(\begin{array}{cccc}
-2\beta-\alpha & \beta & \alpha & \beta \\\\\\\\
\beta & -2\beta-\alpha & \beta & \alpha \\\\\\\\
\alpha & \beta & -2\beta-\alpha & \beta \\\\\\\\
\beta & \alpha & \beta & -2\beta-\alpha 
\end{array}\right)$

  - HKY model (Hasegawa, Kishino & Yano) tiež umožnuje rôzne
    pravdepodobnosti A, C, G a T v ekvilibriu.
  - Ak nastavíme čas v evolučnom modeli na nekonečno, nezáleží na tom, z
    ktorej bázy sme začali, frekvencia výskytu jednotlivých báz sa
    ustáli v tzv. ekvilibriu.
  - V Jukes-Cantorovom modeli je pravdepodobnosť ľubovoľnej bázy v
    ekvilibriu 1/4.
  - V HKY si zvolime aj frekvencie jednotlivých nukleotidov v ekvilibriu
    $\pi_A,\pi_C, \pi_G, \pi_T$ so súčtom 1
  - Parameter kapa: pomer tranzícií a transverzií (alfa/beta)
  - Matica rýchlostí:
      - $\mu_{x,y} =  \kappa \pi_y$ ak mutácia x-\>y je tranzícia,
      - $\pi_y$ ak mutácia x-\>y je transverzia

<!-- end list -->

  - Pre zložité modely nevieme odvodiť explicitný vzorec na výpočet
    S(t), ako sme mali pri Jukes-Cantorovom modeli
  - Ale vo všeobecnosti pre maticu rýchlostí $R$ dostávame
    $S(t)=e^{Rt}$.
      - Exponenciálna funkcia matice A sa definuje ako
        $e^A = \sum_{k=0}^\infty{1 \over k!}A^k.$
      - Ak maticu rychlosti R diagonalizujeme (určite sa dá pre
        symetrické R) $R = U D U^{-1}$, kde D je diagonálna matica (na
        jej diagonále budu vlastné hodnoty R), tak
        $e^{Rt} = U e^{Dt} U^{-1}$, t.j. exponenciálnu funkciu
        uplatníme iba na prvky na uhlopriečke matice *D*.
