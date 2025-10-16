---
title: "Cvičenia pre biológov: Zarovnávanie sekvencií"
---

* TOC
{:toc}

## Skórovacie matice

Chceme určiť skórovaciu schému pre zarovnávanie dvoch DNA sekvencií (bez
medzier). Máme dva modely, každý z nich vie vygenerovať 2 zarovnané
sekvencie dĺžky *n*.

**Model R (random)** reprezentuje nezávislé náhodne sekvencie

  - Použijeme naše vrece s guličkami označenými A,C,G,T, pričom guličiek
    označených A je 30%, C 20%, G 20% a T 30%.
  - Vytiahneme guličku, zapíšeme si písmeno, hodíme ju naspäť, zamiešame
    a opakujeme s ďalším písmenom atď až kým nevygenerujeme *n* písmen
    pre jednu sekvenciu a *n* písmen pre druhú
  - Máme jednu sekvenciu ACT a druhú ACC. Aká je pravdepodobnosť, že
    práve tieto sekvencie vygenerujeme v našom modeli R?
  - Nezávislé udalosti pre jednotlivé písmená, t.j.
    $\Pr(X1=A)\cdot \Pr(X2=C)\cdot \Pr(X3=T) \cdot \Pr(Y1=A) \cdot \Pr(Y2=C)\cdot \Pr(Y3=C)$
  - Po dosadení dostaneme
    $0,3\cdot 0,2\cdot 0,3\cdot 0,3\cdot 0,2\cdot 0,2 = 0,000216$
  - Spolu máme v modeli $4^6 = 4096$ možností ako vygenerovať dve DNA
    sekvencie dĺžky 3

**Model H (homolog)** reprezentuje zarovnanie vzájomne súvisiacich
sekvencií

  - máme vrece, v ktorom je napr.
      - po 21% guličiek označených AA, TT
      - po 14% označených CC, GG
      - po 2,4% označených AC, AG, CA, CT, GA, GT, TC, TG
      - po 3,6% označených AT, TA
      - po 1,6% označených CG, GC.
  - Spolu máme 70% guličiek označených rovnakými písmenami, 30% rôznymi
  - *n* krát z vreca vytiahneme guličku a písmená píšeme ako stĺpce
    zarovnania $A_1, A_2,\dots, A_n$.
  - aká je pravdepodobnosť, ze dostaneme ACT zarovnané s ACC?
  - $\Pr(A_1=AA)\cdot \Pr(A_2=CC)\cdot \Pr(A_3=TC) = 0,21\cdot 0,14\cdot 0,024 = 0,0007056$

**Skóre zarovnania** je log Pr(zarovnania v H)/Pr(zarovnania v R), t.j.
log (0,0007056 / 0,000216) = 0,514105 (pre desiatkový logaritmus)

  - kladné skóre znamená, že model H lepšie zodpovedá dátam (zarovnaniu)
    ako model R
  - záporné skóre znamená, že model R lepšie zodpovedá dátam

### Cvičenie pri počítači

  - Stiahnite si súbor nižšie, uložte si ho a otvorte v Exceli /
    OpenOffice / LibreOffice
      - [ODS
        formát](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb05/scoring.ods)
      - [XLSX formát for
        Excel](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb05/scoring.xlsx)
      - [XLSX English
        version](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb05/scoring-en.xlsx)
  - V záložke Matica vyplňte do žltej oblasti vzorce na výpočet
    pravdepodobnosti krátkeho zarovnania, logaritmus pomeru
    pravdepodobnosti a súčet skóre, pričom vo vzorcoch použijete odkazy
    na políčka v riadkoch 9-13, stĺpcoch B a E
  - Súčet skóre by mal byť zhruba rovný desaťnásobku logaritmu pomeru -
    prečo vidíme rozdiely?


  - Potom skúšajte meniť %GC a %identity v horných riadkoch tabuľky a
    pozrite sa, ako to ovplyvní skórovanie. Výsledné skóre zo stĺpca E
    ručne prepíšte (bez formúl) do tabuľky v záložke Výsledky. Prečo
    nastávajú také zmeny ako vidíte?

## Praktické cvičenie pri počítači: dotploty

### Yass a dotploty

  - Program Yass hľadá lokálne zarovnania v sekvenciách DNA, zobrazuje
    ich vo forme dot plotov
  - V novom okne/tabe si otvorte YASS server na adrese
    <https://bioinfo.univ-lille.fr/yass/yass.php>
  - V ďalšom okne si na stránke UCSC genome browseru si zobrazte [oblast
    chr21:9,180,027-9,180,345](https://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr21%3A9180027-9180345) vo verzii hg38 ľudského genómu
      - tento región obsahuje **Alu repeat**. Tieto opakovania tvoria
        cca 10% ľudského genómu, viac ako milión kópií
      - zobrazte si sekvenciu tohto úseku DNA takto: na hornej modrej
        lište zvoľte `View`, potom v podmenu `DNA`, na ďalšej obrazovke
        tlačidlo `get DNA`
  - **DNA sekvenciu Alu opakovania chceme zarovnať samu k sebe programom
    YASS**
      - DNA sekvenciu Alu opakovania skopírujte do okienka "Paste your
        sequences" v stránke Yass-u a dvakrát stlačte tlačidlo `Select`
        vedľa okienka
      - Nižšie v časti `Selected DNA sequence(s)` by sa vám malo v oboch
        riadkoch objaviť `Pasted file 1`
      - Nižšie v časti `Parameters` zvoľte `E-value threshold` 0.01 a
        stlačte "Run YASS"
      - Vo výsledkoch si pozrite Dotplot, **čo z neho viete usúdiť o
        podobnosti jednotlivých častí Alu opakovania?**
      - Vo výsledkoch si pozrite Raw: blast, **aké sú súradnice
        opakujúcej sa časti a koľko zarovnanie obsahuje
        zhod/nezhod/medzier?** (Pozor, prvé zarovnanie je celá sekvencia
        sama k sebe, druhé je asi to, čo chcete)
  - V genome browseri sa presuňte na [pozíciu chr21:8,552,000-8,562,000](https://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr21%3A8552000-8562000)
    (10kb na chromozóme 21, s niekoľkými výskytmi Alu)
  - **Chceme teraz porovnať tento úsek genómu so sekvenciou Alu pomocou
    YASSu**
      - Ako predtým si stiahnite DNA sekvenciu tohto úseku
      - V YASSe choďte šípkou späť na formulár
      - Skopírujte DNA sekvenciu do okienka
        vpravo (vyznačiť si ju môžete klávesovou kombináciou Ctrl-A
        alebo Select All v menu Edit),
      - V časti formulára Selected DNA sequence(s) stlačte Remove pri
        hornom riadku
      - Pri pravom okienku, kam ste nakopírovali sekvenciu, stlačte
        Select
      - Zase stlačte Run YASS
      - Pozrite si výsledok ako Dotplot, '''koľko opakovaní Alu ste
        našli? Prečo je jedno červené? '''
      - Pozrite si Raw: blast, **na koľko percent sa podobá
        najpodobnejšia a na koľko druhá najpodobnejšia kópia?**

## Dotplot celých kvasinových genómov

  - Na stránke <https://dgenies.toulouse.inra.fr/run> (based on minimap2
    program)
  - Zadáme URL dvoch genómov z NCBI:
      - Candida albicans
        <https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/182/965/GCF_000182965.3_ASM18296v3/GCF_000182965.3_ASM18296v3_genomic.fna.gz>
      - Candida dubliniensis
        <https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/026/945/GCF_000026945.1_ASM2694v1/GCF_000026945.1_ASM2694v1_genomic.fna.gz>
  - Predpočítaný výsledok
    <https://dgenies.toulouse.inra.fr/result/9swN9_20251013211611>
  - Iná dvojica:
      - Magnusiomyces ingens
        <ftp://ftp.ebi.ac.uk/pub/databases/ena/wgs/public/uid/UIDE01.fasta.gz>
        [(web)](https://www.ebi.ac.uk/ena/browser/view/GCA_900497715?show=blobtoolkit)
      - Saprochaete ingens
        <ftp://ftp.ebi.ac.uk/pub/databases/ena/wgs/public/cab/CABVLU01.fasta.gz>
        [(web)](https://www.ebi.ac.uk/ena/browser/view/GCA_902498895)
  - Predpočítaný výsledok <https://dgenies.toulouse.inra.fr/result/AMzJk_20251013212109>

## Príklady praktických programov

Pozrime sa na niekoľko nástrojov, všimnime si, aké poskytujú nastavenia
a čo vypisujú na výstupe, dajme to do súvisu s prednáškami

### Plné dynamické programovanie

  - Balíček emboss, obsahuje programy na klasické dynamické
    programovanie (needle - globálne, water - lokálne), nájdu sa na
    stránke EBI <https://www.ebi.ac.uk/jdispatcher/psa>
  - porovnanie lokálneho a globálneho zarovnania
      - Dva proteiny z rôznych kvasiniek zarovnáme lokálne, globálne a
        globálne s tým, že neplatíme za medzery na koncoch
  - [sekvencie a výsledné zarovnania](./cb-aln-dp.html)
  - vo výsledku si všimnime, koľko má každé z nich %identity, %gaps, a
    kam sa zarovna sekvencia `IRESPLGG` ktorá je na pozícii 29 v prvom a
    30 v druhom proteíne

```
    Lokálne zarovnanie
    Length: 588
    Identity:     170/588 (28.9%)
    Similarity:   270/588 (45.9%)
    Gaps:         116/588 (19.7%)
    Score:  611.0
    MCA_00027_1 29-568 (z 595)
    RKM3_YEAST  30-549 (z 552)
    
    Globálne zarovnanie
    Length: 650
    Identity:     178/650 (27.4%)
    Similarity:   282/650 (43.4%)
    Gaps:         153/650 (23.5%)
    Score: 588.5
```

### NCBI Blast

  - NCBI BLAST <https://blast.ncbi.nlm.nih.gov/> veľa rôznych nástrojov
    (porovnavanie DNA aj proteínov, prípadne translácia DNA na proteín v
    6 rámcoch)
      - Heuristický algoritmus, môže niektoré zarovnania vynechať
      - rôzne nastavenia, výstup E-value

**Low complexity masking:** nepoužívať pri hľadaní jadier zarovnania
regiony, v ktorých sa veľakrát opakuje tá istá aminokyselina

  - Príklad (z učebnice Zvelebil and Baum):

```
    >sp|P04156|PRIO_HUMAN Major prion protein OS=Homo sapiens GN=PRNP PE=1 SV=1
    MANLGCWMLVLFVATWSDLGLCKKRPKPGGWNTGGSRYPGQGSPGGNRYPPQGGGGWGQP
    HGGGWGQPHGGGWGQPHGGGWGQPHGGGWGQGGGTHSQWNKPSKPKTNMKHMAGAAAAGA
    VVGGLGGYMLGSAMSRPIIHFGSDYEDRYYRENMHRYPNQVYYRPMDEYSNQNNFVHDCV
    NITIKQHTVTTTTKGENFTETDVKMMERVVEQMCITQYERESQAYYQRGSSMVLFSSPPV
    ILLISFLIFLIVG
```

  - Hľadajme v databáze `Reference proteins (refseq_protein)`, organizmus `human`, vypneme `Compositional adjustment` (len kvôli tomuto príkladu) aj Masking a Filtering.

BLAST vypíše napr. aj toto zarovnanie:

```
keratin, type I cytoskeletal 9 [Homo sapiens]
Sequence ID: NP_000217.2
Range 1: 498 to 602
Score	          Expect	Identities	Positives	  Gaps
62.0 bits(149)	4e-10	  40/105(38%)	49/105(46%)	6/105(5%)
Query  29   GGWNTGGSRYPGQGSPGGNRYPPQGG----GGWGQPHGGGWGQPHGGGWGQPHGGGWGQP  84
            GG  +GG    G GS GG+     GG    GG G  +GGG G  H GG G  H GG G  
Sbjct  498  GGGGSGGGYGGGSGSRGGSGGSYGGGSGSGGGSGGGYGGGSGGGHSGGSGGGHSGGSGGN  557

Query  85   HGGGWGQGGGTHSQW--NKPSKPKTNMKHMAGAAAAGAVVGGLGG  127
            +GGG G GGG+   +     S+  +   H  G+   G   G  GG
Sbjct  558  YGGGSGSGGGSGGGYGGGSGSRGGSGGSHGGGSGFGGESGGSYGG  602
```

Ak zapneme maskovanie, toto zarovnanie už nenájde, v zarovnaní samého so
sebou sa objavia malé písmená alebo X:

```
    >ref|NP_000302.1|major prion protein preproprotein [Homo sapiens]
    Length=253
    
     Score =   520 bits (1340),  Expect = 0.0, Method: Compositional matrix adjust.
     Identities = 253/253 (100%), Positives = 253/253 (100%), Gaps = 0/253 (0%)
    
    Query  1    MANLGCWMLVLFVATWSDLGLCKKRPKPGGWNTGGSRYPGQGSPGGNRYppqggggwgqp  60
                MANLGCWMLVLFVATWSDLGLCKKRPKPGGWNTGGSRYPGQGSPGGNRYPPQGGGGWGQP
    Sbjct  1    MANLGCWMLVLFVATWSDLGLCKKRPKPGGWNTGGSRYPGQGSPGGNRYPPQGGGGWGQP  60
    
    Query  61   hgggwgqphgggwgqphgggwgqphgggwgqgggTHSQWNKPSKPKTNMKHMagaaaaga  120
                HGGGWGQPHGGGWGQPHGGGWGQPHGGGWGQGGGTHSQWNKPSKPKTNMKHMAGAAAAGA
    Sbjct  61   HGGGWGQPHGGGWGQPHGGGWGQPHGGGWGQGGGTHSQWNKPSKPKTNMKHMAGAAAAGA  120
    
    Query  121  vvgglggymlgsamsRPIIHFGSDYEDRYYRENMHRYPNQVYYRPMDEYSNQNNFVHDCV  180
                VVGGLGGYMLGSAMSRPIIHFGSDYEDRYYRENMHRYPNQVYYRPMDEYSNQNNFVHDCV
    Sbjct  121  VVGGLGGYMLGSAMSRPIIHFGSDYEDRYYRENMHRYPNQVYYRPMDEYSNQNNFVHDCV  180
    
    Query  181  NITIKQHtvttttkgenftetDVKMMERVVEQMCITQYERESQAYYQRGSSMVLFSsppv  240
                NITIKQHTVTTTTKGENFTETDVKMMERVVEQMCITQYERESQAYYQRGSSMVLFSSPPV
    Sbjct  181  NITIKQHTVTTTTKGENFTETDVKMMERVVEQMCITQYERESQAYYQRGSSMVLFSSPPV  240
    
    Query  241  illisfliflivG  253
                ILLISFLIFLIVG
    Sbjct  241  ILLISFLIFLIVG  253
```

## Praktické cvičenie pri počítači: BLAT vs BLAST

### BLAT, chains, nets v UCSC browseri

  - Program BLAT v UCSC browseri <https://genome-euro.ucsc.edu/> rýchlo
    vyhľadáva sekvencie v genóme, ale nevie nájsť slabšie podobnosti
      - Vhodné použitie: zarovnanie mRNA ku genómu, presné určenie
        súradníc nejakej sekvencie, a pod.
  - Net tracky v UCSC genome browseri (predpočítané zarovnania) nám umožňujú prechádzať medzi
    homologickými oblasťami rôznych genómov

### BLAT/BLAST

  - Sekvencia uvedena nižšie vznikla pomocou RT-PCR na ľudských cDNA
    knižniciach
  - Choďte na UCSC genome browser <https://genome-euro.ucsc.edu/> , na
    modrej lište zvoľte BLAT, zadajte túto sekvenciu a hľadajte ju v
    ľudskom genóme. **Akú podobnosť (IDENTITY) má najsilnejší nájdený
    výskyt? Aký dlhý úsek genómu zasahuje? (SPAN).** Všimnite si, že
    ostatné výskyty sú oveľa kratšie.
  - V stĺpci ACTIONS si pomocou Details môžete pozrieť detaily
    zarovnania a pomocou Browser si pozrieť príslušný úsek genómu.
  - V tomto úseku genómu si zapnite track `Vertebrate net` na `full` a
    kliknutím na farebnú čiaru na obrázku pre tento track zistite, **na
    ktorom chromozóme sliepky sa vyskytuje homologický úsek.**
  - Skusme tu istu sekvenciu zarovnat ku genomu sliepky programom Blat:
    stlacte najprv na hornej modrej liste Genomes, zvolte Vertebrates a
    Chicken a potom na hornej liste BLAT. Do okienka zadajte tu istu
    sekvenciu. **Akú podobnosť a dĺžku má najsilnejší nájdený výskyt
    teraz? Na ktorom je chromozóme?**
  - Skúsme to isté v NCBI blaste: Choďte na
    <https://blast.ncbi.nlm.nih.gov/> zvoľte nucleotide blast, database
    others a z menu reference genomic sequence, organism chicken
    (taxid:9031), program blastn
  - **Aká je dĺžka, identity a E-value najlepšieho zarovnania? Na ktorom
    je chromozóme?**

### RT PCR sekvencia z cvičenia vyššie

    AACCATGGGTATATACGACTCACTATAGGGGGATATCAGCTGGGATGGCAAATAATGATTTTATTTTGAC
    TGATAGTGACCTGTTCGTTGCAACAAATTGATAAGCAATGCTTTCTTATAATGCCAACTTTGTACAAGAA
    AGTTGGGCAGGTGTGTTTTTTGTCCTTCAGGTAGCCGAAGAGCATCTCCAGGCCCCCCTCCACCAGCTCC
    GGCAGAGGCTTGGATAAAGGGTTGTGGGAAATGTGGAGCCCTTTGTCCATGGGATTCCAGGCGATCCTCA
    CCAGTCTACACAGCAGGTGGAGTTCGCTCGGGAGGGTCTGGATGTCATTGTTGTTGAGGTTCAGCAGCTC
    CAGGCTGGTGACCAGGCAAAGCGACCTCGGGAAGGAGTGGATGTTGTTGCCCTCTGCGATGAAGATCTGC
    AGGCTGGCCAGGTGCTGGATGCTCTCAGCGATGTTTTCCAGGCGATTCGAGCCCACGTGCAAGAAAATCA
    GTTCCTTCAGGGAGAACACACACATGGGGATGTGCGCGAAGAAGTTGTTGCTGAGGTTTAGCTTCCTCAG
    TCTAGAGAGGTCGGCGAAGCATGCAGGGAGCTGGGACAGGCAGTTGTGCGACAAGCTCAGGACCTCCAGC
    TTTCGGCACAAGCTCAGCTCGGCCGGCACCTCTGTCAGGCAGTTCATGTTGACAAACAGGACCTTGAGGC
    ACTGTAGGAGGCTCACTTCTCTGGGCAGGCTCTTCAGGCGGTTCCCGCACAAGTTCAGGACCACGATCCG
    GGTCAGTTTCCCCACCTCGGGGAGGGAGAACCCCGGAGCTGGTTGTGAGACAAATTGAGTTTCTGGACCC
    CCGAAAAGCCCCCACAAAAAGCCG
