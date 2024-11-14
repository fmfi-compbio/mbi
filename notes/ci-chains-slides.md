---
title: "Cvičenia pre informatikov: Substitučné modely"
---

* TOC
{:toc}

```



```

## Markovove reťazce (Markov chains)

- Markovov reťazec sa podobá na skrytý Markovov
  model (HMM), ale obsahuje iba stavy a pravdepodobnosti prechodu
  medzi nimi, neuvažujeme žiadne emisie

```















```
## Markovove reťazce (Markov chains), formálne

**Markovov reťazec** je postupnosť náhodných premenných
  $X_0, X_1, \dots,$ taká, že<br/> 
  $\Pr(X_t|X_0,\dots,X_{t-1}) = \Pr(X_t|X_{t-1})$,<br/> 
  t.j. stav v čase $t$ závisí len od stavu v čase
  $t-1$ a nie ďalších predchádzajúcich stavov 
  
  Stavy: možné hodnoty $X_{t}$.

- Nás budú zaujímať **homogénne** Markovove reťazce, v ktorých
  $\Pr(X_t|X_{t-1})$ nezávisí od $t$.
- Tiež nás zaujímajú len reťazce, v ktorých náhodné premenné $X_t$
  nadobúdajú hodnoty z konečnej množiny.

```














```
  
## Matica pravdepodobností prechodu

$M[x,y]=\Pr(X_t=y|X_{t-1}=x)$<br/>
pravdepodobnosť prechodu zo stavu $x$ do stavu $y$

Čo by mala matica $M$ spĺňať?

```















```

## Príklad

```
0.767 0.233      0.067 0.933
0.100 0.900      0.400 0.600
```

1. Ako sa budú líšiť postupnosti generované týmito dvomi maticami?
2. Nezávislé hody mincou sú tiež špeciálny prípad Markovovho reťazca. Ako vyzerá jeho matica?

```















```

## Príklad

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

```














```

## Výpočet matice pre viac krokov naraz

Pre reťazec s maticou $M$ spočítajme $\Pr(X_2=y \| X_0=x)$<br/> 
(aké je rozdelenie počasia o dva dni vzhľadom na počasie dnes).


```














```

## Výpočet matice pre viac krokov naraz

$\Pr(X_{t_0+t}=y \| X_{t_0}=x)$: políčko $M^t[x,y]$ z matice $M^t$

Koľko trvá výpočet $M^t$ v závislosti od $t$ a počtu stavov $n$?


```














```

## Príklad

```
1.000 0.000   0.767 0.233   0.611 0.389   0.507 0.493  ...  0.312 0.688
0.000 1.000   0.100 0.900   0.167 0.833   0.211 0.789  ...  0.295 0.705
t=0           t=1           t=2           t=3               t=10
```


```














```


## Stacionárne rozdelenie

Rozdelenie $\pi$ na množine stavov sa nazýva **stacionárne** pre 
Markovov reťazec $M$, ak pre každé $y$ platí<br>
$\sum_{x}\pi(x)M[x,y] = \pi(y)$<br> 
alebo v maticovej notácii $\pi M = \pi$

- Ak matica $M$ spĺňa určité podmienky (je ergodická), existuje pre ňu
  práve jedno stacionárne rozdelenie $\pi$<br/> 
  Navyše pre každé $x$ a $y$
  platí<br> $\lim_{t\to\infty} M^{t}[x,y] = \pi(y)$
- Ak umocňujeme maticu $M$ na veľké $t$,  
  všetky riadky sa blížia k stacionárnemu rozdeleniu
- Stacionárne rozdelenie sa nazýva aj ekvilibrium Markovovho reťazca

```














```

## Príklad

```
1.000 0.000   0.767 0.233   0.611 0.389   0.507 0.493  ...  0.312 0.688
0.000 1.000   0.100 0.900   0.167 0.833   0.211 0.789  ...  0.295 0.705
t=0           t=1           t=2           t=3               t=10
```


```














```

## Ergodické Markovove reťazce

Matica je **ergodická**, ak $P^t$ pre nejaké $t>0$ má všetky položky nenulové

```
    1 0          0.5 0.5          0 1             0.5 0.5
    0 1          0   1            1 0             1   0
    nesúvislá    slabo súvislá    periodická      ergodická
```

Ak matica $M$ spĺňa je ergodická, existuje pre ňu práve jedno stacionárne rozdelenie $\pi$. Navyše pre každé $x$ a $y$ platí $\lim_{t\to\infty} M^{t}[x,y] = \pi(y)$.

```














```

## Substitučné modely

Pravdepodobnosť, že ak začneme s bázou *a*, tak po čase *t* budeme mať bázu *b*<br>
$\Pr(X_{t_0+t}=b \| X_{t_0}=a)$<br>
Označíme $\Pr(a\stackrel{t}{\rightarrow} b)$

Matica $S(t)$:

$S(t) = \left(\begin{array}{cccc} 
\Pr(A\stackrel{t}{\rightarrow} A) & \Pr(A\stackrel{t}{\rightarrow} C) & \Pr(A\stackrel{t}{\rightarrow} G) & \Pr(A\stackrel{t}{\rightarrow} T) \\\\\\\\
\Pr(C\stackrel{t}{\rightarrow} A) & \Pr(C\stackrel{t}{\rightarrow} C) & \Pr(C\stackrel{t}{\rightarrow} G) & \Pr(C\stackrel{t}{\rightarrow} T) \\\\\\\\
\Pr(G\stackrel{t}{\rightarrow} A) & \Pr(G\stackrel{t}{\rightarrow} C) & \Pr(G\stackrel{t}{\rightarrow} G) & \Pr(G\stackrel{t}{\rightarrow} T) \\\\\\\\
\Pr(T\stackrel{t}{\rightarrow} A) & \Pr(T\stackrel{t}{\rightarrow} C) & \Pr(T\stackrel{t}{\rightarrow} G) & \Pr(T\stackrel{t}{\rightarrow} T) 
\end{array}\right)$


```














```


## Diskrétny čas

Uvažujme teraz iba celočíselné hodnoty času $t$

Substitučný model môžeme vyjadriť ako Markovov reťazec s nejakou maticou $M=S(1)$.

$S(t) = $?

```














```

## Príklad

Za čas t=1 má každá zmena pravdepodobnosť 1% a žiadna zmena 97% 

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

```














```

## Príklad

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

* Ako sa mení napr. $S(t)[A,C]$ s $t$? 
* Aké bude stacionárne rozdelenie pre túto maticu?


```














```


## Spojitý čas

- Pri štúdiu evolúcie sa zvyčajne uvažuje čas $t$ ako reálne číslo
- Vieme potom napríklad pravdepodobnosť určitej zmeny derivovať podľa $t$
- Využijeme **Markovov reťazec so spojitým časom (continuous-time Markov chain)**
- Pre ľubovoľné $t$ chceme spočítať $S(t)$
- Stále by malo platiť, že $S(t_1+t_2) = S(t_1)\cdot S(t_2)$


```














```


## Jukesov-Cantorov substitučný model

Tento model predpokladá, že všetky substitúcie sú rovnako pravdepodobné.

Matica teda musí vyzerať nejako takto:

$S(t) = 
\left(\begin{array}{cccc}
1-3s(t) & s(t) & s(t) & s(t) \\\\\\\\
s(t) & 1-3s(t) & s(t) & s(t) \\\\\\\\
s(t) & s(t) & 1-3s(t) & s(t) \\\\\\\\
s(t) & s(t) & s(t) & 1-3s(t) 
\end{array}\right)$


```














```

## Jukesov-Cantorov substitučný model

$S(t) = 
\left(\begin{array}{cccc}
1-3s(t) & s(t) & s(t) & s(t) \\\\\\\\
s(t) & 1-3s(t) & s(t) & s(t) \\\\\\\\
s(t) & s(t) & 1-3s(t) & s(t) \\\\\\\\
s(t) & s(t) & s(t) & 1-3s(t) 
\end{array}\right)$

- Chceme odvodiť vzorec pre $s(t)$ z prednášky
- Spočítajme deriváciu tejto funkcie
- $s'(t) = \lim_{\epsilon\to 0} \frac{s(t+\epsilon)-s(t)}{\epsilon}$


```














```

$S(t+\epsilon) = S(t)S(\epsilon)$

$\left(\begin{array}{cccc}
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


```














```

$s'(t) = \lim_{\epsilon\to 0} \frac{s(t+\epsilon)-s(t)}{\epsilon} = (1-4s(t))s'(0)$

Označme $\alpha = s'(0)$

Dostaneme $s'(t) = \alpha (1-4s(t))$

Diferenciálna rovnica, hľadáme funkciu $s(t)$, ktorá ju spĺňa.


```














```

Diferenciálna rovnica $s'(t) = \alpha (1-4s(t))$<br/>
kde $\alpha = s'(0)$

Riešenie rovnice je $s(t) = 1/4+c e^{-4\alpha t}$ pre každú konštantu $c$

Overíme
- $s'(t) = -4 c \alpha e^{-4\alpha t}$
- Dostadíme do rovnice
- Hodnotou $c=-1/4$ dopočítame z počiatočnej podmienky $s(0)=0$
- Overíme, že $s'(0)=\alpha$


```














```


## Vlastnosti riešenia

$s(t) = 1/4-1/4 e^{-4\alpha t}$<br>
$1-3s(t) = $?

$S(t)=
\left(\begin{array}{cccc}
(1+3e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 \\\\\\\\
(1- e^{-4\alpha t})/4 & (1+3e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 \\\\\\\\
(1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1+3e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 \\\\\\\\
(1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1+3e^{-4\alpha t})/4
\end{array}\right)$

Čo sa deje pre $t\rightarrow \infty$? (Stacionárne rozdelenie)


```














```

## Vlastnosti riešenia

$S(t)=
\left(\begin{array}{cccc}
(1+3e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 \\\\\\\\
(1- e^{-4\alpha t})/4 & (1+3e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 \\\\\\\\
(1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1+3e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 \\\\\\\\
(1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1+3e^{-4\alpha t})/4
\end{array}\right)$


$\alpha=s'(0) = \lim_{\epsilon\to 0} s(\epsilon) / \epsilon$<br>
Rýchlosť ako sa hromadia mutácie<br> 
Pravdepodobnosť konkrétnej zmeny za jednotku času, ak uvažujeme veľmi krátke časy

Modely zvykneme normalizovať tak, aby priemerný počet substitúcii za jednotku času bol 1<br/> 
Pre J-C model $\alpha=1/3$


## Matica rýchlostí  (intenzít) (transition rate matrix, substitution rate matrix)

- $R[x,y]$ pre $x\ne y$ je rýchlosť, ako sa deje zmena z $x$ na $y$
- $R[x,y] = \\lim_{t\\rightarrow 0}\\frac{\Pr(x\\stackrel{t}{\rightarrow} y)}{t}$
- $R[x,x]$ dopočítame tak, aby súčet riadku bol vždy 0<br> 
Rýchlosť, akou báza $x$ mutáciami ubúda
- Pre J-C model:

$R=
\left(\begin{array}{cccc}
-3\alpha & \alpha & \alpha & \alpha \\\\\\\\
\alpha & -3\alpha & \alpha & \alpha \\\\\\\\
\alpha & \alpha & -3\alpha & \alpha \\\\\\\\
\alpha & \alpha & \alpha & -3\alpha
\end{array}\right)$

Pre veľmi malé časy: $S(t)\approx I+Rt$

```














```


- Pre veľmi malé časy: $S(t)\approx I+Rt$
- Diferenciálna rovnica $S'(t) = S(t)R$
- Počiatočný stav $S(0)=I$

```














```


## Substitučné modely, zhrnutie

- Matica $S(t)$, kde $S(t)[a,b]=\Pr(a\stackrel{t}{\rightarrow} b)$ je
  pravdepodobnosť, že ak začneme s bázou $a$, tak po čase $t$ budeme mať
  bázu $b$
- Jukes-Cantorov model predpokladá, že táto pravdepodobnosť je rovnaká
  pre každé dve bázy $a\ne b$
- Všade mimo diagonály $s(t)$ a na diagonále
  $1-3s(t)$
- Matica rýchlostí R: pre Jukes-Cantorov model všade mimo diagonály
  $\alpha$, na diagonále $-3\alpha$
- Pre veľmi malý čas t je S(t) zhruba I-Rt
- Rýchlost $\alpha$ je teda pravdepodobnosť zmeny za jednotku casu, ak
  uvažujeme veľmi krátke časy, resp. derivácia *s(t)* vzhľadom na *t*
  v bode 0
- Riešením diferenciálnych rovníc pre Jukes-Cantorov model dostávame
  $s(t) = (1-e^{-4\alpha t})/4$
- Matica rýchlostí sa zvykne normalizovať tak, aby na jednotku času
  pripadla v priemere jedna substitúcia, čo dosiahneme ak
  $\alpha=1/3$

```














```

## Použitie na odhad evolučnej vzdialenosti

- V čase $t$ je pravdepodobnosť, že uvidíme zmenenú bázu
  $D(t) = 3s(t) = \frac{3}{4}(1-e^{-4\alpha t})$
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

```














```

## Zložitejšie substitučné modely

Rôzne typy mutácií môžu diať rôzne rýchlo

Všeobecnejšia maticu rýchlostí R

$R = \\left( \\begin{array}{cccc}
  -\\mu_A & \\mu\_{AC} & \\mu\_{AG} & \\mu\_{AT}\\\\\\\\
  \\mu\_{CA} & -\\mu_C & \\mu\_{CG} & \\mu\_{CT}\\\\\\\\
  \\mu\_{GA} & \\mu\_{GC} & -\\mu_G & \\mu\_{GT}\\\\\\\\
  \\mu\_{TA} & \\mu\_{TC} & \\mu\_{TG} & -\\mu_T
  \\end{array} \\right)$

$\mu_{xy}$: rýchlosť zmeny bázy x na inú bázu y

$\mu_x = \sum_{y\ne x}\mu_{xy}$

```














```



## Model K80 (Kimura 1980) 

Zachytáva, ze puríny sa častejšie menia na iné
puríny (A a G) a pyrimidíny na ine pyrimidíny (C a T)
- Tranzícia je zmena v rámci skupiny (C<->T, A<->G) 
- Transverzia je zmena medzi skupinami {C, T } <-> {A, G}.

Model má dva parametre: rýchlosť tranzícií $\alpha$, transverzií $\beta$

$R=\left(\begin{array}{cccc}
-2\beta-\alpha & \beta & \alpha & \beta \\\\\\\\
\beta & -2\beta-\alpha & \beta & \alpha \\\\\\\\
\alpha & \beta & -2\beta-\alpha & \beta \\\\\\\\
\beta & \alpha & \beta & -2\beta-\alpha 
\end{array}\right)$

```














```

##  Model **HKY85** (Hasegawa, Kishino & Yano, 1985) 

Umožňuje aj rôzne pravdepodobnosti A, C, G a T v ekvilibriu (pre $t\to\infty$).

- V modeli J-C aj K80 je pravdepodobnosť ľubovoľnej bázy v ekvilibriu 1/4
- V HKY85 si zvolíme aj frekvencie v ekvilibriu $\pi_A,\pi_C, \pi_G, \pi_T$ so súčtom 1.
- Parameter $\kappa$: pomer tranzícií a transverzií ($\alpha/\beta$)
- Matica rýchlostí:
  - $\mu_{x,y} =  \kappa \pi_y$ ak mutácia x-\>y je tranzícia,
  - $\pi_y$ ak mutácia x-\>y je transverzia

```














```

## Výpočet pravdepodobností z matice rýchlostí

- Pre zložité modely nevieme odvodiť explicitný vzorec na výpočet $S(t)$ ako v J-C modeli
- Ale vo všeobecnosti platí<br> $S(t)=e^{Rt}$
- Exponenciálna funkcia matice $A$ sa definuje ako<br> $e^A = \sum_{k=0}^\infty{1 \over k!}A^k$
- Ak R diagonalizujeme (určite sa dá pre
  symetrické R) $R = U D U^{-1}$, kde $D$ je diagonálna matica (na
  jej diagonále budú vlastné hodnoty $R$), tak
  $e^{Rt} = U e^{Dt} U^{-1}$, t.j. exponenciálnu funkciu
  uplatníme iba na prvky na uhlopriečke matice $D$.
- Všimnime si prvé dva členy rozvoja $S(t) = e^{Rt} = \sum_{k=0}^\infty{1 \over k!}(Rt)^k$

```














```
