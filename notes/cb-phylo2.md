---
title: "Cvičenia pre biológov: Fylogenetické stromy"
---


## Praktická ukážka tvorby stromov

### Viacnásobné zarovania z UCSC browsera

  - V UCSC browseri mozeme ziskavat viacnasobne zarovnania jednotlivych
    genov (nukleotidy alebo proteiny). Nasledujuci postup nemusite
    robit, subor si stiahnite tu:
    <http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb06/cb06-aln.fa>
      - UCSC browseri si pozrieme usek ludskeho genomu (verzia hg38)
        chr6:135,851,998-136,191,840 s genom PDE7B (phosphodiesterase
        7B)
        [1](http://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr6%3A135851998-136191840)
      - Na modrej liste zvolime Tools, Table browser. V nastaveniach
        tabuliek Group: Genes and Gene Predictions, Track: GENCODE v
        32., zaklikneme Region: position, a Output fomat: CDS FASTA
        alignment a stlacime Get output
      - Na dalsej obrazovke zaklikneme show nucleotides, zvolime MAF
        table multiz100way a vyberieme si, ktore organizmy chceme. V
        nasom pripade z primatov zvolime chimp, rhesus, bushbaby, z
        inych cicavcov mouse, rat, rabbit, pig, cow, dog, elephant a z
        dalsich organizmov opposum, platypus, chicken, stlacime Get
        output.
      - Vystup ulozime do suboru, nechame si iba prvu formu genu
        (ENST00000308191.11\_hg38), z mien sekvencii zmazeme spolocny
        zaciatok (ENST00000308191.11\_hg38), pripadne celkovo prepiseme
        mena na anglicke nazvy

### Strom metódou spájania susedov

  - Skusme zostavit strom na stranke
    <http://www.ebi.ac.uk/Tools/phylogeny/clustalw2_phylogeny/>
      - Distance correction: ako na prednáške, z počtu pozorovaných
        mutácií na evolučný čas
      - Exclude gaps: vynechať všetky stĺpce s pomlčkami
      - Clustering method: UPGMA predpokladá molekulárne hodiny,
        spájanie susedov nie
      - P.I.M. vypíš aj maticu vzdialeností (% identity, pred korekciou)
      - Vo vyslednom strome by sme mali zmenit zakorenenie, aby sme mali
        sliepku (chicken) ako outgroup

  - Výsledky z programu <http://www.phylogeny.fr/alacarte.cgi> , ktorý
    podporuje aj bootstrap:
      - [Vysledok s povodnym zakorenenim](./Media:Cb06-aln.pdf.md)
      - [Vysledok so spravnym
        zakorenenim](./Media:Cb06-aln-root.pdf.md) (chicken =
        outgroup)

  - "Spravny strom"
    [2](http://genome-euro.ucsc.edu/images/phylo/hg38_100way.png) v
    nastaveniach Conservation track-u v UCSC browseri (podla clanku
    Murphy WJ et al Resolution of the early placental mammal radiation
    using Bayesian phylogenetics. Science 2001
    [3](http://www.sciencemag.org/cgi/pmidlookup?view=long&pmid=11743200))
  - Nas strom ma dost zlych hran: zle postavenie hlodavcov, ale aj slona
    a psa. Zle postavenie hlodavcov môže byť spôsobené [long branch
    attraction](https://en.wikipedia.org/wiki/Long_branch_attraction).
  - Ak chcete skusit zostavit aj zarovnania, treba zacat z nezarovnanych
    sekvencii:
    [4](http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb06/cb06-seq.fa)

### Stromy na Galaxy

Webstránka s veľa nástrojmi <https://usegalaxy.eu/>

  - na tvorbu stromov sa dá použiť IQ-TREE
      - modely vid tu:
        <https://github.com/Cibiv/IQ-TREE/wiki/Substitution-Models>
      - vysledok [CB:phylo](./CB:phylo.md)
  - viewer napr. <http://phylotree.hyphy.org/>

Pre dalsie pokusy: nezarovnane sekvencie proteinov z roznych organizmov:

  - [Sekvencie](http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb06/cb06-prot.fa)
  - Nájdené pomocou BLAST v Uniprote ako homology proteínu YCF1 z S.
    cerevisiae [uniprot](https://www.uniprot.org/uniprotkb/P39109/entry)
  - Zarovnáme na Galaxy pomocou muscle, strom spravíme cez rapidnj alebo
    IQ-tree
  - Zobrazíme kliknutím na ikonku visualize alebo cez phylotree viewer

## Gény, evolúcia a komparatívna genomika v UCSC genome browseri (cvičenie pri počítači)

  - Zobrazme si gén CLCA4
    [5](http://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr1%3A86538658-86589173)
  - Zapnite si štandardnú sadu track-ov (Tlačidlo Configure pod
    obrázkom, potom tlačidlo Default v druhej sekcii stránky)
  - Po kliknutí na gén si môžete prečítať o jeho funkcii, po kliknutí na
    ľavú lištu alebo na názov tracku v zozname na spodku stránky si
    môžete prečítať viac o tracku a meniť nastavenia
  - V tracku RefSeq genes si všimnite, že v tejto databáze má tento gén
    dve formy zostrihu, jedna z nich sa považuje za nekódujúcu, pretína
    sa aj s necharakterizovanou nekódujúcou RNA na opačnom vlákne
      - Track RefSeq a jeho subtrack RefSeq Curated treba zapnut na pack
  - Nižšie vidíte track H3K27Ac Mark (Often Found Near Regulatory
    Elements) on 7 cell lines from ENCODE. Kde bola táto histónová
    modifikácia v okolí génu detegovaná?
  - Všimnite si aj track ENCODE Candidate Cis-Regulatory Elements.
    Všimnite si jeho súvis s H3K27Ac trackom. Čo znamenajú farby v
    tomto tracku?

<!-- end list -->

  - Vsimnime si track Vertebrate Multiz Alignment & Conservation (100
    Species)
      - v spodnej casti tracku vidime zarovnania s roznymi inymi
        genomami
      - v nastaveniach tracku zapnite Element Conservation (phastCons)
        na full a Conserved Elements na dense
      - v tomto tracku vidíme PhyloP, co zobrazuje uroven konzerovanosti
        danej bazy len na zaklade jedneho stlpca zarovnania a dva
        vysledky z phyloHMM phastCons, ktory berie do uvahy aj okolite
        stlpce
  - Konkretne cast Conserved elements zobrazuje konkretne useky, ktore
    su najvac konzervovane
      - Ak chceme zistit, kolko percent genomu tieto useky pokryvaju,
        ideme na modrej liste do casti Tools-\>Table browser, zvolime
        group Comparative genomics, track Conservation, table 100 Vert.
        El, region zvolime genome (v celom genome) a stlacime tlacidlo
        Summary/statistics, dostaneme nieco taketo:

<TABLE border=1>

<TR>

<TD>

item count

</TD>

<TD ALIGN=RIGHT>

10,350,729

</TD>

</TR>

<TR>

<TD>

item bases

</TD>

<TD ALIGN=RIGHT>

162,179,256 (5.32%)

</TD>

</TR>

<TR>

<TD>

item total

</TD>

<TD ALIGN=RIGHT>

162,179,256 (5.32%)

</TD>

</TR>

<TR>

<TD>

smallest item

</TD>

<TD ALIGN=RIGHT>

1

</TD>

</TR>

<TR>

<TD>

average item

</TD>

<TD ALIGN=RIGHT>

16

</TD>

</TR>

<TR>

<TD>

biggest item

</TD>

<TD ALIGN=RIGHT>

3,732

</TD>

</TR>

<TR>

<TD>

smallest score

</TD>

<TD ALIGN=RIGHT>

186

</TD>

</TR>

<TR>

<TD>

average score

</TD>

<TD ALIGN=RIGHT>

333

</TD>

</TR>

<TR>

<TD>

biggest score

</TD>

<TD ALIGN=RIGHT>

1,000

</TD>

</TR>

</TABLE>

  -   - Ak by nas zaujimali iba velmi dlhe "conserved elements", v Table
        browser stlacime tlacidlo Filter a na dalsej obrazovke do
        policka Free-form query dame **chromEnd-chromStart\>=1500**
      - Potom mozeme skusit Summary/Statistics alebo vystup typu
        Hyperlinks to genome browser a Get output - dostaneme zoznam
        tychto elementov a kazdy si mozeme jednym klikom pozriet v
        browseri, napr. taketo
          - [lod=24051 at
            chr1:50201403-50203312](http://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr1:50201403-50203312)
          - [lod=1899 at
            chr1:55663689-55667047](http://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr1:55663689-55667047)
            atd

<!-- end list -->

  - Pozrime si teraz ten isty gen CLCA4 v starsej verzii genomu hg18
    [6](http://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg18&position=chr1%3A86776929-86827444)
      - V casti Genes and Gene Prediction Tracks zapnite track Pos Sel
        Genes, ktory obsahuje geny s **pozitivnym vyberom** (cervenou,
        pripadne slabsie fialovou a modrou)
      - Ked kliknete na cerveny obdlznik pre tento gen, uvidite, v
        ktorych castiach fylogenetickeho stromu bol detegovany pozitivny
        vyber
      - Po priblizeni do jedneho z exonov
        [7](http://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg18&position=chr1%3A86805823-86805917)
        vidite dosledky nesynonymnych mutacii

Poznamka: Existuju aj webservery na predikciu pozitivneho vyberu,
napriklad tieto dva (v sucasnosti asi nefunguju):

  - [Selecton](http://selecton.tau.ac.il/),
    [clanok](http://www.tau.ac.il/~talp/publications/selecton2007.pdf)
  - [Data monkey](http://www.datamonkey.org/)
    [clanok](http://mbe.oxfordjournals.org/cgi/content/abstract/22/5/1208)
  - Skusili sme na Selecton poslat CLCA4 zo 7 cicavcov, subor tu:
    [8](http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb07/clca4.mfa)
      - vysledky
        [9](http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb07/clca4-selecton.html)
        a
        [10](http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb07/clca4-omega.txt)
        (metoda ale odporuca aspon 10 homologov)
  - Nastroj HyPhy
      - vyber metody
        [11](http://hyphy.org/getting-started/#characterizing-selective-pressures)
      - niektore HyPhy nastroje sa nachadzaju v Galaxy

## Objavenie génu HAR1 pomocou komparatívnej genomiky

  - [pdf](http://ribonode.ucsc.edu/Pubs/Pollard_etal06.pdf)

  - Zobrali všetky regióny dĺžky aspoň 100bp s \> 96% podobnosťou medzi
    šimpanzom a myšou/potkanom (35,000)

  - Porovnali s ostatnými cicavcami, zistili, ktoré majú veľa mutáci v
    človeku, ale málo inde (pravdepodobnostný model)

  - 49 štatisticky významných regiónov, 96% nekódujúcich oblastiach

  - Najvýznamnejší HAR1: 118nt, 18 substitúcii u človeka, očakávali by
    sme 0.27. Iba 2 zmeny medzi šimpanzom a sliepkou (310 miliónov
    rokov), ale nebol nájdený v rybách a žabe.

  - Nezdá sa byť polymorfný u človeka

  - Prekrývajúce sa RNA gény HAR1A a HAR1B

  - HAR1A je exprimovaný v neokortexe u 7 a 9 týždenných embrií, neskôr
    aj v iných častiach mozgu (u človeka aj iných primátov)

  - Všetky substitúcie v človeku A/T-\>C/G, stabilnejšia RNA štruktúra
    (ale tiež sú blízko k telomére, kde je viacej takýchto mutácii kvôli
    rekombinácii a biased gene conversion)

### Cvičenie pri počítači

  - Môžete si pozrieť tento region v browseri:
    [**chr20:63102114-63102274**
    (hg38)](http://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr20%3A63102114-63102274),
    pricom ak sa este priblizite, uvidite zarovnanie aj s bazami a
    mozete vidiet, ze vela zmien je specifickych pre cloveka
