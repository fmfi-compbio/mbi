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

- Pravdepodobnosti prechodu medzi stavmi môžeme vyjadriť
  maticou $M$, ktorej prvok $M[x,y]$ označuje
  pravdepodobnosť prechodu zo stavu $x$ do stavu $y$, teda
  $M[x,y]=\Pr(X_t=y|X_{t-1}=x)$.
- Pre maticu musí platiť, že súčet každého riadku je 1, všetky prvky
  matice sú nezáporné.
- Nezávislé hody mincou sú tiež špeciálny prípad Markovovho reťazca. Ako vyzerá jeho matica?

Príklad:
```
0.767 0.233      0.067 0.933     0.30 0.70
0.100 0.900      0.400 0.600     0.30 0.70
Markovov         Markovov        Nezávislé
reťazec          reťazec         hody 
s predĺženými    so skrátenými   mincou
dobami           dobami          s Pr(X=1)=0.7
toho istého      toho istého
počasia          počasi
```

* Všetky tri matice generujú postupnosti s približne 70% jednotiek. 
* Priemerná dĺžka súvislého úseku núl: 4.3, 1.1, 1.4.
* Priemerné dĺžky súvislých úsekov jednotiek: 10, 2.5, 3.3. 

### Výpočet matice pre viac krokov naraz

- Pre reťazec s maticou $M$ spočítajme $\Pr(X_2=y \| X_0=x)$ (aké je rozdelenie počasia o dva dni vzhľadom na počasie dnes). Skúšame všetky hodnoty $y$ v čase 1 (zajtra). 
- Dostaneme $\sum_z \Pr(X_1=z \| X_0=x)\cdot\Pr(X_2=y \| X_1=z) = \sum_z M[x,z]\cdot M[z,y]$
- Ide o súčin matice $M$ samej so sebou, t.j. $M^2$.
- Podobne $\Pr(X_t=y \| X_0=x)$ získame ako políčko $M^t[x,y]$ z matice $M^t$ (počasie o $t$ dní neskôr vzhľadom na počasie dnes).
- Koľko trvá výpočet $M^t$ v závislosti od $t$ a počtu stavov $n$?

```
1.000 0.000   0.767 0.233   0.611 0.389   0.507 0.493  ...  0.312 0.688
0.000 1.000   0.100 0.900   0.167 0.833   0.211 0.789  ...  0.295 0.705
t=0           t=1           t=2           t=3               t=10
```

### Stacionárne rozdelenie

- Rozdelenie $\pi$ na množine stavov sa nazýva **stacionárne** pre
  Markovov reťazec $M$, ak pre každé $y$ platí
  $\sum_{x}\pi(x)M[x,y] = \pi(y)$ (alebo v maticovej notácii
  $\pi M = \pi$)
- Ak matica $M$ spĺňa určité podmienky (je ergodická), existuje pre ňu
  práve jedno stacionárne rozdelenie $\pi$. Navyše pre každé $x$ a $y$
  platí $\lim_{t\to\infty} M^{t}[x,y] = \pi(y)$.
- To znamená, že ak umocňujeme maticu $M$ na veľmi veľké čísla,  
  všetky riadky matice sa blížia k stacionárnemu rozdeleniu.
- Po veľa krokoch už teda príliš nezáleží, v ktorom stave sme začali (počasie o rok nie je príliš ovplyvnené tým, či dnes prší).
- Stacionárne rozdelenie sa nazýva aj ekvilibrium Markovovho reťazca.

### Ergodické Markovove reťazce

- Vravíme, že matica je **ergodická**, ak $P^t$ pre nejaké $t>0$ má všetky položky nenulové.
- Príklady neergodických a ergodických matíc:

```
    1 0          0.5 0.5          0 1             0.5 0.5
    0 1          0   1            1 0             1   0
    nesúvislá    slabo súvislá    periodická      ergodická
```


## Substitučné modely

- Nech $\Pr(a\stackrel{t}{\rightarrow} b)$ je pravdepodobnosť, že ak začneme s bázou *a*, tak
  po čase *t* budeme mať bázu *b*.
- Pre dané *t* môžeme také pravdepodobnosti usporiadať do matice $S(t)$ 4x4 (ak
  študujeme DNA):

$S(t) = \left(\begin{array}{cccc} 
\Pr(A\stackrel{t}{\rightarrow} A) & \Pr(A\stackrel{t}{\rightarrow} C) & \Pr(A\stackrel{t}{\rightarrow} G) & \Pr(A\stackrel{t}{\rightarrow} T) \\\\\\\\
\Pr(C\stackrel{t}{\rightarrow} A) & \Pr(C\stackrel{t}{\rightarrow} C) & \Pr(G\stackrel{t}{\rightarrow} G) & \Pr(C\stackrel{t}{\rightarrow} T) \\\\\\\\
\Pr(G\stackrel{t}{\rightarrow} A) & \Pr(C\stackrel{t}{\rightarrow} C) & \Pr(G\stackrel{t}{\rightarrow} G) & \Pr(G\stackrel{t}{\rightarrow} T) \\\\\\\\
\Pr(T\stackrel{t}{\rightarrow} A) & \Pr(C\stackrel{t}{\rightarrow} C) & \Pr(G\stackrel{t}{\rightarrow} G) & \Pr(T\stackrel{t}{\rightarrow} T) 
\end{array}\right)$

- Riadky zodpovedajú pôvodnej báze $a$, stĺpce novej báze $b$.
- Súčet každého riadku je 1.

### Diskrétny čas

- Ak uvažujeme iba celočíselné hodnoty času $t$, môžeme substitučný model vyjadriť pomocou Markovovho reťazca s nejakou maticou $M$, ktorá bude predstavovať $S(1)$.
- Maticu $S(t)$ dostaneme umocnením $M$, teda $S(t) = M^t$.

Príklad: za čas t=1 má každá zmena, napr. z A na C pravdepodobnosť 1% a žiadna zmena teda 97%. 

```
1 0 0 0   0.97 0.01 0.01 0.01  0.9412 0.0196 0.0196 0.0196
0 1 0 0   0.01 0.97 0.01 0.01  0.0196 0.9412 0.0196 0.0196
0 0 1 0   0.01 0.01 0.97 0.01  0.0196 0.0196 0.9412 0.0196
0 0 0 1   0.01 0.01 0.01 0.97  0.0196 0.0196 0.0196 0.9412
S(0)      S(1)                 S(2)

0.914 0.029 0.029 0.029  0.749 0.084 0.084 0.084  0.470 0.177 0.177 0.177
0.029 0.914 0.029 0.029  0.084 0.749 0.084 0.084  0.177 0.470 0.177 0.177
0.029 0.029 0.914 0.029  0.084 0.084 0.749 0.084  0.177 0.177 0.470 0.177
0.029 0.029 0.029 0.914  0.084 0.084 0.084 0.749  0.177 0.177 0.177 0.470
S(3)                     S(10)                    S(30)
```

* Pre malé hodnoty $t$ je pravdepodobnosť zmeny zhruba $t$ krát vyššia ako pre čas 1.
* Napr. za jednotku času sa 1% Ačok zmení na Cčka. Za ďalšiu jednotku času sa ďalších 1% zmení, ale z tých predchádzajúcich Cčok sa malá časť zmení na niečo iné, takže celkovo vidíme trochu menej ako 2% zmien z A na C.
* Ako sa $t$ zväčšuje, vzďalujeme sa od tohto lineárneho trendu kvôli tomu, že sa zvyšuje šanca, že nastalo aj viac mutácií a vidíme len jednu.
* Aké bude stacionárne rozdelenie pre túto maticu?


### Spojitý čas

- Pri štúdiu evolúcie sa zvyčajne uvažuje čas $t$ ako reálne číslo, čo má viaceré výhody (vieme napríklad pravdedpodobnosť určitej zmeny derivovať podľa $t$).
- Využijeme Markovov reťazec so spojitým časom (continuous-time Markov chain).
- Pre ľubovoľné $t$ chceme spočítať $S(t)$.
- Stále by malo platiť, že $S(t_1+t_2) = S(t_1)\cdot S(t_2)$.

### Jukesov-Cantorov substitučný model

- Tento model predpokladá, že všetky substitúcie sú rovnako pravdepodobné, matica teda musí vyzerať nejako takto:

$S(t) = 
\left(\begin{array}{cccc}
1-3s(t) & s(t) & s(t) & s(t) \\\\\\\\
s(t) & 1-3s(t) & s(t) & s(t) \\\\\\\\
s(t) & s(t) & 1-3s(t) & s(t) \\\\\\\\
s(t) & s(t) & s(t) & 1-3s(t) 
\end{array}\right)$


- Chceme odvodiť vzorec pre $s(t)$, ktorý sme videli na prednáške. Spočítajme deriváciu tejto funkcie.
- Z definície derivácie $s'(t) = \lim_{\epsilon\to 0} \frac{s(t+\epsilon)-s(t)}{\epsilon}$.
- $s(t+\epsilon)$ spočítame zo súčinu matíc $S(t+\epsilon) = S(t)S(\epsilon)$. 
- $\left(\begin{array}{cccc}
1-3s(t) & s(t) & s(t) & s(t) \\\\\\\\
s(t) & 1-3s(t) & s(t) & s(t) \\\\\\\\
s(t) & s(t) & 1-3s(t) & s(t) \\\\\\\\
s(t) & s(t) & s(t) & 1-3s(t) 
\end{array}\right)\cdot 
\left(\begin{array}{cccc}
1-3s(\epsilon) & s(\epsilon) & s(\epsilon) & s(\epsilon) \\\\\\\\
s(\epsilon) & 1-3s(\epsilon) & s(\epsilon) & s(\epsilon) \\\\\\\\
s(\epsilon) & s(\epsilon) & 1-3s(\epsilon) & s(\epsilon) \\\\\\\\
s(\epsilon) & s(\epsilon) & s(\epsilon) & 1-3s(\epsilon) 
\end{array}\right)$
- Dostaneme
  $s(t+\epsilon) = (1-3s(t))s(\epsilon) + s(t)(1-3s(\epsilon))+s(t)s(\epsilon)+s(t)s(\epsilon)$.
- Po úprave
  $s(t+\epsilon) = s(\epsilon) +s(t) - 4s(t)s(\epsilon) = s(t)+s(\epsilon)(1-4s(t))$.
- $s'(t) = \lim_{\epsilon\to 0} \frac{s(\epsilon) (1-4s(t))}{\epsilon} = (1-4s(t))\lim_{\epsilon\to 0} \frac{s(\epsilon)}{\epsilon}= (1-4s(t))s'(0)$.
- Označme $\alpha = s'(0)$ ($\alpha$ je konštanta, nezávisí od $\epsilon$ ani $t$).
- Dostaneme $s'(t) = \alpha (1-4s(t))$. Toto je diferenciálna rovnica, hľadáme funkciu $s(t)$, ktorá ju spĺňa.
- Riešenie je $s(t) = 1/4+c e^{-4\alpha t}$ pre každú konštantu $c$.
- Môžeme overiť dosadením do rovnice, pričom
  $s'(t) = -4 c \alpha e^{-4\alpha t}$.
- Hodnotou $c=-1/4$ dopočítame z počiatočnej podmienky $s(0)=0$.
- Overíme tiež, že $s'(0)=\alpha$.


### Vlastnosti riešenia

- Takže máme maticu:
$S(t)=
\left(\begin{array}{cccc}
(1+3e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 \\\\\\\\
(1- e^{-4\alpha t})/4 & (1+3e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 \\\\\\\\
(1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1+3e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 \\\\\\\\
(1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1+3e^{-4\alpha t})/4
\end{array}\right)$

- Keď $t\rightarrow \infty$, dostávame všetky prvky matice rovné 1/4, t.j. $\lim_{t\to \infty}s(t)=\lim_{t\to \infty}1-3s(t)=1/4$.
- $\alpha=s'(0)$ vyjadruje rýchlosť ako sa hromadia mutácie. Je to pravdepodobnosť konkrétnej zmeny za jednotku času, ak uvažujeme veľmi krátke časy.
- Modely zvykneme normalizovať tak, aby priemerný počet substitúcii za jednotku času bol 1. V prípade Jukes-Cantorovho modelu je to keď $\alpha=1/3$.


### Matica rýchlostí  

- Model substitúcií väčšinou vyjadríme maticou rýchlostí (intenzít) (transition rate matrix, substitution rate matrix).
- Je to matica $R$ 4x4, kde $R[x,y]$ pre $x\ne y$ je rýchlosť, ako sa deje zmena z $x$ na $y$.
- Presnejšie $R[x,y] = \\lim_{t\\rightarrow 0}\\frac{\Pr(x\\stackrel{t}{\rightarrow} y)}{t}$.
- Diagonálne prvky $R[x,x]$ sa dopočítajú tak, aby súčet riadku bol vždy 0, je to teda rýchlosť, akou báza $x$ mutáciami ubúda.
- Pre J-C model máme maticu rýchlostí
$R=
\left(\begin{array}{cccc}
-3\alpha & \alpha & \alpha & \alpha \\\\\\\\
\alpha & -3\alpha & \alpha & \alpha \\\\\\\\
\alpha & \alpha & -3\alpha & \alpha \\\\\\\\
\alpha & \alpha & \alpha & -3\alpha
\end{array}\right)$

- Pre veľmi malé časy platí $S(t)\approx I+Rt$.
- $S(t+\epsilon) = S(t)S(\epsilon) \approx S(t)(I+R\epsilon)$ a teda $(S(t+\epsilon)-S(t))/\epsilon \approx S(t)R$
- V limite dostaneme $S(t)R = \lim_{\epsilon\rightarrow 0} (S(t+\epsilon)-S(t))/\epsilon = S'(t)$
- Dostali sme diferenciálnu rovnicu $S(t)R = S'(t)$, počiatočný stav $S(0)=I$.
- Ak by sme dosadili $R$ pre J-C model, dostali by sme rovnakú diferenciálnu rovnicu ako predtým.

## Substitučné modely, zhrnutie

- S(t): matica 4x4, kde políčko $S(t)_{a,b}=\Pr(a\stackrel{t}{\rightarrow} b)$ je
  pravdepodobnosť, že ak začneme s bázou a, tak po čase t budeme mať
  bázu b.
- Jukes-Cantorov model predpokladá, že táto pravdepodobnosť je rovnaká
  pre každé dve bázy $a\ne b$
- Pre daný čas t máme teda všade mimo diagonály s(t) a na diagonále
  1-3s(t)
- Matica rýchlostí R: pre Jukes-Cantorov model všade mimo diagonály
  $\alpha$, na diagonále $-3\alpha$
- Pre veľmi malý čas t je S(t) zhruba I-Rt
- Rýchlost $\alpha$ je teda pravdepodobnosť zmeny za jednotku casu, ak
  uvažujeme veľmi krátke časy, resp. derivácia *s(t)* vzhľadom na *t*
  v bode 0.
- Riešením diferenciálnych rovníc pre Jukes-Cantorov model dostávame
  $s(t) = (1-e^{-4\alpha t})/4$
- Matica rýchlostí sa zvykne normalizovať tak, aby na jednotku času
  pripadla v priemere jedna substitúcia, čo dosiahneme ak
  $\alpha=1/3$.

## Použitie na odhad evolučnej vzdialenosti

- V čase $t$ je pravdepodobnosť, že uvidíme zmenenú bázu
  $D(t) = \frac{3}{4}(1-e^{-4\alpha t})$
- V reálnom použití (výpočet matice vzdialenosti pre metódu spájania
  susedov) máme dve zarovnané sekvencie, medzi ktorými vidíme $d\%$
  zmenených báz, chceme odhadnúť $t$
    - Spätne teda zrátame $t$, ktoré by hodnote $D(t)=d$ prináležalo.
- Dostávame teda vzorec pre vzdialenosť, ktorý sme videli na prednáške
  $t=-\frac{3}{4} \log\left(1-\frac{4}{3}d\right)$
- Ak $d\rightarrow 0.75$, dostávame $t\rightarrow \infty$
- Prečo sme ten vzorec odvodili takto? V skutočnosti chceme nájsť
  najvierohodnejšiu hodnotu $t$, t.j. takú, pre ktorú hodnota $\Pr(data|t)$
  bude najväčšia. Zhodou okolností vyjde takto.

## Zložitejšie substitučné modely

- Vo všeobecnejších modeloch sa rôzne typy mutácií dejú rôzne rýchlo, teda za čas $t$ môžeme napríklad dostať inú pravdepodobnosť A->C než A->T. 
- Máme teda všeobecnejšiu maticu rýchlostí R
- $R = \\left( \\begin{array}{cccc}
  -\\mu_A & \\mu\_{AC} & \\mu\_{AG} & \\mu\_{AT}\\\\\\\\
  \\mu\_{CA} & -\\mu_C & \\mu\_{CG} & \\mu\_{CT}\\\\\\\\
  \\mu\_{GA} & \\mu\_{GC} & -\\mu_G & \\mu\_{GT}\\\\\\\\
  \\mu\_{TA} & \\mu\_{TC} & \\mu\_{TG} & -\\mu_T
  \\end{array} \\right)$
- Hodnota $\mu_{xy}$ teda označuje rýchlosť, akou sa
    určitá báza x mení na inú bázu y.
- Ako sme videli, diagonála sa dopočíta tak, aby súčet každého riadku bol 0, t.j. $\mu_x = \sum_{y\ne x}\mu_{xy}$
- Často sa používajú modely, ktoré všetky potrebné rýchlosti spočítajú z menšieho počtu parametrov.

Model **K80** (Kimura 1980) napr. zachytáva, ze puríny sa častejšie menia na iné
puríny (A a G) a pyrimidíny na ine pyrimidíny (C a T).
- Tranzícia je zmena v rámci skupiny (C<->T, A<->G), transverzia je zmena medzi skupinami {C, T } <-> {A, G}.
- Model má dva parametre: rýchlosť tranzícií $\alpha$, transverzií $\beta$.
$R=\left(\begin{array}{cccc}
-2\beta-\alpha & \beta & \alpha & \beta \\\\\\\\
\beta & -2\beta-\alpha & \beta & \alpha \\\\\\\\
\alpha & \beta & -2\beta-\alpha & \beta \\\\\\\\
\beta & \alpha & \beta & -2\beta-\alpha 
\end{array}\right)$

Model **HKY85** (Hasegawa, Kishino & Yano, 1985) tiež umožnuje rôzne pravdepodobnosti A, C, G a T v ekvilibriu.
- Ak nastavíme čas v evolučnom modeli na nekonečno, nezáleží na tom, z ktorej bázy sme začali, frekvencia výskytu jednotlivých báz sa ustáli v stacionárnom rozdelení (ekvilibriu).
- V modeli J-C aj K80 je pravdepodobnosť ľubovoľnej bázy v ekvilibriu 1/4.
- V HKY85 si zvolíme aj frekvencie jednotlivých nukleotidov v ekvilibriu $\pi_A,\pi_C, \pi_G, \pi_T$ so súčtom 1.
- Parameter $\kappa$: pomer tranzícií a transverzií ($\alfa/\beta$)
- Matica rýchlostí:
  - $\mu_{x,y} =  \kappa \pi_y$ ak mutácia x-\>y je tranzícia,
  - $\pi_y$ ak mutácia x-\>y je transverzia

### Výpočet pravdepodobností z matice rýchlostí

- Pre zložité modely nevieme odvodiť explicitný vzorec na výpočet $S(t)$, ako sme mali pri Jukesovom-Cantorovom modeli
- Ale vo všeobecnosti pre maticu rýchlostí $R$ dostávame $S(t)=e^{Rt}$.
  - Exponenciálna funkcia matice $A$ sa definuje ako
    $e^A = \sum_{k=0}^\infty{1 \over k!}A^k.$
  - Ak maticu rýchlostí R diagonalizujeme (určite sa dá pre
    symetrické R) $R = U D U^{-1}$, kde $D$ je diagonálna matica (na
    jej diagonále budú vlastné hodnoty $R$), tak
    $e^{Rt} = U e^{Dt} U^{-1}$, t.j. exponenciálnu funkciu
    uplatníme iba na prvky na uhlopriečke matice $D$.
