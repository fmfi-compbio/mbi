---
title: UCSC Genome browser, Quast, Galaxy
---

## Používanie počítačov v M 217

  - V textovom menu pri štarte zvoľte Linux, v prihlasovacom menu
    zadajte užívatela bioinf, heslo dostanete
  - Na dolnom okraji obrazovky je lišta s často používanými nástrojmi,
    napr. internetový prehliadač Firefox
  - Vo Firefoxe si otvorte stránku predmetu
    <https://fmfi-compbio.github.io/mbi/> časť Materials,
    nalistujte materiály k dnešnému cvičeniu

## UCSC genome browser

  - On-line grafický nástroj na prezeranie genómov
  - Konfigurovateľný, veľa možností, ale pomerne málo organizmov
  - V programe Firefox choďte na stránku UCSC genome browser
    <http://genome-euro.ucsc.edu/> (európsky mirror stránky
    <http://genome.ucsc.edu/> )
  - Hore v modrom menu zvoľte Genomes, potom zvoľte ľudský genóm verzia
    hg38. Do okienka `search term` zadajte HOXA2. Vo výsledkoch hľadania
    (Gencode genes) zvoľte gén homeobox A2 na chromozóme 7.
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


  - **Koľko má HOXA2 exónov? Na ktorom chromozóme a pozícii je? Pozor,
    je na opačnom vlákne. Ako je táto skutočnosť naznačená na obrázku?**
  - V tracku GENCODE kliknite na gén, mali by ste sa dostať na stránku
    popisujúcu jeho rôzne vlastnosti. **Čo ste sa dozvedeli o jeho
    funkcii?**
      - Na tejto stránke nájdite linku na stiahnutie proteínovej
        sekvencie. **Aké sú prvé štyri aminokyseliny?**

## Sekvenovanie v UCSC genome browseri

  - Vráťte sa na UCSC genome browser <http://genome-euro.ucsc.edu/>
  - Pozrieme si niekoľko vecí týkajúcich sa sekvenovania a skladania
    genómov
  - Hore v modrom menu zvoľte Genomes, časť Other
  - Na ďalšej stránke zvoľte človeka a pomocou menu Human Assembly
    **zistite, kedy boli pridané posledné tri verzie ľudského genómu
    (hg19, hg38, hs1)**
  - Na tej istej stránke dole nájdete stručný popis zvolenej verzie
    genómu. **Pre ktoré oblasti genómu máme v hg38 najviac
    alternatívnych verzií?**
  - Prejdite na región chr21:31,250,000-31,300,000 v hg19 [touto linkou](http://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg19&position=chr21%3A31250000-31300000).
  - Zapnite si tracky Mapability a RepeatMasker na "full"
  - Mapability: nakoľko sa daný úsek opakuje v genóme a či teda vieme
    jednoznačne jeho čítania namapovať pri použití Next generation
    sequencing
  - Ako a prečo sa pri rôznych dĺžkach čítaní líšia? (Keď kliknete na
    linku "Mapability", môžete si prečítať bližšie detaily.)
  - Približne v strede zobrazeného regiónu je pokles mapovateľnosti.
    **Akému typu opakovania zodpovedá?** (pozrite track RepeatMasker)
  - Pozrite si región
    chr2:110,000,000-110,300,000 v hg19
    [touto linkou](http://genome-euro.ucsc.edu/cgi-bin/hgTracks?db=hg19&position=chr2%3A110000000-110300000), zapnite si tracky "Assembly" a "Gaps".
    **Aká dlhá je neosekvenovaná medzera (gap) v strede tohto regiónu?**
    Približnú veľkosť môžete odčítať z obrázku, presnejší údaj zistíte
    kliknutím na čierny obdĺžnik zodpovedajúci tejto medzere (úplne
    presná dĺžka aj tak nebola známa, nakoľko nebola osekvenovaná).
  - Cez menu položku View, In other genomes si pozrite, ako zobrazený
    úsek vyzerá vo verzii hg38 and hs1. Ako sa zmenila dĺžka z
    pôvodných 300kb?

## QUAST: program na štatistiky o kvalite poskladania genómu

  - Čítania technológie Illumina MiSeq z 500kbp oblasti genómu E.coli
  - Boli poskladané programom [SPAdes](https://github.com/ablab/spades)
  - Vzniknuté [kontigy](http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb01/spades.fasta)
  - Pozrime si štatistiky tohto poskladania v nástroji Quast,
      - [Predpočítané
        výsledky](http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb01/quast.html)
        a [report v pdf
        formáte](http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb01/quast.pdf)
  - Teraz si tento výpočet spustíme v systéme Galaxy

## Prehľad systému Galaxy

  - <https://usegalaxy.eu/>
  - Obsahuje veľa bioinformatických nástrojov, ktoré môžete spúšťať
  - Ale na výsledky treba niekedy dlho čakať
  - V ľavom stĺpci hľadanie nástroja alebo nahrávanie dát
  - V pravom stĺpci zoznam nahratých dát, bežiacich programov a hotových
    výsledkov (výsledky si pozriete ikonou oka alebo stiahnete ikonou
    diskety)
  - V strede nastavenia nástroja alebo prezeranie výsledkov
  - Pri serióznom používaní odporúčam vytvoriť si konto a prihlásiť sa

  - Stiahnite si [kontigy](http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb01/spades.fasta),
    uložte ako súbor
  - V ľavom menu zvolíme Upload Data a nahráme stiahnutý súbor
  - V časti Tools v ľavom menu zadáme do vyhľadávania Quast, zvolíme
    Quast
      - Ako Contigs/scaffolds file zadáme nahratý súbor, ostatné položky
        necháme predvolené, stlačíme Execute
      - [Predpočítané výsledky](https://usegalaxy.eu/u/brejova/h/quast)
  - Druhá analýza: porovnanie poskladaných kontigov so správnou
    odpoveďou (ak je známa)
      - [Skutočná E.coli
        sekvencia](http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb01/ref.fasta),
        ktorú sme chceli dostať
      - Dá sa zadať do nástroja Quast, ak zvolíte Yes v `Use a reference
        genome` a tento súbor nahráte ako `Reference genome`

Ďalšie dáta pre záujemcov:

  - Použité čítania: [prvé čítania z
    páru](http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb01/miseq_R1.fastq.gz),
    [druhé čítania z
    páru](http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb01/miseq_R2.fastq.gz)
  - Galaxy obsahuje aj program SPAdes na skladanie

