---
title: "Cvičenia pre biológov: Fylogenetické stromy"
---

* TOC
{:toc}

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
        49.`, zaklikneme `Region: position`, a `Output format: CDS FASTA
        alignment` a stlačíme `Get output`
      - Na ďalšej obrazovke zaklikneme `show nucleotides`, zvolíme `MAF
        table multiz100way` a vyberieme si, ktoré organizmy chceme. V
        našom prípade z primátov zvolíme chimp, rhesus, bushbaby, z
        iných cicavcov mouse, rat, rabbit, pig, cow, dog, elephant a z
        ďalších organizmov opposum, platypus, chicken, stlačíme `Get
        output`.
      - Výstup uložíme do súboru, necháme si iba prvú formu génu
        (`ENST00000308191.11_hg38`), z mien sekvencií zmažeme spoločný
        začiatok (`ENST00000308191.11_hg38`) a celkovo prepíšeme
        skratky druhov na anglické názvy.
      - [Výsledné zarovnanie](../data/cb-phylo2-aln1.fa.txt)
      - Podobný postup sme ešte spravili s génom EFL1, transkript [ENST00000891314.1](https://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr13%3A40931924-41019316), [výsledné zarovnanie](../data/cb-phylo2-aln2.fa.txt)
	

### Strom metódou spájania susedov

  - So zarovnaniami vyššie skúsme zostaviť strom na stránke
    <https://www.ebi.ac.uk/jdispatcher/phylogeny/simple_phylogeny>
      - Distance correction: ako na prednáške, z počtu pozorovaných
        mutácií na evolučný čas - zapneme
      - Exclude gaps: vynechať všetky stĺpce s pomlčkami - radšej nie
      - Clustering method: UPGMA predpokladá molekulárne hodiny,
        spájanie susedov nie
      - P.I.M. vypíš aj maticu vzdialeností (% identity, pred korekciou)
      - Vo výslednom strome by sme mali zmeniť zakorenenie, aby sme mali
        sliepku (chicken) ako outgroup (kliknutím na sliepku a voľbou v menu)

  - Výsledky pre prvé zarovnanie z programu <https://www.phylogeny.fr/alacarte.cgi> , ktorý
    podporuje aj bootstrap:
      - [Výsledok s pôvodným zakorenením](../data/cb-phylo2-aln1.pdf)
      - [Výsledok so správnym
        zakorenením](../data/cb-phylo2-aln1-root.pdf) (chicken =
        outgroup)


  - ["Správny strom"](https://genome-euro.ucsc.edu/images/phylo/hg38_100way.png) v
    nastaveniach Conservation track-u v UCSC browseri podľa článku
    Murphy WJ et al [Resolution of the early placental mammal radiation
    using Bayesian phylogenetics.](https://www.sciencemag.org/cgi/pmidlookup?view=long&pmid=11743200) Science 2001
  - Náš strom má dosť zlých hrán: zlé postavenie hlodavcov, ale aj slona
    a psa. Zlé postavenie hlodavcov môže byť spôsobené [long branch
    attraction](https://en.wikipedia.org/wiki/Long_branch_attraction).
  - Ak chcete skúsiť zostaviť aj zarovnania, treba začať z [nezarovnaných
    sekvencií](../data/cb-phylo2-seq1.fa)

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
  - Dáta nahráme ikonou Upload úplne vľavo hore, v dolnom rade tlačidiel treba dať `Paste/Fetch data`
  - Strom zobrazíme ikonkou grafu alebo cez [phylotree](https://phylotree.hyphy.org/).
  - [Predpočítané výsledky](https://usegalaxy.eu/u/brejova/h/cb-phylo2-musclerapidnj)

