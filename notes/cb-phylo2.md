---
title: "Cvičenia pre biológov: Fylogenetické stromy"
---

* TOC
{:toc}


## Uniprot pre projekty

  - Prehľadný pohľad na proteíny, veľa linkov na iné databázy, časť
    vytváraná ručne
  - Pozrieme sa na známy koronavírusový proteín Spike
      - Nájdime ho na stránke <https://www.uniprot.org/> pod názvom
        SPIKE\_SARS2
      - Pozrime si jeho stránku (vrátime sa k nej po prednáške o proteínoch)

## Praktická ukážka tvorby stromov

### Viacnásobné zarovnania z UCSC browsera

  - V UCSC browseri môžeme získavať viacnásobné zarovnania jednotlivých
    génov (nukleotidy alebo proteíny). Nasledujúci postup nemusíte
    robiť, súbor je nižšie
      - V UCSC browseri si pozrieme úsek ľudského genómu (verzia hg38)
        [chr6:135,851,998-136,191,840](https://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr6%3A135851998-136191840) s génom PDE7B (phosphodiesterase
        7B)
      - Na modrej lište zvolíme `Tools`, `Table browser`. V nastaveniach
        tabuliek Group: `Genes and Gene Predictions`, Track: `All GENCODE v
        32.`, zaklikneme `Region: position`, a `Output format: CDS FASTA
        alignment` a stlačíme `Get output`
      - Na ďalšej obrazovke zaklikneme `show nucleotides`, zvolíme `MAF
        table multiz100way` a vyberieme si, ktoré organizmy chceme. V
        našom prípade z primátov zvolíme chimp, rhesus, bushbaby, z
        iných cicavcov mouse, rat, rabbit, pig, cow, dog, elephant a z
        ďalších organizmov opposum, platypus, chicken, stlačíme `Get
        output`.
      - Výstup uložíme do súboru, necháme si iba prvú formu génu
        (`ENST00000308191.11_hg38`), z mien sekvencií zmažeme spoločný
        začiatok (`ENST00000308191.11_hg38`), prípadne celkovo prepíšeme
        mená na anglické názvy.

### Strom metódou spájania susedov

  - Otvorte si [zarovnanie](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb06/cb06-aln.fa), ktoré vzniklo postupom vyššie s ručne upravenými menami sekvencií 
  - Skúsme zostaviť strom na stránke
    <https://www.ebi.ac.uk/jdispatcher/phylogeny/simple_phylogeny>
      - Distance correction: ako na prednáške, z počtu pozorovaných
        mutácií na evolučný čas
      - Exclude gaps: vynechať všetky stĺpce s pomlčkami
      - Clustering method: UPGMA predpokladá molekulárne hodiny,
        spájanie susedov nie
      - P.I.M. vypíš aj maticu vzdialeností (% identity, pred korekciou)
      - Vo výslednom strome by sme mali zmeniť zakorenenie, aby sme mali
        sliepku (chicken) ako outgroup

  - Výsledky z programu <https://www.phylogeny.fr/alacarte.cgi> , ktorý
    podporuje aj bootstrap:
      - [Výsledok s pôvodným zakorenením](https://compbio.fmph.uniba.sk/vyuka/mbi/images/4/41/Cb06-aln.pdf)
      - [Výsledok so správnym
        zakorenením](https://compbio.fmph.uniba.sk/vyuka/mbi/images/2/24/Cb06-aln-root.pdf) (chicken =
        outgroup)

  - ["Správny strom"](https://genome-euro.ucsc.edu/images/phylo/hg38_100way.png) v
    nastaveniach Conservation track-u v UCSC browseri podľa článku
    Murphy WJ et al [Resolution of the early placental mammal radiation
    using Bayesian phylogenetics.](https://www.sciencemag.org/cgi/pmidlookup?view=long&pmid=11743200) Science 2001
  - Náš strom má dosť zlých hrán: zlé postavenie hlodavcov, ale aj slona
    a psa. Zlé postavenie hlodavcov môže byť spôsobené [long branch
    attraction](https://en.wikipedia.org/wiki/Long_branch_attraction).
  - Ak chcete skúsiť zostaviť aj zarovnania, treba začať z [nezarovnaných
    sekvencií](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb06/cb06-seq.fa).

Zostavili sme strom aj IQ-TREE pomocou metódy maximálnej vierohodnosti na Galaxy, viď nižšie
- [modely](https://github.com/Cibiv/IQ-TREE/wiki/Substitution-Models) v IQ-TREE
- [výsledok](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb06/cb06-iqtree.txt)
- na strom v newickovom formáte (zátvorky) použijeme vizualizáciu stromov, napr. <https://phylotree.hyphy.org/> aby sme ho mohli správne zakoreniť

### Stromy na Galaxy

Webstránka s veľa nástrojmi <https://usegalaxy.eu/>

  - Obsahuje veľa bioinformatických nástrojov, ktoré môžete spúšťať
  - Ale na výsledky treba niekedy dlho čakať
  - V ľavom stĺpci hľadanie nástroja alebo nahrávanie dát
  - V pravom stĺpci zoznam nahratých dát, bežiacich programov a hotových
    výsledkov (výsledky si pozriete ikonou oka alebo stiahnete ikonou
    diskety)
  - V strede nastavenia nástroja alebo prezeranie výsledkov
  - Pri serióznom používaní odporúčam vytvoriť si konto a prihlásiť sa

Pre ďalšie pokusy: nezarovnané sekvencie proteínov z rôznych organizmov:

  - [Sekvencie](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb06/cb06-prot.fa)
  - Nájdené pomocou BLAST v Uniprote ako homológy [proteínu YCF1](https://www.uniprot.org/uniprotkb/P39109/entry) z S. cerevisiae
  - Zarovnáme na Galaxy pomocou muscle, strom spravíme cez rapidnj alebo
    IQ-tree
  - Zobrazíme kliknutím na ikonku visualize alebo cez [phylotree](https://phylotree.hyphy.org/).

## Gény, evolúcia a komparatívna genomika v UCSC genome browseri (cvičenie pri počítači)

  - Zobrazme si gén [CLCA4](https://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr1%3A86538658-86589173)
  - Zapnite si štandardnú sadu track-ov (Tlačidlo `Configure` pod
    obrázkom, potom tlačidlo `Default` v druhej sekcii stránky)
  - Po kliknutí na gén si môžete prečítať o jeho funkcii, po kliknutí na
    ľavú lištu alebo na názov tracku v zozname na spodku stránky si
    môžete prečítať viac o tracku a meniť nastavenia.
  - V tracku RefSeq genes si všimnite, že v tejto databáze má tento gén
    dve formy zostrihu, jedna z nich sa považuje za nekódujúcu, pretína
    sa aj s necharakterizovanou nekódujúcou RNA na opačnom vlákne.
      - Track RefSeq a jeho subtrack RefSeq Curated treba zapnúť na pack.
  - Nižšie vidíte track H3K27Ac Mark (Often Found Near Regulatory
    Elements) on 7 cell lines from ENCODE. Kde bola táto histónová
    modifikácia v okolí génu detegovaná?
  - Všimnite si aj track ENCODE Candidate Cis-Regulatory Elements.
    Všimnite si jeho súvis s H3K27Ac trackom. Čo znamenajú farby v
    tomto tracku?


  - Všimnime si track Vertebrate Multiz Alignment & Conservation (100
    Species)
      - v spodnej časti tracku vidíme zarovnania s rôznymi inými
        genómami
      - v nastaveniach tracku zapnite Element Conservation (phastCons)
        na full a Conserved Elements na dense
      - v tomto tracku vidíme PhyloP, čo zobrazuje úroveň konzervovanosti
        danej bázy len na základe jedného stĺpca zarovnania a dva
        výsledky z phyloHMM phastCons, ktorý berie do úvahy aj okolité
        stĺpce
  - Konkrétne časť Conserved elements zobrazuje konkrétne úseky, ktoré
    sú najviac konzervované
      - Ak chceme zistiť, koľko percent genómu tieto úseky pokrývajú,
        ideme na modrej lište do časti Tools-\>Table browser, zvolíme
        group Comparative genomics, track Conservation, table 100 Vert.
        El, region zvolíme genome (v celom genóme) a stlačíme tlačidlo
        Summary/statistics, dostaneme niečo takéto:

<TABLE border=1>
<TR><TD>item count</TD><TD ALIGN=RIGHT>10,350,729</TD>
</TR>
<TR><TD>item bases</TD><TD ALIGN=RIGHT>162,179,256 (5.32%)</TD></TR>
<TR><TD>item total</TD><TD ALIGN=RIGHT>162,179,256 (5.32%)</TD></TR>
<TR><TD>smallest item</TD><TD ALIGN=RIGHT>1</TD></TR>
<TR><TD>average item</TD><TD ALIGN=RIGHT>16</TD></TR>
<TR><TD>biggest item</TD><TD ALIGN=RIGHT>3,732</TD></TR>
<TR><TD>smallest score</TD><TD ALIGN=RIGHT>186</TD></TR>
<TR><TD>average score</TD><TD ALIGN=RIGHT>333</TD></TR>
<TR><TD>biggest score</TD><TD ALIGN=RIGHT>1,000</TD></TR>
</TABLE>

  -   - Ak by nás zaujímali iba veľmi dlhé "conserved elements", v Table
        browser stlačíme tlačidlo Filter a na ďalšej obrazovke do
        políčka Free-form query dáme **chromEnd-chromStart\>=1500**
      - Potom môžeme skúsiť Summary/Statistics alebo výstup typu
        Hyperlinks to genome browser a Get output - dostaneme zoznam
        týchto elementov a každý si môžeme jedným klikom pozrieť v
        browseri, napr. takéto
          - [lod=24051 at
            chr1:50201403-50203312](https://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr1:50201403-50203312)
          - [lod=1899 at
            chr1:55663689-55667047](https://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr1:55663689-55667047)
            atď


  - Pozrime si teraz ten istý gén CLCA4 v staršej verzii genómu [hg18](https://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg18&position=chr1%3A86776929-86827444)
      - V časti Genes and Gene Prediction Tracks zapnite track Pos Sel
        Genes, ktorý obsahuje gény s **pozitívnym výberom** (červenou,
        prípadne slabšie fialovou a modrou)
      - Keď kliknete na červený obdĺžnik pre tento gén, uvidíte, v
        ktorých častiach fylogenetického stromu bol detegovaný pozitívny
        výber
      - Po [priblížení](https://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg18&position=chr1%3A86805823-86805917) do jedného z exónov
        vidíte dôsledky nesynonymných mutácií

Poznámka: Existujú aj webservery na predikciu pozitívneho výberu,
napríklad tieto dva (v súčasnosti Selecton asi nefunguje):

  - [Selecton](https://selecton.tau.ac.il/),
    [článok](https://www.tau.ac.il/~talp/publications/selecton2007.pdf)
  - [Data monkey](https://www.datamonkey.org/),
    [článok](https://mbe.oxfordjournals.org/cgi/content/abstract/22/5/1208)
  - Skúsili sme na Selecton poslať [CLCA4 zo 7 cicavcov](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb07/clca4.mfa)
      - [výsledky](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb07/clca4-selecton.html)
        a
        [ich pokračovanie](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb07/clca4-omega.txt)
        (metóda ale odporúča aspoň 10 homologov)
  - Nástroj HyPhy
      - [výber metódy](https://hyphy.org/getting-started/#characterizing-selective-pressures)
      - niektoré HyPhy nástroje sa nachádzajú v Galaxy

## Objavenie génu HAR1 pomocou komparatívnej genomiky

  - [pdf](https://ribonode.ucsc.edu/Pubs/Pollard_etal06.pdf)
  - Zobrali všetky regióny dĺžky aspoň 100bp s \> 96% podobnosťou medzi
    šimpanzom a myšou/potkanom (35,000)
  - Porovnali s ostatnými cicavcami, zistili, ktoré majú veľa mutácií v
    človeku, ale málo inde (pravdepodobnostný model)
  - 49 štatisticky významných regiónov, 96% nekódujúcich oblastiach
  - Najvýznamnejší HAR1: 118nt, 18 substitúcií u človeka, očakávali by
    sme 0.27. Iba 2 zmeny medzi šimpanzom a sliepkou (310 miliónov
    rokov), ale nebol nájdený v rybách a žabe.
  - Nezdá sa byť polymorfný u človeka
  - Prekrývajúce sa RNA gény HAR1A a HAR1B
  - HAR1A je exprimovaný v neokortexe u 7 a 9 týždňových embryí, neskôr
    aj v iných častiach mozgu (u človeka aj iných primátov)
  - Všetky substitúcie v človeku A/T-\>C/G, stabilnejšia RNA štruktúra
    (ale tiež sú blízko k telomére, kde je viacej takýchto mutácií kvôli
    rekombinácii a javu biased gene conversion)

### Cvičenie pri počítači

  - Môžete si pozrieť tento región v browseri:
    [chr20:63102114-63102274
    (hg38)](https://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr20%3A63102114-63102274),
    pričom ak sa ešte priblížite, uvidíte zarovnanie aj s bázami a
    môžete vidieť, že veľa zmien je špecifických pre človeka

