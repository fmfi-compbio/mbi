---
title: "Cvičenia pre biológov: Proteíny"
---

* TOC
{:toc}


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
    Profile HMM.
  - Takisto si pozrieme, ako sú domény viditeľné v Uniprot pre [FHIT](https://www.uniprot.org/uniprot/P49789).


  - Skúsme nájsť podobnosť medzi týmito proteínmi v BLASTe:
    <https://blast.ncbi.nlm.nih.gov/> v časti proteíny, zvoľme databázu
    **Swissprot**, ako Query zadajme Accession proteínu FHIT **P49789**,
    spustime program **PSI-BLAST**, E-value zvýšená na **0.1**.
  - V prvom kole PSI-BLAST spúšťa bežný BLASTP
  - Vidíte medzi výsledkami naše dva proteíny? S akou E-hodnotou?
    - HNT1 zo *Saccharomyces cerevisiae*
    - GAL-1-P z *Haemophilus influenzae*
  - Spustíme teraz druhú iteráciu PSI-BLAST, ktorá zostaví profil z
    proteínov s nízkou E-hodnotou v prvej iterácii
  - Ako sa zmenili výsledky pre HNT1 a GAL-1-P?

<!--
  - Ak by výpočet dlho trval, výsledky sú tu:
      - [1.
        kolo](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Get&RID=N2UHJWVB013)
      - [2.
        kolo](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Get&RID=N2UMV18C016)
-->

## FoldSeek

* Pre zadaný proteín hľadá podobné 3D štruktúry
* Choďte na stránku <https://search.foldseek.com/>
* Zvoľte `Load accession`, zadajte PDB identifikátor 1FHI pre ľudský FHIT používaný aj vyššie
* Vo výsledkoch zvoľte záložku Swissprot
* Vidíte vo výsledkoch naše dva proteiny?
  * HNT1 (pod názvom Adenosine 5'-monophosphoramidase) zo *Saccharomyces cerevisiae*
  * Galactose-1-phosphate uridylyltransferase z *Haemophilus influenzae*
* V pravom stĺpci si môžete pozrieť zarovnané štruktúry. Dá sa prepnúť, aby zobrazil celú štruktúru, nielen zarovnanú časť.

## Alphafold

