---
title: "Cvičenia pre biológov: Proteíny, nadreprezentácia"
---

* TOC
{:toc}


## Nadreprezentácia

Dokončíme z [minulého cvičenia](./cb-expr.html#nadreprezentácia-cvičenie-pri-počítači)

<!--
Proteín PTPRZ1 si pozrieme v [Uniprote](https://www.uniprot.org/uniprotkb/P23471/entry), ktoré techniky z prednášok vidíme na jeho stránke?
-->

## Sekvenčné motívy, program MEME

  - Vazobne miesta transkripcnych faktorov sa casto reprezentuju ako
    sekvencne motivy
  - Ak mame skupinu sekvencii, mozeme hladat motiv, ktory maju spolocny
  - Znamy program na tento problem je MEME
  - Chodte na stranku <https://meme-suite.org/>
  - Zvolte nastroj MEME a v casti *Input the primary sequences* zvolte
    *Type in sequences* a zadajte [tieto
    sekvencie](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb11/seq.fa)
  - Pozrite si ostatne nastavenia. Co asi robia?
  - Ak server pocita dlho, mozete si pozriet [vysledky](https://compbio.fmph.uniba.sk/vyuka/mbi-data/cb11/MEME.html)

## Kvasinkové transkripčné faktory v SGD

  - Yeast genome database SGD obsahuje pomerne podrobne stranky pre
    jednotlive transkripcne faktory
  - Pozrime si stranku pre transkripcny faktor [GAL4](https://www.yeastgenome.org/locus/S000006169/regulation)
  - A tiez podstranku pre [vazobny motif](http://yetfasco.ccbr.utoronto.ca/MotViewLong.php?PME_sys_qf2=1510)
  - Mozeme si najst gen G4 aj v databaze [JASPAR](https://jaspar2020.genereg.net/)

## PSI BLAST a Pfam

  - Budeme uvažovať tri vzdialene podobné enzýmy
      - Bis(5'-adenosyl)-triphosphatase (FHIT) u človeka, accession
        P49789 ([Uniprot](https://www.uniprot.org/uniprot/P49789))
      - Adenosine 5'-monophosphoramidase HNT1 u kvasinky Saccharomyces
        cerevisiae, ([Uniprot](https://www.uniprot.org/uniprot/Q9BX68))
      - Galactose-1-phosphate uridylyltransferase u baktérie Haemophilus
        influenzae (GAL-1-P)
        ([Uniprot](https://www.uniprot.org/uniprot/P31764))
      - FHIT a HNT1 majú doménu HIT
        ([Pfam](https://www.ebi.ac.uk/interpro/entry/pfam/PF01230/)).
      - GAL-1-P má domény
        [GalP\_UDP\_tr\_C](https://www.ebi.ac.uk/interpro/entry/pfam/PF02744/)
        a
        [GalP\_UDP\_transf](https://www.ebi.ac.uk/interpro/entry/pfam/PF01087/).
        Tieto domény patria v databáze Pfam do toho istého
        [klanu](https://www.ebi.ac.uk/interpro/set/pfam/CL0265/) ako
        HIT.
  - Pozrime si doménu HIT na stránke databázy [Interpro](https://www.ebi.ac.uk/interpro/entry/pfam/PF01230/), hlavne časť
    Profile HMM


  - Skúsme nájsť podobnosť medzi týmito proteínmi v BLASTe:
    <https://blast.ncbi.nlm.nih.gov/> v časti proteíny, zvoľme databázu
    **Swissprot**, ako Query zadajme Accession proteínu FHIT **P49789**,
    spustime program **PSI-BLAST**, E-value zvýšená na **0.1**.
  - V prvom kole PSI-BLAST spúšťa bežný BLASTP
  - Vidíte medzi výsledkami HNT1 a GAL-1-P? S akou E-hodnotou?
  - Spustíme teraz druhú iteráciu PSI-BLAST, ktorá zostaví profil z
    proteínov s nízkou E-hodnotou v prvej iterácii
  - Ako sa zmenili výsledky pre HNT1 a GAL-1-P?


  - Ak by výpočet dlho trval, výsledky sú tu:
      - [1.
        kolo](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Get&RID=MGF6WZTN016)
      - [2.
        kolo](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Get&RID=MGFDET28013)

## FoldSeek

* Pre zadaný proteín hľadá podobné 3D štruktúry
* Choďte na stránku <https://search.foldseek.com/>
* Zvoľte `Load accession`, zadajte PDB identifikátor 1FHI pre ľudský FHIT používaný aj vyššie
* Vo výsledkoch zvoľte záložku Swissprot
* Vidíte vo výsledkoch naše dva proteiny?
  * HNT1 (pod názvom Hit family protein 1) z Saccharomyces cerevisiae
  * Galactose-1-phosphate uridylyltransferase z  Haemophilus influenzae
* V pravom stĺpci si môžete pozrieť zarovnané štruktúry. Dá sa prepnúť, aby zobrazil celú štruktúru, nielen zarovnanú časť.

