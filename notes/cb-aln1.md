---
title: "Cvičenia pre biológov: Zarovnávanie sekvencií"
---

* TOC
{:toc}

## Zarovnávanie sekvencií, opakovanie

  - Uvažujme skórovanie zhoda +3, nezhoda -1, medzera -2
  - Reťazce TAACGG a CACACT

Globálne zarovnanie

Rekurencia: $A[i,j] = \max \{A[i-1,j]-2, A[i,j-1]-2, A[i-1,j-1]+s(x_i, y_j) \}$, pričom $A[0,i]=-2i$, $A[i,0]=-2i$


``` 
        C   A   C   A   C   T
    0  -2  -4  -6  -8  -10  -12
T  -2  
A  -4  
A  -6  
C  -8  
G  -10  
G  -12  
```

Lokálne zarovnanie

Rekurencia: $A[i,j] = \max \{0, A[i-1,j]-2, A[i,j-1]-2, A[i-1,j-1]+s(x_i, y_j) \}, pričom $A[0,i]=0$, $A[i,0]=0$


``` 
        C   A   C   A   C   T
    0   0   0   0   0   0   0
T   0 
A   0 
A   0 
C   0 
G   0 
G   0 
```

## Dotploty

  - Dotplot je graf, ktory ma na kazdej osi jednu sekvenciu a ciarky
    zobrazuju lokalne zarovnania (cesty v matici)
  - Niekoľko príkladov dotplotov: 
  - Prvé príklady dotplotov porovnávajú rôzne mitochondriálne genomy
  - Tieto boli vytvorene pomocou nastroja YASS
    <https://bioinfo.lifl.fr/yass/yass.php>
  - Dalsi priklad je zarovnanie genu Oaz Drosophila zinc finger s
    genomickym usekom chr2R:10,346,241-10,352,965
  - Trochu iny dotplot, ktory funguje pre proteiny a nerobi lokalne
    zarovnania, iba spocita skore bez medzier v kazdom okne danej vysky
    a nakresli ciaru ak prekroci urcenu hodnotu
  - <https://emboss.bioinformatics.nl/cgi-bin/emboss/dotmatcher>
  - Vyskusame protein escargot voci sebe s hodnotami
    <https://pfam.xfam.org/protein/ESCA_DROME> window 8 threshold 24
  - Pomocou YASSu vyskusame kluster zhlukov PRAME z ludskeho genomu

**Dáta:**

Mitochondriálne genómy 
- Človek *Homo sapiens* <https://www.ncbi.nlm.nih.gov/nuccore/NC_012920.1/>
- Rybička *Danio rerio* <https://www.ncbi.nlm.nih.gov/nuccore/NC_002333.2/>
- Muška *Drosophila melanogaster* <https://www.ncbi.nlm.nih.gov/nuccore/NC_024511.2/>


## Dynamické programovanie v Exceli

### Práca so vzorcami v tabuľkovom procesore (Excel, LibreOffice, ...)

  - Okrem konkrétnych hodnôt, napr. 0.3, môžu byť aj vzorce, ktoré
    začínajú `=`, napr `=0.3*0.3` dá do políčka 0.09 (`*` znamená
    násobenie)
  - Vo vzorcoch môžeme používať aj hodnoty z iných políčok, napr. `=A2+B2`
    dáme do políčka C2, zobrazí sa tam súčet
  - Ak políčko so vzorcom skopírujeme do iného políčka, Excel sa snaží
    uhádnuť, ako zmeniť vzorec
      - Ak sme v C2 mali `=A2+B2` a skopírovali sme to do C3, vzorec sa
        zmení na `=A3+B3`
  - Ak niektoré adresy políčok majú zostávať rovnaké aj pri kopírovaní,
    dáme pred písmeno aj číslo `$`,
      - Ak v C2 máme `=A2+$B$2` a skopírujeme to do C3, dostaneme `=A3+B2`
  - Dolár môžeme dať aj pred iba jednu súradnicu (stĺpec alebo riadok),
    tá sa potom nebude pri kopírovaní meniť

### Späť k rozmieňaniu mincí

  - Vráťme sa k príkladu s rozmieňaním mincí a skúsme si ho
    "naprogramovať" v Exceli, resp. spreadsheet aplikácii v OpenOffice
  - Vseobecna formulacia:
      - Vstup: hodnoty k minci $m_1,m_2,\dots ,m_k$ a cielova suma $X$
        (vsetko kladne cele cisla)
      - Vystup: najmensi pocet minci, ktore potrebujeme na zaplatenie $X$
  - My pouzijeme mince hodnot 1,3,4
  - Spravime si tabulku, kde si pre kazdu sumu $i=0,1,2,\dots,X$ pamatame
    $A[i]=$najmensi pocet minci, ktore treba na vyplatenie sumy $i$

```
  i     0    1    2    3    4    5    6    7    8    9  
A[i]    0    1    2    1    1    2    2    2    2    3
```

  - vzorec $A[i] = \min \{ A[i-1]+1, A[i-3]+1, A[i-4]+1 \}$
  - aby sme nemuseli zvlast uvazovat hodnoty mensie ako 4, (kde sa neda
    $A[i-4]$), urcime si $A[-1]$, $A[-2]$ atd ako nejake velke cislo
    (napr 100), takze vzorec plati pre vsetky $i>0$


```
  i   -4  -3  -2  -1  0    1    2    3    4    5    6    7    8    9  
A[i] 100 100 100 100  0    1    2    1    1    2    2    2    2    3
```

  - v exceli si najskor spravime horny riadok tabulky
      - do nejakeho policka (napr, B4) zapiseme prvu hodnotu (`-4`)
      - do susedneho C4 zapiseme vzorec `=B4+1`, dostaneme hodnotu -3
          - vzorce zacinaju znamienkom `=`
          - B4 je suradnica policka o jedno vlavo, k nej pripocitame 1
      - policko C4 nakopirujeme do riadku kolkokrat chceme, dostaneme
        hodnoty -2, -1, 0, 1,...
          - kopirovat sa da tahanim laveho dolneho rohu okienka
          - vzorec sa automaticky posuva na `=C4+1`, `=D4+1`, atd
      - o riadok nizsie do B5..E5 napiseme hodnotu 100 (okienka
        $A[-4]\dots A[-1]$)
      - do F5 dame 0 (okienko $A[0]$ nasej tabulky)
      - do G5 napiseme vzorec `=MIN(F5+1,D5+1,C5+1)`, t.j. $A[1] =
        min(A[1-1]+1,A[1-3]+1,A[1-4]+1)$
      - tento vzorec potom nakopirujeme do riadku tabulky
      - F5 sa bude posuvat na G5, H5,... a podobne ostatne dva cleny

**Cvičenie:**

  - Ako by sme zmenili na inu mincovu sustavu, napr. 1,2,5?
  - Stiahnite si [subor](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb03/mince.ods) zo stranky predmetu a skuste si tuto zmenu urobit
    

### Zarovnávanie sekvencií v Exceli

  - skusme si dynamicke programovanie pre globalne zarovnanie
    naprogramovat v Exceli
  - budeme postupovat podobne ako pri minciach, ale potrebujeme dve
    specialne funkcie: `MID(text,od,dlzka)` z textu vyberie urcitu cast.
    Pomocou toho si vstupny text rozdelime na jednotlive pismena, ktore
    si napiseme do zahlavia tabulky
  - vsimnite si pouzivanie dolarov v nazvoch policok: ak je pred menom
    stlpca alebo riadku $, tento sa neposuva, ked vzorec kopirujem do
    inych policok
  - `IF(podmienka,hodnota1,hodnota2)` vyberie bud hodnotu 1 ak je
    podmienka splnena alebo hodnotu2 ak nie je. Napr `IF(F$8=$B12, 1, -1)`
    zvoli skore +1 ak sa hodnota v F8 rovna hodnote v B12 a skore -1 ak
    sa nerovnaju.

**Cvičenie:**

  - Zmente tabulku tak, aby skore pre zhody, nezhody a medzery bolo dane
    bunkami B1, B2 a B3 tabulky. Staci zmenit vzorce a policka D9, C10 a
    D10 a nakopirovat do zvysku tabulky. Ake bude skore najlepsieho
    zarovnania sekvencii `AACGTA` a `ACACCTA` ak skore nezhody je -2 a
    medzery -3?
  - Ako treba zmenit vzorce, aby sme pocitali lokalne zarovnanie?
  - Subor najdete
    [tu](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb03/dynprog.ods)

## Jupyter notebooks, Colab

* Jupyter notebook je programátorské prostredie pre prácu v jazyku Python (a iných), ktorý priamo spája program a jeho výsledky, dajú sa pridať rôzne grafy a pod.
* Notebooky sa dajú spúšťať v online prostredí Google Colab bez toho aby ste si niečo inštalovali.
* V Colabe je aj AI asistent, takže je to vhodný spôsob ako začať s programovaním (nebudeme robi5 na tomto predmete).
* Niektoré softvéry sú k dispozícii vo forme notebooku, vyskúšame si teda spustiť už hotový notebook.
* Notebook nižšie obsahuje naprogramované dyamické progrmaovanie na zarovnávanie sekvencií.
* Budeme ho používať aj na domácej úlohe.

{% include notebook.html file="cb-aln1" name="colab" show=1 dot=0 %}
