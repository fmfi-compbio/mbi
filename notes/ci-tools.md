---
title: "Cvičenia pre informatikov: Úvod do bioinformatických databáz a on-line nástrojov"
---

* TOC
{:toc}


## NCBI, Genbank, Pubmed, blast

  - National Center for Biotechnology Information
    <http://www.ncbi.nlm.nih.gov/>
  - Zhromazduje verejne pristupne data z molekularnej biologie
  - My vyuzijeme nastroj BLAST na hladanie homolog (podobnych sekvencii)
    - najdime homology nasledujucej sekvencie v genome sliepky (zvoľme
        nucleotide blast, database others a z menu Refseq reference genomes,
        organism chicken (taxid:9031), program blastn)
      - Ide o osekvenovany kusok ludskej mRNA, kde v genome sliepky sme
        nasli homolog, ake ma dlzku, skore, E-value, % zhodnych baz?


```
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
```

### Uniprot

  - Prehladnejsi pohlad na proteiny, vela linkov na ine databazy, cast
    vytvarana rucne
      - Pozrieme sa na známy koronavírusový proteín Spike
      - Nájdime ho na stránke <https://www.uniprot.org/> pod názvom
        SPIKE\_SARS2
      - Pozrime si podrobne jeho stránku, ktoré časti boli predpovedané
        bioinformatickými metódami z prednášky?
      - Všimnime si niektorú Pfam doménu a pozrime si jej stránku

### UCSC genome browser

  - <https://genome-euro.ucsc.edu/>
  - On-line grafický nástroj na prezeranie genómov
  - Konfigurovateľný, veľa možností, prijemne pouzivatelske rozhranie
  - Moznost stiahnut data vhodne na dalsie spracovanie alebo zobrazit
    vlastne data
  - Pomerne málo organizmov
      - doraz hlavne na ludsky genom

**Základy**

  - Adresa <https://genome-euro.ucsc.edu/>
  - Hore v modrom menu zvoľte Genomes, potom zvoľte ľudský genóm. Do
    okienka `search term` zadajte HOXA2. Vo výsledkoch hľadania (UCSC
    genes) zvoľte gén homeobox A2 na chromozóme 7.
      - Pozrime si spolu túto stránku
      - V hornej časti sú ovládacie prvky na pohyb vľavo, vpravo,
        približovanie, vzďaľovanie
      - Pod tým schéma chromozómu, červeným vyznačená zobrazená oblasť
      - Pod tým obrázok vybranej oblasti, rôzne tracky
      - Pod tým zoznam všetkých trackov, dajú sa zapínať, vypínať a
        konfigurovať
      - Po kliknutí na obrázok sa často zobrazí ďalšia informácia o
        danom géne alebo inom zdroji dát
      - V génoch exony hrubé, UTR tenšie, intróny vodorovné čiary
  - Po kliknutí na gén alebo inú časť nejakého tracku väčšinou o ňom
    dostaneme viac informácií. Kliknutim na listu ku tracku (lavy okraj
    obazku) sa dozviete viac o tracku a mozete nastavovat parametre
    zobrazenia

**Komparativna genomika**

  - V casti **multiz alignments** vidite zarovnania k roznym inym
    genomom (da sa zapinat, ze ku ktorym). Mozete si pozriet, ako sa
    uroven zarovnania zmeni ked sa priblizujeme a vzdalujeme (zoom
    in/zoom out).
  - Ked sa priblizite na uroven "base", t.j. zobrazenych cca 100bp, v
    obdlzniku multiz alignment uvidite zarovnanie s homologickym usekom
    v inych genomoch.
  - V casti '''conservation by PhyloP '''vidime graf toho, ako silne su
    zachovane jednotlive stlpce zarovnania
  - Da sa zapnut track Placental Chain/Net a pozriet sa na ktorych
    chromozomoch je ortologicky usek v inych genomoch

**Práca s tabuľkami, sťahovanie anotácií**

  - Položka Tables na hornej lište umožnuje robiť rafinované veci s
    tabuľkami, ktoré obsahujú súradnice génov a pod.
  - Základná vec: vyexportovať napr. všetky gény v zobrazenom výseku v
    niektorom formáte:
      - sequence: fasta súbor proteínov, génov alebo mRNA s rôznymi
        nastaveniami
      - GTF: súradnice
      - Hyperlinks to genome browser: klikacia stránka
  - Namiesto exportu si môžeme pozrieť rôzne štatistiky
  - Zložitejšie: prienik dvoch tabuliek, napr. gény, ktoré sú viac než
    50% pokryté simple repeats
      - V intersection zvolíme group: Variation and repeats, track:
        RepeatMasker, nastavíme records that have at least 50% overlap
        with RepeatMasker
      - V summary/statistics zistíme, kolko ich je v genóme, môžeme si
        ich preklikať cez Hyperlinks to genome browser
