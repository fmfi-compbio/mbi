---
title: "Cvičenia pre informatikov: Heuristické zarovnávanie sekvencií"
---


## Vzorec na výpočet senzitivity jadra

  - Uvažujme jadro dĺžky k (ako v programe BLAST pre nukleotidy, na
    prednáške sa dĺžka jadra označovala w, teraz k)
  - Uvažujme pravdepodobnostný model zarovnania, v ktorom má každá
    pozícia pravdepodobnosť p, že bude zhoda a (1-p), ze bude nezhoda
    alebo medzera, zarovnanie ma dlzku L
  - Nahodna premenna $X_i = 1$ ak na pozicii i je zhoda, 0 inak
  - Nahodna premenna $Y_i = 1$ ak na pozicii i zacina jadro, t.j. ak
    $X_i=1, X_{i+1}=1, \dots, X_{i+k-1}=1$
  - $P(Y_i = 1) = p^k$, nakolko $X_i$ su navzajom nezavisle
  - Nech $Y = \sum_{i=1}^{L-k-1} Y_i$
  - Z linearity strednej hodnoty vieme lahko odhadnut
    $E(Y) = (L-k+1)p^k$
  - Nas ale zaujima P(Y\>0) = 1-P(Y=0)
  - $P(Y=0) = P(Y_1=0 \wedge \dots \wedge Y_{L-k+1}=0)$
  - Preco neplati, $P(Y=0) = P(Y_i = 0)^{L-k+1}$?
      - Jednotlive $Y_i$ nie su nezavisle, napr. $P(Y_{i+1}=1\|Y_i=1)=p$
      - V postupnosti $Y_i$ sa jednotky maju tendenciu zhlukovat spolu
  - Pravdepodobnost nepritomnosti jadra $P(Y=0)$ ale vieme spocitat
    dynamickym programovanim
  - Nech $A\[n\]$ je pravdepodobnost nepritomnosti jadra v prvých $n$
    stlcoch zarovnania ($0\le n\le L$)
  - Budeme rozlisovat pripady podla toho, kolko je na konci $X_1,\dots,X_n$
    jednotiek
      - Tento pocet moze byt 0..k-1 (ak by bol $\ge k$, mali by sme vyskyt
        jadra)
  

$A\[n\] = \\left\\{\\begin{array}{ll} 1 & \\mbox{ak } n \< k\\\\
\\sum\_{i=0}^{k-1} p^i (1-p)A\[n-i-1\] & \\mbox{ak } n \\ge k\\\\
\\end{array}\\right.$

V druhom riadku $p^i(1-p)$ zodpoveda
$P(X_1...X_n\mbox{ konci presne }i\mbox{ jednotkami})$ a A\[n-i-1\] je
$P(X_1...X_{n-i-1}\mbox{ neobsahuje jadro})$, ale to je to iste ako
$P(X_1...X_n\mbox{ neobsahuje jadro }| X_1...X_n\mbox{ konci presne }i\mbox{ jednotkami})$

## Minimizery: ako ušetriť pamäť a čas

  - k-merom nazveme k za sebou iducich pismen v nejakom retazci
    (nukleotidov DNA)
  - Zakladne pouzite jadier: pri porovnavani dvoch sekvencii (alebo
    mnozin sekvencii) uloz vsetky k-mery jednej sekvencie do slovnika
    (napr. hash tabulky), potom prechadzaj vsetky k-mery druhej
    sekvencie a hladaj ich v slovniku

  - Priklad k=5,
      - DB vľavo, k-mery a ich pozície uložené do slovníka,
      - query vpravo, k-mery hľadané v slovníku, šípkou vyznačené
        nájdené k-mery (jadrá)


``` 
AGTGGCTGCCAGGCTGG    cGaGGCTGCCtGGtTGG  
AGTGG,0              CGAGG              
 GTGGC,1              GAGGC             
  TGGCT,2              AGGCT <-          
   GGCTG,3              GGCTG <-
    GCTGC,4              GCTGC <-          
     CTGCC,5              CTGCC <-       
      TGCCA,6              TGCCT        
       GCCAG,7              GCCTG        
        CCAGG,8              CCTGG
         CAGGC,9              CTGGT     
          AGGCT,10             TGGTT    
           GGCTG,11             GGTTG   
            GCTGG,12             GTTGG  
```

  - Trik na znizenie potrebnej pamate (napr. program BLAT): ukladaj iba
    kazdy s-ty k-mer z prvej sekvencie, potom hladaj vsetky k-mery z
    druhej
  - Trochu znizi aj senzitivitu, ale kedze jadra sa zhlukuju, mame sancu
    aspon jedno jadro zo zhluku najst
  - Zarucene najdeme jadro, ak mame aspon k+s-1 zhod za sebou
  - Mohli by sme v query tiež hľadať iba každý s-ty k-mer?
      - Čo ak by db a query boli to isté, iba v query by chýbalo prvé
        písmeno?

Priklad k=5, s=3, k-mery nalavo sa ulozia, k-mery napravo sa hladaju,
najde sa jedno jadro

``` 
AGTGGCTGCCAGGCTGG    cGaGGCTGCCtGGtTGG  
AGTGG,0              CGAGG              
   GGCTG,3            GAGGC             
      TGCCA,6          AGGCT
         CAGGC,9        GGCTG <-        
            GCTGG,12     GCTGC          
                          CTGCC         
                           TGCCT        
                            GCCTG       
                             CCTGG
                              CTGGT
                               TGGTT    
                                GGTTG   
                                 GTTGG  
```

  - Prefikanejsia idea je **minimizer**: uvazuj vsetky skupiny *s* po
    sebe iducich k-merov (sliding window), v kazdej skupine najdi
    abecedne najmensi k-mer (minimizer) a uloz do slovnika
  - Pri posune okna o jedno doprava casto najmensi k-mer zostava ten
    isty a netreba ho znovu ukladat, cim sa usetri pamat (a čas)
  - Rozdiel je pri hladani: v slovniku nehladame vsetky k-mery druhej
    sekvencie, ale tiez len minimizery, co moze usetrit cas
  - Zarucene najdeme jadro, ak mame aspon k+s-1 zhod za sebou
  - Priklad k=5, s=3, vlavo: do slovnika sa ulozia k-mery s cislom;
    vpravo: v slovniku sa hladaju k-mery s hviezdickou, najde sa jedna
    zhoda

``` 
AGTGGCTGCCAGGCTGG    cGaGGCTGCCtGGtTGG  
AGTGG,0              CGAGG              
 GTGGC                GAGGC             
  TGGCT                AGGCT*           
   GGCTG,3              GGCTG           
    GCTGC,4              GCTGC          
     CTGCC,5              CTGCC* <--    
      TGCCA                TGCCT        
       GCCAG                GCCTG       
        CCAGG,8              CCTGG*     
         CAGGC,9              CTGGT*    
          AGGCT,10             TGGTT    
           GGCTG                GGTTG*  
            GCTGG                GTTGG  
```

  - Obzvlast vyhodne ak prva a druha mnozina sekvencii su ta ista, napr.
    pri hladani prekryvov v citaniach pri skladani genomu. Kazde citanie
    ma mnozinu minimizerov, ktore sa pouziju ako kluce v slovniku,
    hodnoty su zoznamy citani. Dvojice citani zdielajuce nejaky
    minimizer (binning) sa dostanu do jedneho zoznamu a budu uvazovane
    pri vypocte vzajomneho prekryvu
  - V praxi sa do slovnika neuklada lexikograficky najmensi k-mer, ale
    kazdy k-mer sa prehasuje nejakou funkciou f a zoberie sa ten s
    minimalnou hodnotou
  - Dovod je, ze sa chceme vyhnut tomu, aby minimizermi boli casto sekvencie
    typu AAAAA, ktore su v biologickych sekvenciach nadreprezentovane a
    casto nie su funkcne zaujimave
  - Priemerna hustota minimizerov v sekvencii pri nahodnom hashovani je
    cca 2/(s+1)⁠ (v BLATe bola nizsia, 1/s)
  - Minimizery vyuziva napr. aj minimap2, velmi popularny nastroj na
    zarovnavanie citani navzajom a ku genomom
      - na zarovnanie nanoporovych citani ku genomu pouziva k=15, s=10,
        prekryvy v nanoporovych citaniach k=15, s=5, porovnanie genomov
        s identitou nad 80% k=19, s=10

Literatúra
  - Li, Heng. [Minimap and miniasm: fast mapping and de novo assembly
    for noisy long sequences.](https://academic.oup.com/bioinformatics/article-abstract/32/14/2103/1742895) Bioinformatics 32.14 (2016): 2103-2110.   
  - Roberts M, Hayes W, Hunt BR, Mount SM, Yorke JA. [Reducing storage
    requirements for biological sequence comparison.](https://academic.oup.com/bioinformatics/article/20/18/3363/202143) Bioinformatics.
    2004 Dec 12;20(18):3363-9.
  - Marçais, G., Pellow, D., Bork, D., Orenstein, Y., Shamir, R., &
    Kingsford, C. (2017). [Improving the performance of minimizers and
    winnowing schemes.](https://doi.org/10.1093/bioinformatics/btx235) Bioinformatics, 33(14), i110-i117.
    

## MinHash

### Odbočka do analýzy web-stránok: Podobnosť textov

Majme množinu webových stránok (webová stránka je postupnosť slov).
Chceme nájsť medzi nimi dvojice podobných. Ako môžeme definovať
podobnosť dvoch textov?

Jeden zo spôsobov ako to spraviť je pozrieť sa na počet slov
spoločných pre jednotlivé dvojice stránok. Očakávame, že čím viac
spoločných slov majú, tým sú podobnejšie. Túto mieru podobnosti
formalizuje matematický pojem Jaccardovej miery podobnosti.

Nech $U$ je univerzum slov a nech $A, B \subseteq U$ sú jeho
podmnožinami (t.j. množiny slov dvoch textov). Potom Jaccardova miera
podobnosti $J(A, B)$ je definovaná nasledovne:

$J(A, B) := \frac{|A \cap B|}{|A \cup B|}$

Táto miera nadobúda hodnotu 0 iba v prípade, ak množiny sú disjunktné, a
hodnotu 1 iba v prípade, že množiny sú totožné. Inak sa jej hodnota
nachádza na otvorenom intervale $(0, 1)$, a čím viac spoločných slov
majú, tým je jej hodnota vyššia.

Potom otázku "Ktoré dvojice textov sú podobné?" môžeme preformulovať
napríklad ako "Ktoré dvojice textov majú Jaccardovu mieru podobnosti
vyššiu ako nejaký prah $\alpha$?".

Ako rýchlo by sme vedeli spočítať Jaccardovu mieru pre dve množiny slov,
každú s $n$ prvkami?

Exaktný výpočet Jaccardovej miery podobnosti nie je vždy dostatočne
rýchly pre účely konkrétnej aplikácie, takže logickým riešením je
pokúsiť sa jej hodnotu vypočítať iba približne (t.j. aproximovať).

### Prvá idea: aproximácia vzorkovaním

  - Ak by sme vedeli vzorkovať z $A \cup B$ prvky $u_1, u_2, \dots, u_s$
    (rovnomerne, nezavisle), a pre kazdy prvok $u_i$ by sme vedeli rychlo
    zistit, ci patri do prieniku, mohli by sme odhadnut $J(A, B)$ pomocou
    nahodnej premennej $X$
  - $X = \frac{1}{n}\sum_{i=1}^s X_i$ kde $X_i=1$ ak $u_i$ patri do prieniku
a $X_i=0$ inak
  - Toto sa podoba na zistovanie oblubenosti politika prieskumom
    verejnej mienky, $u_1, u_2, \dots, u_s$ su "respondenti"
  -  Pre kazde $X_i$ mame $E(X_i) = 0 \cdot Pr[X_i = 0] + 1 \cdot Pr[X_i = 1] = Pr[X_i = 1] = J(A, B)$.
  - Z linearity strednej hodnoty $E(X) = E(\frac{1}{n}\sum_{i=1}^n X_i) = \frac{1}{n}\sum_{i=1}^n E[X_i] = J(A, B)$.
  - Z toho vyplýva, že náhodná premenná $X$ je nevychýleným odhadom Jaccardovej miery.
  - V štatistike základnou mierou kvality nevychýleného odhadu je jeho disperzia, odvodenie pozri nižšie
  - $Var(X) = \frac{J(A, B) - J^2(A, B)}{s} \le \frac{1}{4s}$
  - Pri zvyšujúcej veľkosti vzorky *s* teda klesá disperzia. Podobná situácia ako pri prieskumoch verejnej mierky, kde pri väčšom súbore respondentov dostaneme dôveryhodnejšie výsledky.

**Problémy:**

  - nie je ľahké rovnomerne vzorkovať z $A \cup B$
  - na zisťovanie, či $u_i$ je v prieniku potrebujeme mat reprezentaciu $A$
    a $B$ v pamati, co moze byt problem pri velkej kolekcii dokumentov
  - idea: chceme dostat nejake ine premenne $X_i$, ktore budu nezavisle a
    ich $E(X_i)=J(A,B)$ a ktore sa budu lahsie pocitat

### Aproximácia Jaccardovej miery: MinHash

Budeme mať náhodné hašovacie funkcie $h_1, h_2, \dots, h_s$.

O každej hašovacej funkcii predpokladáme, že ak ju použijeme na nejakú
množinu $A = \{a_1, a_2, \ldots, a_n\}$, tak
$h(a_1), h(a_2), \ldots, h(a_n)$ bude náhodná permutácia množiny
$A$.

Pre množinu $A = \{a_1, a_2, \ldots, a_n\}$ a hašovaciu funkciu *h* je
$minHash_{h}(A)$ je definovaný nasledovne:

$minHash_h(A) := \min\{h(a_1), h(a_2), \ldots, h(a_n)\}$

Keďže $h$ je náhodná hašovacia funkcia, tak sa na hodnotu
$minHash(A)$ môžeme pozerať ako na náhodnú premennú, ktorá
reprezentuje rovnomerne náhodný výber prvku z množiny $A$.

Nech $X_i$ je náhodná premenná, ktorá nadobúda hodnotu 1, ak
$minHash_{h_i}(A) = minHash_{h_i}(B)$, a inak hodnotu 0. Potom
$E[X_i] = J(A, B)$, lebo celkovo máme $|A\cup B|$ prvkov a
$J(A,B) = |A\cap B|/|A\cup B|$ značí, aké percento z nich je v
prieniku.

Takýmito hodnotami $X_i$ teda nahradíme náhodné vzorky diskutované
vyššie.

Algoritmus:

  - Pre kazdy dokument hasuj kazde slovo *s* funkciami, najdi minHash
    pre kazdu funkciu a uloz vektor tychto hodnot ako "sketch"
    dokumentu. Cas vypoctu pre dokument s *n* slovami je $O(ns)$
  - Pre dva dokumenty porovname vektor po zlozkach a ak najdeme $x$
    zhod, $J(A,B)$ odhadneme ako $x/s$. Cas vypoctu $O(s)$
  - Vieme si tiez pre kazdu hasovaciu funkciu spravit slovnik, ktory
    mapuje minHash do zoznamu dokumentov a budeme porovnavat iba dvojice
    dokumentov, ktore sa niekde dostali do toho isteho zoznamu (t.j ich
    odhad $J(A,B)$ bude nenulovy)

Alternativa: namiesto *s* roznych funkcii pouzijeme iba jednu a vezmeme
nielen minimum, ale *s* najmensich prvkov. Potom $J(A,B)$ odhadneme
pomocou $|S_A\cap S_B|/s$ kde $S_A$ je mnozina hodnot v sketchi
mnoziny $A$. To usetri cas pri vypocte sketchu, lebo nemusime hashovat
vsetky prvky *s* krat.

Literatúra
  - Broder AZ. [On the resemblance and containment of documents.
    Compression and Complexity of SEQUENCES](https://www.cs.princeton.edu/courses/archive/spring13/cos598C/broder97resemblance.pdf) 1997 (pp. 21-29). IEEE.
    

### Hľadanie podobných sekvencií

Ako "slová" použijeme všetky k-mery danej sekvencie. Potom na hľadanie
dvoch podobných sekvencií z množiny sekvencií môžeme použiť minhash.

  - Napríklad Mash používa k=21, s=1000 (s najmenších v jednej funkcii)
    na porovnávanie genómov, sketch má asi 8kb na genóm (genóm má
    milióny alebo miliardy nukleotidov)
  - Ondov BD, Treangen TJ, Melsted P, Mallonee AB, Bergman NH, Koren S,
    Phillippy AM. [Mash: fast genome and metagenome distance estimation
    using MinHash.](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0997-x) Genome Biology. 2016 Dec;17(1):1-4.
    

### Vypocet disperzie

Binarna premenna $X_i$, kde $P(X_i=1)=p$. Disperzia
$Var(X_i) = E(X_i^2) - (E(X_i))^2$. Spočítajme si postupne obe
hodnoty.

$E(X_i^2) = 0^2 \cdot Pr[X_i = 0] + 1^2 \cdot Pr[X_i = 1] = Pr[X_i = 1] = p$

$(E(X_i))^2 = p^2$

Čiže $Var(X_i) = p - p^2$. Aká je maximálna možná hodnota disperzie?

Táto otázka je ekvivalentná otázke "Aké je maximum funkcie
$f(x) = x - x^2$ na intervale $[0, 1]$?". Pre určenie extrémov
hladkých funkcií treba nájsť korene jej prvej derivácie. Derivácia
tejto funkcie je $f'(x) = 1 - 2x$, jej koreň je hodnota $0.5$.
Hodnota funkcie v tomto bode je rovná $0.25$. Čiže
$Var(X_i) \leq 0.25$.

$X =\frac{1}{s}\sum_{i=1}^s X_i$ kde $X_i$ su nezavisle premenne a
každá z nich má strednú hodnotu $E(X_i) = E(X) = p$ a rovnakú
disperziu $Var(X_i) = Var(X) = p - p^2$. Premenná $X$ je ich priemer.

Pozrieme sa na jej disperziu:
$Var(X) = Var\left(\frac{X_1+X_2+\ldots+X_s}{s}\right) = \frac{1}{k^2} Var(X_1+X_2+\ldots+X_s) \overset{*}{=} \frac{1}{s^2} [Var(X_1) + \ldots Var(X_s)] = \frac{1}{k^2} s \cdot Var(X_i) = \frac{Var(X_i)}{s} \leq \frac{1}{4s}$

*(\*) tento prechod je možný len kvôli tomu, že premenné $X_i$ sú
nezávislé.*

Vidíme teda, že disperziu (resp. kvalitu) môžeme potlačiť na ľubovoľne
malú postupným zvýšením počtu hashov.

Všimnite si, že premenná $s \cdot X$ (t.j. nie priemer, ale súčet
jednotlivých $X_i$) je súčtom nezávislých indikátorov s rovnakým
rozdelením, a teda má binomické rozdelenie s parametrami $s$ a
$p$.

